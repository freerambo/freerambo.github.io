---
layout:     post
title:      "Qualification Test Online - Line corporation (Fukuoka Office)"
subtitle:   "1 hour online test - Development Engineer - Fukuoka "
date:       2017-10-03 18:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - 面试
    - 技术

---


> 求学至北方，混迹于南洋. 欢迎来到自由兰博的世界

## Q1. Please write a brief explanation of Processes/Threads. Also, please compare the differences of the two in your explanation.

>  Answer:

   Process is the execution of a program. Generally, a process contains one or more threads. 
   
   Thread is the minimium task can be allocated by CPU for a process. 
   
   Threads within a process will share the resources of process. 
   
   Process is heavy weight cause it includes memory, resources and i/o channels. Thread is much light weight.
   
   Often we use multi-thread to improve the efficiency of process. 



## Q2. Given “n” number of elements, please demonstrate how to output a permutation with a length of n. 
(Duplications acceptable; please use only pseudo-code and/or algorithms in your answer.)

```
Input example: data = {'a', 'b', 'c' }
Output example: aaa, aab, aac, aba, abb, abc, ... , cca, ccb, ccc
```

> Answer:

```java
String output(String[] data){

  StringBuilder sb = new StringBuilder();
  for(int i =0; i < data.length; i++){
  
    int n = Random.nextInt(data.length);
    sb.append(data[n]);
  }

  return sb.toString();

}
```

##  Q3. The following defines the parameter “n” entered for an algorithm.
```
(1) Bubble sort - n: Number of elements for target array
(2) Quick sort - n: Number of elements for target array 
(3) Binary search - n: Size of collection that will be the search target
(4) Hash search – n: Size of collection that will be the search target. Please assume that the table is large enough to handle anything, and that you are using the ideal hash algorithm 

Please choose the correct order for each algorithm from among the options below.

O(1)　　O(log n)　　O(n)　　O(n * log n)　　O(n^2)　　O(n^3) and above
--------------

```

> Answer:

```
(1) O(n^2)   avg/worst   
(2) O(n * log n) avg   worst  O(n^2)
(3) O(log n) avg   worst O(n^2)
(4) O(1) avg/worst  
```

## Q4. Please briefly explain the following terms. Also, please describe the names of typical algorithms and use applications for each of the items below.
```
(1) Message-digest 
(2) Shared-key encryption (symmetric)
(3) Public-key encryption (asymmetric)
-------------
```
> Answer:

(1)    Message-digest

```
It is a kind of one-way hashing algorithm. Most frequently I use MD5. 

From target file or object, it will take random data then transmit a set length hash value.
```


(2)    Shared-key encryption (symmetric)

```
Symmetric-key encryption will use the same cryptographic key to encrypt and decrept the message. 

```

(3)    Public-key encryption (asymmetric)
```
There are a pair of public key and private key. User will sent the public key to others. 
If user sent message encrypted with his/her private key others can decode it with the public. 
On the contrary, message encoded with the public key, only the private can decrypt it. 
```




## Q5. Please demonstrate how to implement a “Queue” using two “Stacks”.


> Answer:

``` java
Queue: first come first out
Stack: first come last out

Two stack ---- > one queue



Enqueue 
Push all input element to the Input Stack


Dequeue 

If ( Output Stack Empty)
    pop every element in the Input Stack
    then push all of them to the Output Stack 

//  Java code 

public class Queue<E>
{

    private Stack<E> inbox = new Stack<E>();
    private Stack<E> outbox = new Stack<E>();

    public void queue(E item) {
        inbox.push(item);
    }

    public E dequeue() {
        if (outbox.isEmpty()) {
            while (!inbox.isEmpty()) {
               outbox.push(inbox.pop());
            }
        }
        return outbox.pop();
    }

}

```

## Q6.Please explain differences between “Blocking I/O” and “Non-blocking I/O”.

> Answer:


Blocking: It will keep waiting utils the IO process was done.  

NonBlocking: It will pass the data to IO channel then do others utils there's a callback from the process. 




## Q7. If you were developing an Echo Server that accepts requests from over 10K clients at same time, 
what kind of problems do you predict? Please explain at least two problems and their solutions describing 
particular OSs and programing languages.
(An “Echo Server” is a server that directly outputs content that is input.)

