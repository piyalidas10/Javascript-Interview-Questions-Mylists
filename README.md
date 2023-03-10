# Javascript-Interview-Questions-Mylists
Javascript Interview Questions

### 1. Pure function vs Impure functions

<details><summary><b>Answer</b></summary>
<p>

#### 

- Pure functions do not have side effects. Impure functions can cause side effects.
- Pure functions return the same output if we use the same input parameters. However, impure functions give different outcomes when we pass the same arguments multiple times.
- Pure functions always return some results. Impure functions can execute without producing anything.
- Debugging pure functions is relatively easier than debugging impure functions.
- Pure functions cannot execute AJAX calls or standard DOM manipulation.
- Impure functions aren’t inherently wrong. They merely can cause some confusion in more extensive systems in the form of spaghetti code.

`Pure Functions`
To get a better understanding, let’s consider the following example.
```
function add(a,b) { 
  return a + b
}
console.log(add(4,5))
```
This example contains a simple add() function, which gives 9 as the output. It is a very predictable output, and it does not depend on any external code. This makes the add() function a pure function.

`Impure Functions`
For example, consider the following code snippet:
```
var addNew = 0;
function add(a,b){ 
  addNew =1; 
  return a + b + addNew
}
console.log(add(4,5))
```
In the above example, there is a variable named addNew, and it is declared outside of the add() function. But the state of that variable is changed inside the add() function. So, the add() function has a side effect on a variable outside of its scope and is therefore considered an impure function.

</p>
</details>

---

### 2. Javascript is Synchronous or Asynchronous

<details><summary><b>Answer</b></summary>
<p>

#### 
  - Javascript is the synchronous single-threaded language but with the help of event-loop and promises, JavaScript is used to do asynchronous programming.
  - The most common triggers for asynchronous Javascript operations are:
      1. Event Loop
      2. Callback
      3. Promises
      4. Async/Await
  
  https://www.freecodecamp.org/news/synchronous-vs-asynchronous-in-javascript/
  https://betterprogramming.pub/is-javascript-synchronous-or-asynchronous-what-the-hell-is-a-promise-7aa9dd8f3bfb
  
  
 </p>
</details>

---

### 3. Javascript Event Loop

<details><summary><b>Answer</b></summary>
<p>

#### 
  https://www.jsv9000.app/
  
  ![JavaScript Visualizer 9000](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/blob/main/images/jsv9000.gif)
  
  ```
  function three() {
  console.log("three");

  setTimeout(function () {
    console.log("last");
  }, 1000);
}

function two() {
  console.log("two");
  three();
}

function one() {
  console.log("one");
  two();
}

one();
```
  ![JavaScript Visualizer 9000](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/blob/main/images/event_loop_1.gif)
  
  JavaScript is single-threaded: only one task can run at a time. Usually that’s no big deal, but now imagine you’re running a task which takes 30 seconds.. Ya.. During that task we’re waiting for 30 seconds before anything else can happen (JavaScript runs on the browser’s main thread by default, so the entire UI is stuck) 😬 It’s 2019, no one wants a slow, unresponsive website.

Luckily, the browser gives us some features that the JavaScript engine itself doesn’t provide: a Web API. This includes the DOM API, setTimeout, HTTP requests, and so on. This can help us create some async, non-blocking behavior.


When we invoke a function, it gets added to something called the call stack. The call stack is part of the JS engine, this isn’t browser specific. It’s a stack, meaning that it’s first in, last out (think of a pile of pancakes). When a function returns a value, it gets popped off the stack.
  
![popped off the stack](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/blob/main/images/call_stack.gif)


The respond function returns a setTimeout function. The setTimeout is provided to us by the Web API: it lets us delay tasks without blocking the main thread. The callback function that we passed to the setTimeout function, the arrow function () => { return 'Hey' } gets added to the Web API. In the meantime, the setTimeout function and the respond function get popped off the stack, they both returned their values!

![setTimeout](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/blob/main/images/setTimeout.gif)


In the Web API, a timer runs for as long as the second argument we passed to it, 1000ms. The callback doesn’t immediately get added to the call stack, instead it’s passed to something called the queue.

![callback_queue](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/blob/main/images/callback_queue.gif)


This can be a confusing part: it doesn't mean that the callback function gets added to the callstack(thus returns a value) after 1000ms! It simply gets added to the queue after 1000ms. But it’s a queue, the function has got to wait for its turn!

