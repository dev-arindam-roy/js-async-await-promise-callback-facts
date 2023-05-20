# Asynchronous Javascript using - async, await, promise, callback
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

## callback
```js
const userlist = [
  { name: "John Doe", profession: "Software Developer" },
  { name: "Alice Smith", profession: "Graphic Designer" },
  { name: "Michael Johnson", profession: "Data Analyst" },
  { name: "Emily Brown", profession: "UX/UI Designer" }
];


function showUsers() {
  setTimeout(() => {
    if (userlist.length) {
      userlist.forEach((item, index) => {
        console.log(index + '> ' + item.name + ' --- ' + item.profession);
      });
    }
  }, 1000);
}

function createUser(userData, callback = null) {
  setTimeout(() => {
    userlist.push(userData);
    callback();
  }, 3000);
}

/** Here is the callback use to aync the execution **/
/** Here showUsers() function will display after execution of createUser() function **/
createUser({name: "Arindam Roy", profession: "Dream Developer"}, showUsers);
```

