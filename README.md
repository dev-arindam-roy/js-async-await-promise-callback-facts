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
## Promise
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

function createUser(userData) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      userlist.push(userData);
      let isErrorOccur = false;
      if (!isErrorOccur) {
        //resolve();
        resolve("pass your param");
      } else {
        reject("Something Wrong");
      }
    }, 3000);
  });
}

/** Here is the promise uses to aync the execution **/
/** First createUser() call and if the previous call has success Then showUsers() call **/

/** Call Example: 1 (simple) **/
createUser({name: "Arindam Roy", profession: "Dream Developer"}).then(showUsers);

/** Call Example: 2 (using error parameter from reject) **/
createUser({name: "Arindam Roy", profession: "Dream Developer"}).then(showUsers)
  .catch((getRejectParam) => { console.log(getRejectParam); });

/** Call Example: 3 (using no parameter from resolve) **/
createUser({name: "Arindam Roy", profession: "Dream Developer"}).then(() => {
  showUsers();
}).catch((getRejectParam) => { 
  console.log(getRejectParam); 
});

/** Call Example: 4 (using both resolve & reject parameter) **/
createUser({name: "Arindam Roy", profession: "Dream Developer"}).then((getResolveParam) => {
  // do operation
  console.log(getResolveParam); 
  showUsers();
  // do another operations
}).catch((getRejectParam) => {
  console.log(getRejectParam);
});
```
# async & await
```js
function f0() {
    setTimeout(() => {console.log('0/6sce')}, 6000);
}

function f1() {
  console.log('1/0sce');
}

function f2() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (true) {
        console.log('2/5sce'); 
        resolve();
      } else {
        console.log('error-2');
        reject();
      } 
    }, 5000);
  });
}

function f3() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (true) {
        resolve(console.log('3/3sce'));
      } else {
        reject(console.log('error-3'));
      } 
    }, 3000);
  });
}

function f4() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (true) {
        resolve(console.log('4/1sce'));
      } else {
        reject(console.log('error-4'));
      } 
    }, 1000);
  });
}

/**
Point 1: 'await' can't use without 'async'
Point 2: 'await' feature will work if returns come from 'promise' 
**/

/** A simple understandable call like below **/
async function start() {
  await f0(); // await not work here as f0() not return promise
  await f1(); // await not work here as f1() not return promise
  await f2();
  await f3();
  await f4();
}

/** Another simple call using the **/ 
async function start() {
  f0();
  await f2().then(f3).then(f4);
  f1();
}

/** Nested async - await call **/
async function start() {
  await f2().then(async () => {
    await f3().then(async () => {
      await f4().then(async () => {
        console.log('success block of f4');
      }).catch(() => {
        console.log('error catch block of f4');
      });
    }).catch(() => {
      console.log('error catch block of f3');
    });
  }).catch(() => {
    console.log('error catch block of f2');
  });
  f0();
  f1();
}

start();
```

