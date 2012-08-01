---
layout: post
title: "Setup up ruby environment for Octopress blog"
date: 2012-08-01 18:08
comments: true
categories: Octopress Ruby
---
* Need to install [Ruby1.9.2](http://files.rubyforge.vm.bytemark.co.uk/rubyinstaller/rubyinstaller-1.9.2-p290.exe)

* Need to install [DevKit](http://cloud.github.com/downloads/oneclick/rubyinstaller/DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe) for windows environment   
After install DevKid(extract to a folder)  
Run devkitvars.bat to update path env or add bin folder to path env  
Can skip to install RVM or RBENV in windows, skip
```
rvm use 1.9.2@rails31
```

* Get Ruby bundle
```
bundle update
```