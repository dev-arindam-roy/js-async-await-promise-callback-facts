# JS async, await, promise, callback facts
Snippets for async, await, promise, callback debugging example and facts

## Why? there is the problem
```js
console.log(1); // 1st execute
setTimeout(() => { console.log(2) }, 2000); // 5th execute
console.log(3); // 2nd execute
setTimeout(() => { console.log(4) }, 3000); // 6th execute
console.log(5); // 3rd execute
setTimeout(() => { console.log(6) }, 5000); // 7th execute
setTimeout(() => { console.log('ABC') }, 1000); // 4th execute

/** 
Output Will Be 
1 3 5 "ABC" 2 4 6
**/
```
