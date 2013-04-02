---
layout: post
title: "Update Octopress"
date: 2013-04-02 23:04
comments: true
categories: Octopress
---

####Add Octopress Respository
```
$ git remote add octopress https://github.com/imathis/octopress.git
```
####Update Octopress
```
$ git pull octopress master
```

####Do not change to "sync", rollback change to use "push" and "master" 
```
-<<<<<<< HEAD
 rsync_delete   = true
 deploy_default = "push"
-=======
-rsync_delete   = false
-rsync_args     = ""  # Any extra arguments to pass to rsync
-deploy_default = "rsync"
->>>>>>> 75cc24c8d602d509e6b272db983489090595f001
-
 # This will be configured for you when you run config_deploy
 deploy_branch  = "master"
```

####Update bundles
```
$ bundle install
$ rake update_source
$ rake update_style
```

####Preview and push changes
```
$ rake generate
$ git commit -a
$ git push origin master
$ rake deploy
$ rake deploy
```

####Problems
After update Octopress, you can preview new post but can not deploy new posts, you need to 
``$ rake setup_github_pages`` redo initialize 

If you meet ``fatal: ‘octopress’ does not appear to be a git repository`` error, execute ``$ git remote add octopress https://github.com/imathis/octopress.git``