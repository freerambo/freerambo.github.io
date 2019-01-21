---
layout:     post
title:      "ReactJS training"
subtitle:   ""
date:       2019-01-14 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - sharing
    - startup
    
---



　　
## javascript test
Jasmine 

```javascript
  describe("A Calculator", function(){
  it("Should be able to add two numbers", function(){
    //Arrange
    var number1 = 10,
      number2 = 20,
      expectedResult = 30;
    //Act
    var result = add(number1, number2);
    //Assert
    expect(result).toBe(expectedResult);
  });
  });
```

## Javascript Closures

Closure is the only way for javascript has private area. 

If extends a life of inner method, then will extends the outter method life circle as well.

This is called a JavaScript closure. It makes it possible for a function to have "private" variables.

The counter is protected by the scope of the anonymous function, and can only be changed using the add function.


```javascript

var isPrime = (function(){
	var cache = {};

	function checkPrime(n){
		console.log('Processing ', n);
		for(var index = 2; index <= (n/2); index++)
			if (n % index === 0)
				return false;
		return true;
	}

	return function(n){
		if (typeof cache[n] === 'undefined')
			cache[n] = checkPrime(n);
		return cache[n];
	}
})();


var isEvenOrOdd = (function(){
	var cache = {};

	function checkEvenOrOdd(n){
		console.log('Processing ', n);
		return n % 2 === 0 ? 'even' : 'odd';
	}

	return function(n){
		if (typeof cache[n] === 'undefined')
			cache[n] = checkEvenOrOdd(n);
		return cache[n];
	}
})();


var isPrime = memoize(function checkPrime(n){
	console.log('Processing ', n);
	for(var index = 2; index <= (n/2); index++)
		if (n % index === 0)
			return false;
	return true;
});

var isOddOrEven = memoize(function checkEvenOrOdd(n){
	console.log('Processing ', n);
	return n % 2 === 0 ? 'even' : 'odd';
});

function memoize(fn){
	var cache = {};

	return function(){
		var key = JSON.stringify(arguments);
		if (typeof cache[key] === 'undefined')
			cache[key] = fn.apply(this, arguments);
		return cache[key];
	}
}


```

## Construct

this -> new Object
return by default

```javascript

function Employee(id, name, salary){
    // this -> new Object
    this.id = id;
    this.name = name;
    this.salary = salary;
    
    // this -> returned by default
    
    this.display = function(){
        console.log(this.id, this.name, this.salary);
    }
}


var emp = new Employee(100,"yuanbo", 10000)

emp.display()


function Spinner(){
    
    var counter = 0;
    
    if(!(this instanceof Spinner))
        return new Spinner();
    
    this.up = function(){
        return ++counter;
    }
    
    
    this.down = function(){
        return --counter;
    }
}


function Spinner(){
  this.__counter__ = 0;  
  if(!(this instanceof Spinner))
          return new Spinner();
    
}
//can not hide counter if using prototype
Spinner.prototype.down= function(){
  return --this.__counter__;
}

Spinner.prototype.up = function(){
 return ++this.__counter__;
}


```


child class mantian base class info in __proto__
prototype hooping only for read. does not happen when write. when create will generate a new one if not existing. 

Hiding is costly. 


constructor method has prototy which has a default empty object which is the base for all objects.


Object.prototype is the **root** for all objects.


```javascript

Employee.prototype.display = function(){
     console.log(this.id, this.name, this.salary);
 }

```



Home works:

JavaScript forEach map, reduce, filter

---

### END

