---
layout:     post
title:      "git branch conflicts "
subtitle:   "git branch  冲突 "
date:       2018-04-30 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - 技术
    - JAVA
    - Git
    
---


## git rebase local code to remote branch and write off any local changes 




```shell


- git status 
- git branch
- git stash
- git branch -a
- git log
- git merge --abort" to abort the merge
- git status
- git fetch
- vim .git/config

WM-C02TX3AGHTD8:cbp yuzhu$ git status
On branch F7785_VTSSGP_dev-6.3.0
nothing to commit, working tree clean

WM-C02TX3AGHTD8:cbp yuzhu$ git branch -a | grep F7785_VTSSGP_dev-6.3.0
* F7785_VTSSGP_dev-6.3.0
  remotes/origin/F7785_VTSSGP_dev-6.3.0
  remotes/origin/F7785_VTSSGP_dev-6.3.0_twalgama
  
  
  git rebase origin/F7785_VTSSGP_dev-6.3.0
  
  git rebase --abort
  
 WM-C02TX3AGHTD8:cbp yuzhu$ git status
 On branch F7785_VTSSGP_dev-6.3.0
 nothing to commit, working tree clean 
  

 git checkout -b F7785_VTSSGP_dev-6.3.0

 git checkout dev-6.2.0
 
 
 WM-C02TX3AGHTD8:cbp yuzhu$ git branch -d F7785_VTSSGP_dev-6.3.0
 Deleted branch F7785_VTSSGP_dev-6.3.0 (was c4ef95a3ca).
 WM-C02TX3AGHTD8:cbp yuzhu$ git checkout -b F7785_VTSSGP_dev-6.3.0 origin/F7785_VTSSGP_dev-6.3.0
 Branch F7785_VTSSGP_dev-6.3.0 set up to track remote branch F7785_VTSSGP_dev-6.3.0 from origin.
 Switched to a new branch 'F7785_VTSSGP_dev-6.3.0'


git pull --rebase


```

##  Syncronizae local branch from remote branch

```shell

git pull origin <local_branch>:<remote_branch>
```





---

### END

