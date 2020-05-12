---
layout:     post
title:      "Unit test for Google guave cahce"
subtitle:   ""
date:       2020-05-12 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - Tech
    - Interview
    
---


## Use Guava cache in JAVA

```java

@Service
public class CacheService {

   // LoadingCache is thread safe and can be safely accessed by multiple concurrent threads
    private LoadingCache<CacheKey, Boolean> myCache = null;
    
    @PostConstruct
    public void init() {
            issuerCertsCache = CacheBuilder.newBuilder().maximumSize(CACHE_SIZE) // maximum 100 records can be cached
                .expireAfterWrite(TTL_IN_DAYS, TimeUnit.DAYS)  // cache will expire after 1 day of write
                .build(new CacheLoader<DasCacheKey, Boolean>() {  // build the cacheloader
                    @Override
                    public Boolean load(DasCacheKey cacheKey) {
                        // doSomething 
                        return doSomething(cacheKey);
                    }
                });    
    }

    public LoadingCache<CacheKey, Boolean> getMyCache(){
        if (Objects.isNull(myCache)) init();
        return myCache;
    }
    
}
  
```    

### Unite test for guava cache

```java

public class CacheServiceTest {

    @InjectMocks
    private CacheService cacheService;

    @Mock
    private LoadingCache<CacheKey, Boolean> myCache;
    
    @Test
    public void testWithCache_True() throws Exception {

        when(myCache.get(any(CacheKey.class))).thenReturn(Boolean.TRUE);

        Boolean actual = dasCacheService.getMyCache().get(getDasCacheKey());

        Assert.assertTrue(actual);
    }
}    
```

### END

