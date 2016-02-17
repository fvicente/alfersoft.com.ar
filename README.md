# AlferSoft Blog

Using **[Minimal Mistakes](http://mmistakes.github.io/minimal-mistakes)** Jekyll template

*First time installation*

(sudo)
- apt-get update
- apt-get install ruby-full rubygems
- gem install jekyll

First time clone
- cd /server/www/alfersoft.com.ar/
- git clone git@bitbucket.org:fvicente/alfersoft.com.ar.git src


*Update posts*

When you add or modify a post, connect to alfersoft.com.ar via SSH.
- cd /server/www/alfersoft.com.ar/src
- git pull
- jekyll build
- cp -R _site/* ../blog
