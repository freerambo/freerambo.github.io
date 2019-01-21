---
layout:     post
title:      "ReactJS training1"
subtitle:   ""
date:       2019-01-14 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - sharing
    - startup
    
---



　　
## Browser

Before 2010, browser only work as mediate, DOM terminal

- Infrastructure changes 
    - data storage cap.
    - file system access
    - multi-thread cap. 
    - realtime updates
    - offline access
    - device access
- Browser(OS) becomes much powerful
    -   Http client
    -   HTML DOM render

## HTML5 

Data storage
local storage
session storage
indexed DB

File access - API 
web workers

server send events 
webRTC
websocket

Cache
service worker

Device access - API 

Browser as virtual OS


## Books 
- Patterns of Enterprise Application architecture
- Design Patterns
- Refactoring
- Refactoring to patterns
- Agile Principles Patterns and Practives
- Clean Code
- Domain driven design
- Pragmatic Programmer
- Growing Object-Oriented Software Guided by Tests

## RIA

Rich Internet Application - RIA 
UX


SPA

**Maintainability**  easy to change

private implementation does not impact other dependent entities.



```javascript
 
```


Incremental- Efficient but complex (Angular)

ReplaceAll - Inefficient but simple


React - Simple and Efficient
How? 

Introduced a virtual DOM on top of real DOM

vDOM - replaceAll
vDOM-> DOM incremental


## ES6
var -> NOT block scope

````javascript
for(var i =0; i < 10; i++){}

i = 10

for(let i =0; i < 10; i++){}
after loop
i = undefined



````

const. can not change

array destructuring

rest operator ...argument

spread operator

default arguments

arrows function =>

Classes

```javascript
class Employee{
    constructor(id,name,salary){
        this.id = id;
        this.name = name;
        this.salary = salary
    }
    
       display(){
            console.log(`id is ${this.id} name is ${this.name} salary is ${this.salary}`)
        }
}

class FteEmployee extends Employee{
    constructor(id,name,salary, benifits){
        super(id,name,salary);
        this.benifits = benifits
    }
}
```
// variable hoisting

handy features


```javascript

a = [1,2,3,4]

[x,y] = a
//swap two value
[x,y] = [y,x]


var emp = {id:100, name:"hello", salary:20000}


var {id, ...z} = emp
z {name: "hello", salary: 20000}


// default value
function add(x=10,y=20){
    return x+y;
}

add() // return 30
add(20,40) // return 60


// block
var add = (x,y) => {
    return x+y;
}

var add = (x,y) => x+y;

'value of x is ${x}'

```

UI need elements - > state
used by other modules outside -> model

---

### END

