---
layout: post
title: "Contributing to an existing Octopress blog"
date: 2012-08-01 17:45
comments: true
categories: 
---

Take my blog as example _wulliam.github.com_  
You will use following command to get source code (from source branch) and blog content (form master branch)

```
git clone git@github.com:wulliam/wulliam.github.com.git
git checkout source
mkdir _deploy
cd _deploy
git init
git remote add origin git@github.com:wulliam/wulliam.github.com.git
git pull origin master
cd ..
rake new_post["new post to share"]
```

reference to original post(GWF will block this url) [Octopress: Setting up a Blog and Contributing to an Existing One](http://code.dblock.org/octopress-setting-up-a-blog-and-contributing-to-an-existing-one "Octopress: Setting up a Blog and Contributing to an Existing One")