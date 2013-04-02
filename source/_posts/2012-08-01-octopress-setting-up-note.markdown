---
layout: post
title: "Octopress setting up note"
date: 2012-08-01 15:25
comments: true
categories: Octopress github
---
*  Generate SSH Key
```
[[ -f ~/.ssh/id_rsa.pub ]] || ssh-keygen -t rsa
[[ -f ~/.ssh/id_rsa.pub ]] && cat ~/.ssh/id_rsa.pub | xclip
```

*  Past public key to github  
   Open https://github.com/account/ssh  
   Add another public key  

*  Get octopress code
```
git clone git://github.com/imathis/octopress.git wulliam.github.com
```

*  Preare ruby environment
```
bundle update
rake install
```

*  Creat a new post
```
rake new_post["post title"]
```

*  Preivew blog in localhost
```
rake preview
```

*  Open browser to check
```
http://127.0.0.1:4000
```

*  Preare your github project   
   If you have an account name _wulliam_ in github.com, then create a project name _wulliam.github.com_
```
rake setup_github_pages
```
   and input your github project url
```
git@github.com:wulliam/wulliam.github.com.git
```

*  Check in source to github (in source branch)
```
git add .
git commit -m "Initial porject"
git push origin source
```

*  Check in blogs to github (in master branch)
```
rake generate
rake deploy
```
