---
layout:     post
title:      "Innovations"
subtitle:   ""
date:       2019-04-18 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - sharing
    
---

## patent application

   MSD: Magnetic-Stripe Data

https://www.thinkwithgoogle.com/marketing-resources/8-pillars-of-innovation/

Have a mission that matters
Think big but start small
Strive for continual innovation, not instant perfection
Look for ideas everywhere
Share everything
Spark with imagination, fuel with data
Be a platform
Never fail to fail



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

