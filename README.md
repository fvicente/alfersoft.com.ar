# AlferSoft Blog

Using **[Minimal Mistakes](http://mmistakes.github.io/minimal-mistakes)** Jekyll template

### Everything as root!

*First time installation*

```
# apt-get update
# apt-get install ruby-full rubygems
# gem install jekyll
# pip install Pygments
# gem install pygments.rb
```

*First time clone*

```
# cd /server/www/alfersoft.com.ar/
# git clone git@bitbucket.org:fvicente/alfersoft.com.ar.git src
# cd src
# gem update
# gem install bundler
# bundle install
```

*Update posts*

```
# cd /server/www/alfersoft.com.ar/src
# git pull
# bundle exec jekyll build
# cp -R _site/* ../www/blog/
# chown -R www-data:www-data ../www/blog
```

### Not root, this is your local machine

*Run local*

```
$ cd ~/workspace/alfersoft.com.ar/   (or wherever you've checked out the code)
$ jekyll build
$ jekyll serve
```

