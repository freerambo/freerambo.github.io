---
layout:     post
title:      "ReactJS training3"
subtitle:   ""
date:       2019-01-14 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - sharing
    - startup
    
---

## reducer

````javascript
nums.reduce((n1,n2)=>{
var even = n2%2 === 0? ++n1.even:n1.even;
var odd = n2%2 !== 0? ++n1.odd:n1.odd;
console.log(`n1=${n1} n2=${n2} result = ${even} odd ${odd}`)
return {even, odd}
},{even:0, odd:0})

````


```javascript

var stateManager = (function(){
	var _currentState = undefined,
		_subscribers = [],
		_reducer = function(){},
		_init_action = '@@INIT_ACTION';

	function getState(){
		return _currentState;
	}

	function subscribe(listenerFn){
		if (typeof listenerFn === 'function')
			_subscribers.push(listenerFn);
	}

	function triggerChange(){
		_subscribers.forEach(listenerFn => listenerFn());
	}

	function dispatch(action){
		var newState = _reducer(_currentState, action);
		if (newState === _currentState) return;
		_currentState = newState;
		triggerChange();
	}

	function createStore(reducer){
		if (typeof reducer !== 'function') 
			throw new Error('a reducer is mandatory to create a store');
		_reducer = reducer;
		_currentState = _reducer(_currentState, _init_action);
		return { getState, subscribe, dispatch };
	}

	return { createStore };
})();

function spinnerReducer(currentStatus=0,action){
if(action === 'UP') return ++currentStatus;
if(action === 'DOWN') return --currentStatus;
return currentStatus;
}

var store = stateManager.createStore(spinnerReducer)
store.subscribe(() => console.log(store.getState()))

store.dispatch('UP')

store.dispatch('DOWN')


```



Action -> Store -> reducer (renderApp)

Unidirectional Data flow. Everything is clean.


if only render, we can define as a function 

No This, props will be given as arguments
　　
## ReactJS


Deployemnt

minifine
argumentize
bundling

call webpack



---

### END

