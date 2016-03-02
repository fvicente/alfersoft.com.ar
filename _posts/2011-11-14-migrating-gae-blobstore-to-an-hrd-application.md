---
uid: 296
title: Migrating GAE Blobstore to an HRD application
date: 2011-11-14T00:09:40+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=296
permalink: /2011/11/14/migrating-gae-blobstore-to-an-hrd-application/
categories:
  - Programming FAQ
  - Software Development
tags:
  - app
  - blobinfo
  - blobstore
  - engine
  - GAE
  - google
  - high replication database
  - HRD
  - migrate
  - migration
comments: true
---
[<img src="{{ site.url }}/images/gae-logo.png" alt="Google App Engine" title="google app engine"/>]({{ site.url }}/images/gae-logo.png)

If you are planning to migrate your Google App Engine application to the &#8220;new&#8221; High Replication Database scheme, then you probably know that the Google migration process won&#8217;t handle Blobstore files.

Here you will find some python scripts that will help you to move the Blobstore files from the old application to the new one, and correct all the references in the new database. As always, use at your own risk, don&#8217;t try to do anything without reading and understanding the scripts, otherwise you may loose your data permanently.

<!--more-->

Some considerations:

* We did used these scripts in our very own application, so I can say it works. However, I had to edit the original scripts to make them more &#8220;generic&#8221;, remove my database references and names, etc.
* I will try to describe the steps of the migration, but please don&#8217;t try to do anything before reading the whole article first.
* This is not by any means intended to be a fully automatic migration, in fact, the steps needs to be manually executed one by one.
* **The scripts won&#8217;t migrate big files.** We have migrated files up to 40Mb without problems, but if you have files bigger than that, it will probably fail. For example we tried a 300Mb file unsuccessfully, the reason is that the file needs to be downloaded from the old app to the new, and there is a 1 minute timeout for the app requests, so if the download takes more than that it will fail.

### How does the script works

You will need to put the script in your applications, both the new (HRD) and the old one. It basically exposes four URLs and you will need to access two of them from the HRD application.

By doing this you will download all the blob files from the old application to the new one, the script will create a model that keeps a map between the old and new references.

The final step is to migrate the old references to the new one in the HRD databases.

### Prerequisites

