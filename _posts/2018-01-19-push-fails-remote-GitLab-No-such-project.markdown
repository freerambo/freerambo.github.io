---
layout:     post
title:      "push fails: remote: GitLab: No such project"
subtitle:   "remote rejected -> master (pre-receive hook declined)"
date:       2018-01-19 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - 技术
    - 招聘
---


## GitLab -  master -> master (pre-receive hook declined)

When I try to push changes to our gitlab I encounter below error: 

```
pre-receive hook declined GitLab: No such project

```

After search and oversee, I found that 
![gitlab protection](/img/post/git-lab-protect.png)


It looks like the problem is that "Developers can push" checkbox is unchecked for "master" branch.

That probably allowed me to do the initial push, but caused all subsequent push attempts to fail.

If we select the checkbox or click unprotect button for 'master' branch, then solved the probem. 

Hope the project owner could configure correctly in order to avoid such kind of problem.  

---

### END