Now this is the part we’ve all been waiting for… Time for the event loop to do its only task: connecting the queue with the call stack! If the call stack is empty, so if all previously invoked functions have returned their values and have been popped off the stack, the first item in the queue gets added to the call stack. In this case, no other functions were invoked, meaning that the call stack was empty by the time the callback function was the first item in the queue

![event_loop](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/blob/main/images/event_loop.gif)


The callback is added to the call stack, gets invoked, and returns a value, and gets popped off the stack.

![callstack_executed](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/blob/main/images/callstack_executed.gif)


Reading an article is fun, but you'll only get entirely comfortable with this by actually working with it over and over. Try to figure out what gets logged to the console if we run the following:

```
const foo = () => console.log("First");
const bar = () => setTimeout(() => console.log("Second"), 500);
const baz = () => console.log("Third");

bar();
foo();
baz();
```

Got it? Let's quickly take a look at what's happening when we're running this code in a browser:

![output](https://github.com/Javascript-Interview-Questions-Mylists/blob/main/images/output.gif)

- 1. We invoke bar. bar returns a setTimeout function.
- 2. The callback we passed to setTimeout gets added to the Web API, the setTimeout function and bar get popped off the callstack.
- 3. The timer runs, in the meantime foo gets invoked and logs First. foo returns (undefined),baz gets invoked, and the callback gets added to the queue.
- 4. baz logs Third. The event loop sees the callstack is empty after baz returned, after which the callback gets added to the call stack.
- 5. The callback logs Second.
  
  
 </p>
</details>

---

### 4. Slice vs Splice

<details><summary><b>Answer</b></summary>
<p>

#### 

- 1. Slice is used to get a new array from the original array whereas the splice is used to add/remove items in the original array. The changes are not reflected in the original array in the case of slice and in the splice, the changes are reflected in the original array.
```
--------- Slice-----------
let arr = [4, 3, 5, 9];
let res = arr.slice(0, 2);
console.log(res); --------- (2) [4, 3]

-----------Splice----------
let arr = [4, 3, 5, 9];
arr.splice(0, 2);
console.log(arr); --------- (2) [5, 9]
```
- 2. The splice() method is used to add or remove elements of an existing array 
```
--------- Add element-----------
let arr = [4, 3, 5, 9];
arr.splice(2, 0, 10);
console.log(arr); ---------- (5) [4, 3, 10, 5, 9]

--------- Remove element-----------
let arr = [4, 3, 5, 9];
arr.splice(2, 1);
console.log(arr); ---------- (3) [4, 3, 9]
```
</p>
</details>

---

### 5. Spread vs Rest

<details><summary><b>Answer</b></summary>
<p>

#### 

The spread operator helps us expand an iterable such as an array where multiple arguments are needed, it also helps to expand the object expressions.
    var array1 = [10, 20, 30, 40, 50];
    var array2 = [60, 70, 80, 90, 100];
    var array3 = [...array1, ...array2];
    console.log(array3);       [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
  
    const obj = {
        firstname: "Divit",
        lastname: "Patidar",
    };
    const obj2 = { ...obj };
    console.log(obj2);
    {
        firstname: "Divit",
        lastname: "Patidar"
    }
  
The rest parameter syntax allows a function to accept an indefinite number of arguments as an array.
  
  ```
  function myBio(firstName, lastName, ...otherInfo) {
    return otherInfo;
  }

  console.log(
    myBio('Oluwatobi', 'Sofela', 'CodeSweetly', 'Web Developer', 'Male')
  );
  ["CodeSweetly", "Web Developer", "Male"]
  ```

</p>
</details>

---

### 5. Event Propagation ( Event Bubbling and capturing )

<details><summary><b>Answer</b></summary>
<p>
  
#### 
  ![bubbling-and-capturing](https://javascript.info/article/bubbling-and-capturing/eventflow.svg)
  <br>
- 1. Capturing phase – the event goes down to the element.
- 2. Target phase – the event reached the target element.
- 3. Bubbling phase – the event bubbles up from the element.
  
Event capturing means propagation of event is done from ancestor elements to child element in the DOM while event bubbling means propagation is done from child element to ancestor elements in the DOM. <br>
  
  ![bubbling-and-capturing](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/blob/main/images/LsIr2.png)
  <br>
DOM Events describes 3 phases of event propagation: Capturing phase – the event goes down to the element. Target phase – the event reached the target element. Bubbling phase – the event bubbles up from the element.
  
Both can be prevented by using the stopPropagation() method.

</p>
</details>

---


### 6. Top 10 Features of ES6 

<details><summary><b>Answer</b></summary>
<p>

#### 

https://www.boardinfinity.com/blog/top-10-features-of-es6/
<br>

- 1. let and const Keywords
- 2. Arrow Functions
- 3. Multi-line Strings
- 4. Default Parameters
- 5. Template Literals
- 6. Destructuring Assignment
- 7. Enhanced Object Literals
- 8. Promises
- 9. Classes
- 10. Modules


</p>
</details>

---

### 7. What is Recursion?

<details><summary><b>Answer</b></summary>
<p>

#### 
Recursion is a technique for iterating over an operation by having a function call itself repeatedly until it arrives at a result.
```
function factorial(num) { 
   if(num <= 0) { 
      return 1; 
   } else { 
      return (num * factorial(num-1)  ) 
   } 
} 
console.log(factorial(6)) 
```

</p>
</details>

---

### 8. What is Temporal Dead Zone?

<details><summary><b>Answer</b></summary>
<p>

#### 
Temporal Dead Zone is the period of time during which the let and const declarations cannot be accessed.
Temporal Dead Zone starts when the code execution enters the block which contains the let or const declaration and continues until the declaration has executed.
Variables declared using let and the constants declared using const are hoisted but are in a TDZ. 

```
console.log(a);
var a = 10;
It will print ----------- undefined

console.log(a);
let a = 10;
It will print ----------- VM228:1 Uncaught ReferenceError: a is not defined
```

</p>
</details>

---

### 9. map() vs forEach()

<details><summary><b>Answer</b></summary>
<p>

#### 
The map() method is used to transform the elements of an array, whereas the forEach() method is used to loop through the elements of an array. The map() method can be used with other array methods, such as the filter() method, whereas the forEach() method cannot be used with other array methods.

</p>
</details>

---

### 10. What is prototype in javascript

<details><summary><b>Answer</b></summary>
<p>

#### 
Prototypes are the mechanism by which JavaScript objects inherit features from one another. The prototype is an object that is associated with every functions and objects by default in JavaScript. 
When a programmer needs to add new properties like variables and methods at a later point of time, and these properties need sharing across all the instances, then the prototype will be very handy.

</p>
</details>

---

### 11. When to use Prototype in JavaScript?

<details><summary><b>Answer</b></summary>
<p>

#### 
As we all know, Javascript is a dynamic language where we can add new variables and methods to the object at any point in time, as shown below.
```
function Employee() {
    this.name = 'Arun';
    this.role = 'QA';
}

var empObj1 = new Employee();
empObj1.salary = 30000;
console.log(empObj1.salary); // 15

var empObj2 = new Employee();
console.log(empObj2.salary); // undefined
```
As we see in the above example, the salary variable adds to the empObj1. But when we try to access the salary variable using the empObj2 object, it doesn't have the salary variable because the salary variable is defined only in the empObj1 object.

Now comes the question, Can there be a way that the new variable can be added to the function itself so as it is accessible to all the objects created using the function?

The answer to this question is the use of a prototype. A prototype is an invisible inbuilt object which exists with all the functions by default. The variables and methods available in the prototype object can be accessible, modifiable, and even can create new variables and functions.

</p>
</details>

---

### 12. prototype inheritance in javascript

<details><summary><b>Answer</b></summary>
<p>

#### 
Prototype inheritance in javascript is the linking of prototypes of a parent object to a child object to share and utilize the properties of a parent class using a child class.
The syntax used for prototype inheritance has the __proto__ property which is used to access the prototype of the child. The syntax to perform a prototype inheritance is as follows :

child.__proto__ = parent;

```
let company = {
  name: "A",
  pay: function () {
    console.log("Paying");
  },
}; //company object
let worker = {
  id: 1,
  work: function () {
    console.log("Working");
  },
}; //worker object
worker.__proto__ = company; //worker object inherits company object
console.log(worker);
worker.pay(); // calling method from company object using worker object.
```

</p>
</details>

---

### 13. Remove duplicate values from Array

<details><summary><b>Answer</b></summary>
<p>

#### 
```
arr = [1, 2,3,4,5,6,2,3,4];
let arr1 = [...new Set(arr)];  ///////  [1, 2, 3, 4, 5, 6]
```

</p>
</details>

---

### 14. Removing duplicate objects (based on multiple keys) from array

<details><summary><b>Answer</b></summary>
<p>

#### 
```
const listOfTags = [
    {id: 1, label: "Hello", color: "red", sorting: 0},
    {id: 2, label: "World", color: "green", sorting: 1},
    {id: 3, label: "Hello", color: "blue", sorting: 4},
    {id: 4, label: "Sunshine", color: "yellow", sorting: 5},
    {id: 5, label: "Hello", color: "red", sorting: 6},
]

const unique = [];

listOfTags.map(x => unique.filter(a => a.label == x.label && a.color == x.color).length > 0 ? null : unique.push(x));

console.log(unique);
```

</p>
</details>

---

### 15. Why JavaScript is a single-thread language ? 

<details><summary><b>Answer</b></summary>
<p>

#### 
The JavaScript within a chrome browser is implemented by a V8 engine.

Memory Heap
Call Stack
Memory Heap: It is used to allocate the memory used by your JavaScript program. Remember memory heap is not the same as the heap data structures, they are totally different. It is the free space inside your OS.

Call Stack: Within the call stack, your JS code is read and gets executed line by line.

Now, JavaScript is a single-threaded language, which means it has only one call stack that is used to execute the program. The call stack is the same as the stack data structure that you might read in Data structures. As we know stacks are FILO that is First In Last Out. Similarly, within the call stack, whenever a line of code gets inside the call stack it gets executed and moves out of the stack. In this way, JavaScript is a single-thread language because of only one call stack.

JavaScript is a single-threaded language because while running code on a single thread, it can be really easy to implement as we don’t have to deal with the complicated scenarios that arise in the multi-threaded environment like a deadlock.

Since JavaScript is a single-threaded language, it is synchronous in nature. Now, you will wonder if you have used async calls in JavaScript so is it possible then?

So, let me explain to you the concept of async calls within JavaScript and how it is possible with single-threaded language. Before explaining it let’s discuss briefly why we require the async call or asynchronous calls. As we know within the synchronous calls, all the work is done line by line i.e. the first task is executed then the second task is executed, no matter how much time one task will take. This arises the problem of time wastage as well as resource wastage. These two problems are overcome by asynchronous calls, where one doesn’t wait for one call to complete instead it runs another task simultaneously. So, when we have to do things like image processing or making requests over the network like API calls, we use async calls.

Now, coming back to the previous question of how to use async calls within JS. Within JS we have a lexical environment, syntax parser, and an execution context (memory heap and call stack) that is used to execute the JS code. But these browsers also have Event Loops, Callback queue, and WebAPIs that are also used to run the JS code. Although these are not part of JS it also helps to execute the JS properly as we sometimes used the browser functions within the JS.e);

  ![JavaScript Runtime Environment](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/blob/main/images/single-threaded.jpg)

</p>
</details>

---

### 16. How can i make array immutable in javascript

<details><summary><b>Answer</b></summary>
<p>

#### 
In JavaScript, things that are immutable don’t change in value when you use them, and things that are mutable do.
Object. freeze() to arrays to make them immutable. 

```
For example, lets create a variable, age1, and assign its value to another variable, age2. If we update age2, the original variable, age1, is unaffected.
// Create a variable
let age1 = 42;
// Assign it to a new variable
let age2 = age1;
// Update the new variable
age2 = 84;
// logs 42
console.log(age1);
```

You can create an immutable copy of an array using Array.slice() with no arguments, or with the Array.from() method. It’s considered a best practice to do so before manipulating an array.
```
// Create an immutable copy
let evenMoreSandwiches = Array.from(sandwiches);
// Add a few sandwiches
sandwiches.push('italian', 'blt');
// logs ["turkey", "ham", "pb&j", "italian", "blt"]
console.log(sandwiches);
// logs ["turkey", "ham", "pb&j"]
console.log(evenMoreSandwiches);
```
You can use the spread operator to create immutable copies of arrays and objects instead of use Array.from().
</p>
</details>

---

### 17. How can i make object immutable in javascript

<details><summary><b>Answer</b></summary>
<p>

#### 
You can create an immutable copy of an object using Object.assign(). Pass in an empty object ({}) as the first argument and the object you want to copy as the second.

It’s considered a best practice to do so before manipulating an object.
```
// Create an immutable copy
let evenMoreLunch = Object.assign({}, lunch);
// Add a snack
lunch.snack = 'cookies';
// Logs {sandwich: 'turkey', drink: soda, snack: 'cookies'}
console.log(lunch);
// Logs {sandwich: 'turkey', drink: soda}
console.log(evenMoreLunch);
```
You can use the spread operator to create immutable copies of arrays and objects instead of use Object.assign().
</p>
</details>
---