* All the references to blob files in your models must be of type `blobstore.BlobReferenceProperty`, if not you will need to adjust them first.
* Migrate the databases from the old app to the new HRD app using the [standard Google migration method](http://code.google.com/appengine/docs/adminconsole/applicationsettings.html#Migrate_from_Master/Slave_to_High_Replication_Datastore "Google App Engine migrate datastore").

### Steps

* Add the URLs to your application

{% highlight python %}
def main():
    application = webapp.WSGIApplication(
          [
           # ... Your app URLs here ...
           # TODO - REMOVE THIS AFTER MIGRATION
           ('/mig/__getblob/(.+)', GetBlob),
           ('/mig/__getblobkeys/?', GetBlobKeys),
           ('/mig/__migrateblobs/?', MigrateBlobs),
           ('/mig/__migratereferences/?', MigrateBlobReferences),
          ], debug=True)
    run_wsgi_app(application)

if __name__ == '__main__':
  main()
{% endhighlight %}

* Then add the migration code

{% highlight python %}
####
# TEMPORARY BLOBSTORE MIGRATION CODE
####

MODELS = (
    # add your (model, blobinfo_field_name) tuples here
    ("myModel1", "blobinfofield"),
    ("myModel2", "BlobInfo"),
)

class mig_BlobMig(db.Model):
    fname = db.StringProperty()
    blobinfo = blobstore.BlobReferenceProperty()
    origkey = db.StringProperty()

class MigrateBlobReferences(blobstore_handlers.BlobstoreDownloadHandler):
    '''Migrate blob info references'''
    def get(self):
        import models
        tb = dict([(str(blob.origkey), blob.blobinfo) for blob in mig_BlobMig.all()])
        for modelname, field in MODELS:
            to_put = []
            skipped = 0
            for obj in db.GqlQuery('SELECT * FROM %s'%modelname):
                oldkey = getattr(obj, field)
                if oldkey:
                    oldkey = str(oldkey.key())
                if oldkey in tb:
                    setattr(obj, field, tb[oldkey])
                    to_put.append(obj)
                else:
                    skipped += 1
            db.put(to_put)
            self.response.out.write("[%s] - Migrated %d references!<br/>Skipped: %d<br/>\n"%(modelname, len(to_put), skipped))

class MigrateBlobs(blobstore_handlers.BlobstoreDownloadHandler):
    '''Migrate blobs from myoldapp to mynewapp'''
    def get(self):
        import time
        from google.appengine.api import files, urlfetch
        start_time = time.time()
        timed_out = False
        migrated_keys = set([str(blob.origkey) for blob in mig_BlobMig.all()])
        all_blobs = eval(urlfetch.fetch("https://myoldapp.appspot.com/mig/__getblobkeys", deadline=15.0).content)
        all_keys = set(all_blobs.keys())
        missing_keys = list(all_keys-migrated_keys)
        if not missing_keys:
            self.response.out.write("Nothing to migrate!")
            return
        # Download 20 blobs per round
        download_keys = missing_keys[:20]
        MAXSIZE = (10*1024*1024) # 10MB
        for origkey in download_keys:
            blob = all_blobs[origkey]
            url = "https://myoldapp.appspot.com/mig/__getblob/%s"%urllib.quote_plus(origkey)
            blob_path = files.blobstore.create(mime_type=blob["content_type"], _blobinfo_uploaded_filename=blob["filename"])
            fsize = int(blob["size"])
            with files.open(blob_path, 'a') as f:
                for first_byte in range(0, fsize, MAXSIZE):
                    last_byte = (first_byte+MAXSIZE-1)
                    if last_byte>=fsize: last_byte=(fsize-1)
                    bytes_range = "bytes=%d-%d"%(first_byte,last_byte)
                    logging.info("Downloading [%s] range [%s] key=[%s]"%(blob["filename"], bytes_range, origkey))
                    res = urlfetch.fetch(url, deadline=35.0, headers={"Range": bytes_range})
                    f.write(res.content)
                    del res.content
                    del res
                    if (time.time()-start_time > 40.0):
                        timed_out = True
                        break
            if timed_out:
                self.response.out.write("Stopped due to time limit!<br/>")
                break
            files.finalize(blob_path)
            blob_key = files.blobstore.get_blob_key(blob_path)
            blob_info = blobstore.BlobInfo.get(blob_key)
            mig_BlobMig(fname=blob["filename"], blobinfo=blob_info, origkey=origkey).put()
            logging.info("Successfully downloaded [%s]!!!"%(blob["filename"]))
            self.response.out.write("Migrated %s<br/>"%blob["filename"])
        self.response.out.write("<br/>SUCCESS!")

class GetBlobKeys(blobstore_handlers.BlobstoreDownloadHandler):
    '''Retrieve all Blob Keys, as a JSON string'''
    def get(self):
        from simplejson.encoder import JSONEncoder
        blobs = {}
        for blob_info in blobstore.BlobInfo.all():
            blobs[str(blob_info.key())] = {
             "filename":str(blob_info.filename),
             "content_type":str(blob_info.content_type),
             "size":int(blob_info.size),
             "key":str(blob_info.key()),
            }
        self.response.headers["Content-type"] = "application/json"
        self.response.out.write(JSONEncoder().encode(blobs))

class GetBlob(blobstore_handlers.BlobstoreDownloadHandler):
    '''Download a blob given its key'''
    def get(self, blobkey):
        blobkey = str(urllib.unquote(blobkey))
        blob_info = blobstore.BlobInfo.get(blobkey)
        if not blob_info:
            self.error(404)
            return
        self.response.headers["Content-type"] = str(blob_info.content_type)
        self.send_blob(blob_info, save_as=True)

####
# END OF TEMPORARY BLOBSTORE MIGRATION CODE
####
{% endhighlight %}

* Modify the `MODELS` variable adding the models and blob info fields that you need to update
* Replace `myoldapp` URLs with the actual URL of your old application
* Synchronize the code on both apps
* Open a browser and point it to `http://mynewapp.appspot.com/mig/__migrateblobs`. Of course, `mynewapp` must be replaced with your actual HRD application name. This will download the blobs from the old app to the new one. Because there is a timeout limitation you will need to call it many times. **WARNING: You must make only one request at time, never call it from two browsers / tabs at the same time or you will mess the migration database!!!**. So this step must be carefully executed, one request at time and wait for it to finish before calling the next one. Repeat this until you see the message `Nothing to migrate!`. This step can be very slow depending on how many files you have in the blobstore and the size of them, it might take a long time to get all the files downloaded. Be patient!
* Once all the files are copied to the new app, point the browser to `http://mynewapp.appspot.com/mig/__migratereferences`. Should be quick, and it needs to be executed only one time. However doesn&#8217;t harm to call it more than once.
* The last step, remove the migration code and URLs and synchronize the code again. You can also remove the <code>mig_BlobMig</code> database.

Good migration.
