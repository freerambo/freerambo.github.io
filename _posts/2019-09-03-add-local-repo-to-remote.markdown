---
layout:     post
title:      "git add local repo to remote"
subtitle:   ""
date:       2019-04-18 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - sharing
    
---


1. Create a new repository on GitHub.

2. set up SSH keys

generate or using existing public key - id_rsa.pub
The ssh key location On mac: /Users/XXX/.ssh

3. Initialize the local directory as a Git repository.

$ git init

id_rsa.pub
Add the files in your new local repository. This stages them for the first commit.

$ git add .
# Adds the files in the local repository and stages them for commit. To unstage a file, use 'git reset HEAD YOUR-FILE'.
Commit the files that you've staged in your local repository.

$ git commit -m "First commit"
# Commits the tracked changes and prepares them to be pushed to a remote repository. To remove this commit and modify the file, use 'git reset --soft HEAD~1' and commit and add the file again.

In Terminal, add the URL for the remote repository where your local repository will be pushed.

##  git remote add origin git@github.com:freerambo/springcloud.git
   WM-C02YF7ALJG5J:cloud yuzhu$ git remote -v
   origin  git@github.com:freerambo/springcloud.git (fetch)
   origin  git@github.com:freerambo/springcloud.git (push)

Push the changes in your local repository to GitHub.

$ git push -u origin master
# Pushes the changes in your local repository up to the remote repository you specified as the origin



WM-C02YF7ALJG5J:cloud yuzhu$ git remote -v
origin  git@github.com:freerambo/springcloud.git (fetch)
origin  git@github.com:freerambo/springcloud.git (push)
WM-C02YF7ALJG5J:cloud yuzhu$ git remote rm origin
WM-C02YF7ALJG5J:cloud yuzhu$ git remote -v
WM-C02YF7ALJG5J:cloud yuzhu$ git remote add origin https://github.com/freerambo/springcloud.git
WM-C02YF7ALJG5J:cloud yuzhu$ git remote -v
origin  https://github.com/freerambo/springcloud.git (fetch)
origin  https://github.com/freerambo/springcloud.git (push)



## CompletableFuture usability and unit test

https://stackoverflow.com/questions/41858894/completablefuture-usability-and-unit-test

```java

        doAnswer(
                (InvocationOnMock invocation) -> {
                    ((Runnable) invocation.getArguments()[0]).run();
                    return null;
                }
        ).when(executorService).execute(any(Runnable.class));

        CompletableFuture<APIUsageModel> apiUsageModelCompletableFuture = apiUsageService.saveAsync(expected);



doAnswer(invocation -> CompletableFuture.completedFuture(this.serviceAResponse))
    .when(this.serviceAExecutorService)
    .execute(any());

```


## git stash

git stash
git stash apply
git stash show

https://www.atlassian.com/git/tutorials/saving-changes/git-stash

## RestTemplate for list of Object - Object array

```java
 ResponseEntity<Object[]> responseEntity = api.postForEntity(path, request, Object[].class, headers);

```


Add commit to another branch

You can just git cherry-pick -n <commitid>, and the changes will be applied to your working directory and staged in the index (like git -a), but will not be committed.


---

### END