-----------------

> Answer:

Two problems: 

1. Wait time is too long for each user
2. 10K requests at th esame time may exceed server's capability

Solutions in Java on Linux.

1 Cache is the king
Configure the cache at each layer

when user with same request, it will hit the cache then response the value we stored in cache.
This will greatly shorten the response time. 

2. if too much request beyond server's capability

We will consider use the message queue like kafka or apacheMQ. 

we can configure the MQ and only process i. e. 1000 request maximium. 
Above that, we will deny the service or forward user other pages

3. clustering 

add more servers and configured as a cluster with load balance by nginx or haproxy.

The servers can be managed by zookeeper. 


## Q8.Please explain the problems with the following code, and provide solutions for them.

```objectc
NSMutableArray *arr = [@[@"a1",@"b",@"c",@"a2",@"e"] mutableCopy];
[arr enumerateObjectsWithOptions:NSEnumerationConcurrent usingBlock:^(id obj,NSUInteger idx,BOOL *stop) {
    if ([obj hasPrefix:@"a"]) {
        [arr removeObjectAtIndex:idx];
    }
}];
```

> Answer:


Not applicable. There's no pionter in Java. 
  
  


## Q9. Please explain the following terms.
(1) Target-action paradigm
(2) NSCopying, NSCoding 


> Answer:

(1)    Target-action paradigm






(2)    NSCopying, NSCoding 





I hava no experience on object C or IOS  

## Q10. The following view controller’s dealloc() does not get called even after the external references of it are gone. Please fix the code so dealloc() can get called. 
//    （Assumptions: ARC supported, iOS5 and above）

```c
#import "ViewController.h"

@interface ViewController ()
    @property (nonatomic, copy) void (^completionBlock)();
@end

@implementation ViewController

- (void)dealloc {
    NSLog(@"dealloc");
}

- (void)viewDidLoad {
    [super viewDidLoad];
    self.completionBlock = ^{
        [self someMethod];
    };
}

- (void)viewWillAppear:(BOOL)animated {
    [super vierWillAppear:animated];
    
    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        //long process
        self.completionBlock();
    });
}

- (void)someMethod {
    // Do something
}

@end
```

//------------------------
//Answer:

limited by time. No experience on IOS. 
    
    
    

## Q11. Does the following class have any problems? If so, please explain and provide solutions for them.

```java
public class MainActivity extends Activity {
    public String urlString;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        findViewById(R.id.button1).setOnClickListener(new OnClickListener() {
            @Override
            public void onClinck(View v) {
                try {
                    URL url = new URL(urlString);
                    HttpURLConnection conn = (HttpURLConnection)url.openConnection();
                    InputStream in = new BufferedInputStream(conn.getInputStream());
                    byte[] bytes = IOUtils.toByteArray(in);
                    draw(bytes); // Display the read date                    
                } catch (MalfromedURLException e){
                    e.printStackTrace();                    
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        });
    }
}
```

//------------------------
> Answer:

To read data through a web service, you can use code below. 
  
 ```java 
 BufferedReader in = new BufferedReader(
                                new InputStreamReader(conn.getInputStream()));

StringBuilder result = new StringBuilder();
String line;
while((line = reader.readLine()) != null) {
    result.append(line);
}
line = result.toString();
if(line != null)
  draw(line.getBytes()); // Display the read date     

```

Q12.
You are about to implement a list with thumbnails (like LINE’s “Friends” list). 
Usually, thumbnails take time to fetch. In this situation, please explain in detail what 
you would do to improve user experience (e.g., faster scroll and lower memory usage, etc.).

> Answer:


1. Make full use of local memory at client side.
2. Configure cache at every layer both client and server. 
3. use CDN that user will request the nearest server
4. rich client feathers to improve user experience. 



> 2017年10月06日 更新

朱渊博，特普通一人，渊为深，博为广，一直努力做一个有深度有广度的人。多混迹于Github, Stackoverflow, ITeye, 知乎, Linkedin 等地带。蹉跎中练就了一身码农好本领。 酷爱编程，但绝不宅。爱生活，爱思考，性情温和但充满激情，做事有主见，信奉读万卷书行万里路。
<http://www.freerambo.com/about>

---

### END

