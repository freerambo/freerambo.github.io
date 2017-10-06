---
layout:     post
title:      "阿里妈妈广告业务-二面设计题"
subtitle:   "Interview of Alimama - Design - Senior JAVA developer - Beijing"
date:       2017-09-17 12:11:43
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - 面试


> 求学至北方，混迹于南洋. 欢迎来到自由兰博的世界

## 评测题目:+编写代码题：
>
有一张任务表demo_task。有4个字段，分别是
id（任务ID）、
name（任务名）、
status（任务状态，分别是未开始、进行中、已完成、失败）、
content（任务内容）。
>
业务逻辑是，每秒会至少新增10个任务，状态是末开始，进行中的任务一般要处理1分钟，有些任务会失败，因此需要重试3次。
请用java多线程机制，实现一个任务处理的流程。
任务处理部分请用伪代码即可。
重点是从数据表读取任务、多线程处理任务、更新状态这三个部分。
其余可省略。请随意发挥。

1、	请编写伪代码，将任务处理的整个流程体现出来？
2、	你觉得这个任务处理的核心问题是什么？为什么？
3、  如何提升开发者的开发效率，请举一个你实践过的案例。


### 1. 伪代码

```java

// Task entity 
  Class Task: 
	id, name, status, content

// DB operations 
  Class TaskDao:
	// fetch only the unprocessed/ new added tasks
  	public List<Task> readTask(){
	// Driver Class.forName(Driver..)
	//Connection
	//Statement
	//Transaction
	//Resultset
	//Close
    }
    public void updateStatus(Task task){
    	// update task status
    }
//Task processing
  @EnableScheduling
  Class TaskService: 
	volatile List<Task> taskList = new ArrayList<Task>();
    static ExecutorService executor = Executors.newFixedThreadPool(600);

	@Scheduled(fixedDelay=1000)
	public void process(){
    	taskList.add(TaskDao.readTask());
      	processTasks(taskList);
    }
	public void processTasks(List<Task> taskList){
		for task : taskList
          executor.execute(new Handler(task));
      	  taskList.remove(task);
    }

// handler for task
  class Handler implements Runnable {
     private final Task task;
     private final volatile int count;
     Handler(Task task) { this.task = task; }
     public void run() {
       while(count < 3){
         count++;
         try{
			// run the task 
           
           	//update the task status if run success
                task.status = "success"; 
           	taskDao.updateTask(task);
   			// get out of the loop
           	break; 	                 	
         }catch(Exception e){
                task.status = "failure"; 
           	taskDao.updateTask(task);
			// exception handling 
         }
       }
     }
   }
  	
```



### 2. 你觉得这个任务处理的核心问题是什么？为什么？
  
  多线程管理： 
  每秒至少10条新增任务，每个任务处理一分钟。这里采用大小为600的线程池ExecutorService 10*60 = 600 
  
  任务列表的维护：
  采用volatile关键字， 同时维护list的加减。
  
  任务执行：
  每条任务可能出错，至少执行三次，这里采用单独的处理线程handler并加上计数器控制
  

### 3. 如何提升开发者的开发效率，请举一个你实践过的案例。
 
  a. 工具 
	-- git 代码同步和版本控制
    -- IDE 方便开发 eclipse等
    -- 快捷键 
   b.开发方法 Scrum framework，agile，增量开发, 结伴编程
       在实践过程中，用户场景页面的开发方式让我们尽快的满足用户需求，保证开发正确
   c.管理，将事情记录下来，先做重要的事，专注。

团队方面： 在完成产品线框图后，首先特别重要的事定义好前后端交互的API文档，用来指导前后端的同时开发。 任何API的修改需要前后端人员共同参与。



> 2017年10月06日更新

朱渊博，特普通一人，渊为深，博为广，一直努力做一个有深度有广度的人。多混迹于Github, Stackoverflow, ITeye, 知乎, Linkedin 等地带。蹉跎中练就了一身码农好本领。 酷爱编程，但绝不宅。爱生活，爱思考，性情温和但充满激情，做事有主见，信奉读万卷书行万里路。
<http://www.freerambo.com/about>

---

### END

