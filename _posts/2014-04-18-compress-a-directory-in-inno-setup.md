---
uid: 709
title: Compress a directory in inno setup
date: 2014-04-18T21:03:08+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=709
categories:
  - Programming FAQ
  - Software Development
tags:
  - compress
  - directory
  - folder
  - inno
  - pascal
  - setup
  - windows
  - zip
comments: true
excerpt: Inno setup script to zip a directory using Windows built-in compression
---
<a href="http://www.jrsoftware.org/isinfo.php" title="Inno Setup" target="_blank">Inno setup</a> script to zip a directory using Windows built-in compression.

{% highlight delphi %}
function MakeBackup(indir, outfile: String): Boolean;
var
  res: Boolean;
  fso: Variant;
  winShell: Variant;
  f: Variant;
  inObj: Variant;
  outObj: Variant;
begin
  res := FALSE;
  try
    fso := CreateOleObject('Scripting.FileSystemObject');
    winShell := CreateOleObject('shell.application');
    // create a new clean zip archive
    f := fso.CreateTextFile(outfile, TRUE);
    f.write('PK' + Chr(5) + Chr(6) + Chr(0) + Chr(0) + Chr(0) + Chr(0) + Chr(0) + Chr(0) + Chr(0) + Chr(0) + Chr(0) + Chr(0) + Chr(0) + Chr(0) + Chr(0) + Chr(0) + Chr(0) + Chr(0) + Chr(0) + Chr(0));
    f.close;
    inObj := winShell.NameSpace(indir);
    outObj := winShell.NameSpace(outfile);
    outObj.CopyHere(inObj.Items);
    repeat
      Sleep(1000);
    until outObj.Items.Count = inObj.Items.Count;
    res := TRUE;
  except
  end;
  Result := res;
end;

// Sample usage

procedure CurStepChanged(CurStep: TSetupStep);
var
  timestamp: String;
  bkprc: Boolean;
begin
  if CurStep = ssInstall then begin
    timestamp := GetDateTimeString('yyyymmdd_hhnnss', #0, #0);
    bkprc := MakeBackup(ExpandConstant('{app}'), ExpandConstant('{app}') + '\..\' + timestamp + '.zip');
    if bkprc = FALSE then begin
      if MsgBox('Backup failed. Are you sure you want to continue?', mbConfirmation, MB_YESNO) = IDNO then
        Abort();
    end;
  end;
end;
{% endhighlight %}

[See compress.iss on gist](https://gist.github.com/fvicente/c18387adf86654cfdc80)

Inspired by:

<a href="http://stackoverflow.com/questions/19832128/inno-setup-zip-local-files-prior-to-an-update" title="Inno Setup – zip local files prior to an update" target="_blank">Inno Setup – zip local files prior to an update</a>

<a href="http://stackoverflow.com/questions/30211/can-windows-built-in-zip-compression-be-scripted" title="Can Windows' built-in ZIP compression be scripted?" target="_blank">Can Windows built-in ZIP compression be scripted?</a>
