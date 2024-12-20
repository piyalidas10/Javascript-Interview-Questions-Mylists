# Javascript-Interview-Questions
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

### 2. What is Javascript closure ?

<details><summary><b>Answer</b></summary>
<p>

  #### 
  https://medium.com/@piyalidas.it/closure-in-javascript-2496fdedeb7d
  Closure provides access to the outer scope of a function from inside the inner function, even after the outer function has closed. The main advantage of javascript closure is Data Privacy.

  ```
  const countFunc = function counter() {
     let c = 0;
     c = c+1;
    return c;
  }();
```
  If you run countFunc, eveytime it will print "1". This you can resolve by closure.
  
  Solution using normal function
  ------------------------------------------
  const countFunc = function counter() {
     let c = 0;
     return function() {
         c = c+1;
         return c;
     }
  }();
  run countFunc()
  
  Solution using arrow function
  ------------------------------------------
  ```
  const countFunc = function counter() {
     let c = 0;
     return ()=> {
         c = c+1;
         return c;
     }
  }();
  run countFunc()
  ```

  The variable countFunc is assigned to the return value of a self-invoking function.
  
When to Use Closure in JavaScript (Practical use)?
  Suppose, you want to count the number of times user clicked a button on a webpage. For this, you are triggering a function on onclick event of button to update the count of the variable
 ```
<button onclick="updateClickCount()">click me</button>
 ```
Now there could be normal approaches like:
  You could use a global variable, and a function to increase the counter:
   ```
   var counter = 0;
   function updateClickCount() {
       ++counter;
       // Do something with counter
   }
   ```
  But, the pitfall is that any script on the page can change the counter, without calling updateClickCount().
  ou might be thinking of declaring the variable inside the function:
  ```
   function updateClickCount() {
       var counter = 0;
       ++counter;
       // Do something with counter
   }
  ```
  But, hey! Every time updateClickCount() function is called, the counter is set to 1 again.
  you need to find a way to execute counter = 0 only once not everytime. SO have to use closure.
  ```
  <script>
  var updateClickCount = (function(){
      var counter = 0;
  
      return function(){
          ++counter;
          document.getElementById("spnCount").innerHTML = counter;
      }
  })();
  </script>
  
  <html>
  <button onclick="updateClickCount()">click me</button>
  <div> you've clicked
      <span id="spnCount"> 0 </span> times!
  </div>
  </html>
  ```
 
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

```
<form onclick="alert('form')">FORM
  <div onclick="alert('div')">DIV
    <p onclick="alert('p')">P</p>
  </div>
</form>
```
So if we click on <p>, then we’ll see 3 alerts: p → div → form.
The process is called “bubbling”, because events “bubble” from the inner element up through parents like a bubble in the water.

event.stopPropagation() stops the move upwards

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

### 18. How can reduce Memory Leaks in Javascript ?

<details><summary><b>Answer</b></summary>
<p>

#### 
most of the memory leaks can be traced back to not removing all references to objects that you don't need anymore. This can happen when you forget to remove intervals or timers, or you make excessive use of global variables.
Storing data in global variables is probably the most common type of memory leak. In the browser, for instance, if you use var instead of const or let—or leave out the keyword altogether—the engine will attach the variable to the window object.
The same will happen to functions that are defined with the function keyword.
```
user = getUser();
var secondUser = getUser();
function getUser() {
  return 'user';
}
```
All three variables, user, secondUser, and getUser, will be attached to the window object.
This only applies to variables and functions that are defined in the global scope. If you want to learn more about this, check out this article explaining the JavaScript scope.
Avoid this by running your code in strict mode.
Apart from adding variables accidentally to the root, there are many cases in which you might do this on purpose.
You can certainly make use of global variables, but make sure you free space up once you don't need the data anymore.
To release memory, assign the global variable to null.
```
window.users = null;
```
So something to keep in mind is that memory is limited. When it comes to a call stack and memory Heap, those are two places is where javascript runs and stores memory. So we have to be careful not to have memory leaks or stack overflow if we are to have efficient code.

On the frontend, you can profile the memory usage in the Chrome DevTools under the Memory tab.

</p>
</details>

---
### 19. Why JavaScript is single threaded?

<details><summary><b>Answer</b></summary>
<p>

#### 

![CallStack](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/blob/main/images/javascript_call_stack.gif)

JavaScript can do one single thing at a time because it has only one call stack.
The call stack is a mechanism that helps the JavaScript interpreter to keep track of the functions that a script calls.
Every time a script or function calls a function, it's added to the top of the call stack. Every time the function exits, the interpreter removes it from the call stack.
A function either exits through a return statement or by reaching the end of the scope.

Every time a function calls another function, it's added to the top of the stack, on top of the calling function.
The order in which the stack processes each function call follows the LIFO principle (Last In, First Out).
 
</p>
</details>

---

### 20. Primitive vs Non-Primitive

<details><summary><b>Answer</b></summary>
<p>

#### 
Primitive data types
----------------------------------------------------------------------
Examples of Primitive data types are Boolean, char, byte, int, short, long, float, and double.

basic data types for whom memory is allocated in stack.
A stack is a data structure that JavaScript uses to store static data. Static data is data where the engine knows the size at compile time. In JavaScript, this includes primitive values (strings, numbers, booleans, undefined, and null) and references, which point to objects and functions.
Since the engine knows that the size won't change, it will allocate a fixed amount of memory for each value.
The process of allocating memory right before execution is known as static memory allocation.
Because the engine allocates a fixed amount of memory for these values, there is a limit to how large primitive values can be.
The limits of these values and the entire stack vary depending on the browser.

Reference (non primitive) data types
----------------------------------------------------------------------
1.	Function – typeof function is function
2.	Array – typeof array is object
3.	Object – typeof object is object
4.	Date – typeof date is object

Memory allocated for reference data types is in heap ( dynamic memory allocation).
The heap is a different space for storing data where JavaScript stores objects and functions.
Unlike the stack, the engine doesn't allocate a fixed amount of memory for these objects. Instead, more space will be allocated as needed.
Allocating memory this way is also called dynamic memory allocation.

</p>
</details>

---

### 21. What is garbage collector ?

<details><summary><b>Answer</b></summary>
<p>

#### 

In javascript, memory should be cleared up automatically using a thing called Garbage collector. Javascript is a language who ha garbage collector meaning you don't have to manage your memory manually. It gets cleared automatically & assigned automatically.

Garbage collector consists of three steps : 1. Find the root node in your code and recursively go through every child. The root node of browser code is Window object. In Nodejs , window object is global variable. 2. Mark every child including the root as active or inactive. Active means this part of code is referenced in the memory. Inactive mens it's not referenced from anywhere. 3. Delete all these inactive ones which mean if the variable i not needed anymore in the memory simple delete it. 

window.x = 10   it's a global variable. you probably heard many times is a realy bad practive to have global variables. 
window.calc  = function() => {}  calc is a fuction and imagine it does something heavy inside. That's obviously gonna stay on the root becuase root is accessing it and garbage collectors think that is always active becuase it sits on the root. 
First thing you can do use strict to prevent you from these memory leaks becuae it i going to throw errors as soon as you have global variables. Or simple not use any global variables. 

 
</p>
</details>

---

### 22. Difference Between Null & Undefined ?

<details><summary><b>Answer</b></summary>
<p>

#### 

Undefined means the variable has been declared, but its value has not been assigned. Null means an empty value or a blank value.
 
</p>
</details>

---

### 23. What will be the output of undefined==null & undefined===null ? Why ?

<details><summary><b>Answer</b></summary>
<p>

#### 

undefined==null will be true because both undefined and null represent nothingness.
undefined===null will be false because they contain different data type because undefined and null are different in terms of data type.

 typeof(undefined) // 'undefined' <br/>
 typeof(null) // 'object'
</p>
</details>

---

### 24. What is Hosting ?

<details><summary><b>Answer</b></summary>
<p>

#### 
In JavaScript, hoisting refers to the built-in behavior of the language through which declarations of functions, variables, and classes are moved to the top of their scope – all before code execution. Hoisting only happen with var and function keywords. 

  console.log(x);
  var x = 9;
  it will print undefined becuase at runtime it will be executed like the following way =:>
  var x;
  console.log(x);
  x = 9;
 
</p>
</details>

---

### 25. var vs let ?

<details><summary><b>Answer</b></summary>
<p>

#### 
![var_let](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/blob/main/images/var_let.png)
var is function scoped and let is block scoped. Variables declared by let are only available inside the block where they’re defined. Variables declared by var are available throughout the function in which they’re declared.

    console.log(x);
    var x=5;
    Output will be undefined
    
    console.log(x);
    let x=5;
    Output will be ReferenceError: Cannot access 'x' before initialization
    
    let variables are not initialized until their definition is evaluated. Accessing them before the initialization results in a ReferenceError. The variable is said to be in "temporal dead zone" from the start of the block until the initialization is processed.
![var_let](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/blob/main/images/temporal_dead_zone.png)
    
When working outside of function bodies, at a global level, let does not create a property on the global object, whereas var does. Therefore:
    // Global variables
    var x = 1;
    let y = 2;
    console.log(this.x); // will print 1
    console.log(this.y); // will print undefined
    
    {
      let name1 = "Aryan" // name1 in block scope
      console.log(name1) // Aryan
    }

    {
      var newName = "Kaush" // newName not in block scope as var is used
      console.log(newName) // Kaush
    }

    console.log(name1) // ReferenceError
    console.log(newName) // Kaush    
 
</p>
</details>

---

### 26. What is Arrow function ?

<details><summary><b>Answer</b></summary>
<p>

#### 
  Arrow functions do not have their own arguments and it uses the arguments from the outer function.
  
  - This object does not work with arrow function.
  - arguments object does not work with arrow function.
  - you cannot use new to call arrow function.
 
 ```
  const obj = {
    test() {
        console.log(this);
    }
  };
  obj.test() ==== {test: ƒ}

  const obj = {
      test:()=>{
          console.log(this);
      }
  };
  obj.test() ======== Window{0: Window, window: Window, self: Window, document: document, name: '', location: Location,…}

  function show() {
      console.log(arguments);
  }
  show(1,2,3); ======== Arguments(3)[1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]

  const show = ()=> {
      console.log(arguments);
  }
  show(1,2,3); ======== Uncaught ReferenceError: arguments is not defined
  
  solution for that --------
  const show = (...arg)=> {
      console.log(arg);
  }
  show(1,2,3); ========(3)[1, 2, 3]
  ```

</p>
</details>

---

### 27. What is IIFE(Immediately Invoked Function Expression) ?

<details><summary><b>Answer</b></summary>
<p>

#### 
a JavaScript function which calls itself, runs as soon as it is defined.
```
    (function(){
      console.log("IIFE");
    })();
```
</p>
</details>

---

### 28. What is Call, Bind, Apply ?

<details><summary><b>Answer</b></summary>
<p>

#### 
bind() - creates a new function with predefined this value and returns it.
call() and apply() execute functions immediately with the specified this value. , but call() accepts arguments individually while apply() accepts an array of arguments.

<b>Practical Scenario ::=</b>
```
  function show(obj) {
      console.log(this);
  }
  let obj = {
    a: 5
  }
```
  show(obj) will print Window {0: Window, window: Window, self: Window, document: document, name: '', location: Location, …}
  
  Now I want that show function should bind to this object and not the window object, which is the global object. So purpose of this function is at the moment I want to assign the object reference tool that this object. Now call is handy for this scenario.
  The call method takes first argument as the object to be passed to this. Then whatever parameters you want to pass, it's absolutely fine.
  
  show.call(obj) will print {a: 5}
  So in short call is used to change the reference or context are in charge value of this object.

```
  const person = {
    firstName: 'John',
    lastName: 'Doe',
    getFullName: function() {
      return `${this.firstName} ${this.lastName}`;
    }
  };

  const logName = function() {
    console.log(`Logged name: ${this.getFullName()}`);
  }
  
  // bind logName to person object
  const logPersonName = logName.bind(person); 
  
  logPersonName(); // will output "Logged name: John Doe"
```
  Now call is handy for this scenario. When we want to bind the object but will call later.
  
 
</p>
</details>

---

### 29. What are the use of Map & Set ?

<details><summary><b>Answer</b></summary>
<p>

#### 
    let product = new Map();
    product.set('pCode', '001');
    =====> Map(1) {'pCode' => '001'}
    product.set(1, '002');
    =====> Map(2) {'pCode' => '001', 1 => '002'}
    product.set(true, '003');
    =====> Map(3) {'pCode' => '001', 1 => '002', true => '003'}
    product.size  =====> 3
    product.get(1) =====> '002'
  
    let product = new Set(['one', 'two', 'three', 'two']);
    product =====> Set(3) {'one', 'two', 'three'}
    let arr = [1, 2, 3, 4, 2, 5];
    arr = [...new Set(arr)];  =====> (5) [1, 2, 3, 4, 5]
  
</p>
</details>

---

### 30. Delete/Remove a property from an Object ?

<details><summary><b>Answer</b></summary>
<p>

#### 
Using delete operator
    let emp = {
    name: "Kareem",
    age: 26,
        designation: "Software Engineer",
    }
    delete emp.age
    console.log(emp)  =====>  {name: 'Kareem', designation: 'Software Engineer'}

Using rest operator
    let emp = {
        name: "Kareem",
        age: 26,
        designation: "Software Engineer",
    }
    const {age, ...newEmp} = emp;
    console.log(newEmp);  =====>  {name: 'Kareem', designation: 'Software Engineer'}
  
</p>
</details>

---

### 31. Difference Between == (loose equality) and === (strict equality operator) in Javascript ?

<details><summary><b>Answer</b></summary>
<p>

#### 
The == operator checks if two values are equal.

    let a = '10', b = 10;    
    a == b          =====>      true
    a === b         =====>      false
  
</p>
</details>

---

### 32. Anonymous Functions in JavaScript ?

<details><summary><b>Answer</b></summary>
<p>

#### 
Anonymous functions in JavaScript, are the functions that do not have any name or identity.
    let variableName = function () {
        //your code here
    }
    variableName();     //Can call the anonymous function through this

Use of Anonymous functions : 
    button.addEventListener('click', 
        function(event) {
        alert('Button is clicked!')
    });
    
    setTimeout(function () {
        console.log('I will run after 5 seconds!')
    }, 5000);
    
    Using Anonymous Functions as Arguments of Other Functions
    function greet(wish){
      console.log(wish() ,"everyone!");
    }
    greet(function(){
      return "Good Morning";
    })
    
    Immediately Invoked Function Execution
    (function(text) {
      alert(text);
    })('Hi all! Welcome to our page.');
    
    for (var i = 1; i <= 5; i++) {
        (function (count) {
            setTimeout(function() {
                console.log(`Counted till ${count} after ${count} seconds`);
            }, 1000 * i);
        })(i);
    }
    Output:
    "Counted till 1 after 1 second"
    "Counted till 2 after 2 seconds"
    "Counted till 3 after 3 seconds"
    "Counted till 4 after 4 seconds"
    "Counted till 5 after 5 seconds"
  
</p>
</details>

---

### 33. Difference between Document Object & Window Object ?

<details><summary><b>Answer</b></summary>
<p>

#### 
the entire browser window is actually window object and the inner part where the content is displayed, all the content can be accessed using the document object. This also means that window is the main container or patterned or global object and document is child of window object operations related to entire browser.
![window](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/blob/main/images/window.png)
</p>
</details>

---

### 34. Rest Operator: Delete property from an Object using Rest operator ?

<details><summary><b>Answer</b></summary>
<p>

#### 
      let user = {
          name: 'Calvin',
          age: 200,
          country: 'Spain',
          food: 'Pizza'
      }
      const {age, ...restOfUser} = user;
      console.log(restOfUser)  // { name: 'Calvin', country: 'Spain', food: 'Pizza' }
      console.log(age)       // 200
</p>
</details>

---

### 35. What is Object destructuring?

<details><summary><b>Answer</b></summary>
<p>

#### 
Object destructuring allows us to create variables from object property names, and the variable will contain the value of the property name - for example:

    const data = { a: 1, b: 2, c: 3 };
    const { a, b, c } = data;
    console.log(a, b, c); // 1, 2, 3
    By using const { a, b, c } = data we are declaring 3 new variables (a, b and c).
    If a, b and c exist as property names on data, then variables will be created containing the values of the object properties. If the property names do not exist, you’ll get undefined.
    
    Let’s take our earlier example and use the ...rest syntax to see what happens:
    const data = { a: 1, b: 2, c: 3 };
    const { a, ...rest } = data;
    console.log(a); // 1
    console.log(rest); // { b: 2, c: 3 }

    let arr = [1, 2, 3, 4];
    let [a,,c,d] = arr;
    console.log(a,c,d); // 1 3 4

    let a = 4;
    let b = 9;
    [a,b] = [b,a];
    console.log(a,b); // 9, 4

    let arr =[1,2,3,4];
    let [a,...b] = arr;
    console.log(a); // 1 
    console.log(b); // (3) [2, 3, 4]

    let arr = [1];
    let [a,b=0] = arr;
    console.log(a,b); // 1 0    
    
</p>
</details>

---

### 36. Spread Operator : Practical use

<details><summary><b>Answer</b></summary>
<p>

#### 
    const odd = [1,3,5];
    const combined = [2,...odd, 4,6];
    console.log(combined);   // [ 2, 1, 3, 5, 4, 6 ]
    
    1. Push an array inside another array
      let rivers = ['Nile', 'Ganges', 'Yangte'];
      let moreRivers = ['Danube', 'Amazon'];
      rivers.push(...moreRivers);  // ['Nile', 'Ganges', 'Yangte', 'Danube', 'Amazon']
  
    2. Concatenate two or more arrays
      let numbers = [1, 2];
      let moreNumbers = [3, 4];
      let allNumbers = [...numbers, ...moreNumbers];
      console.log(allNumbers); // [1, 2, 3, 4]
  
    3. Copying an array
      let scores = [80, 70, 90];
      let copiedScores = [...scores];
      console.log(copiedScores); // [80, 70, 90]
    
</p>
</details>

---

### 37. Splice() method to delete existing elements, insert new elements, and replace elements in an array

<details><summary><b>Answer</b></summary>
<p>

#### 
The splice() method is used to add or remove elements of an existing array 
```
--------- Add element-----------
  let colors = ['red', 'green', 'blue'];
  colors.splice(2, 0, 'purple');
  console.log(colors); ---------- (4) ["red", "green", "purple", "blue"]
  colors.splice(1, 0, 'yellow', 'pink');
  console.log(colors); ---------- (6) ["red", "yellow", "pink", "green", "purple", "blue"]

--------- Remove element-----------
  let arr = [4, 3, 5, 9];
  arr.splice(2, 1);
  console.log(arr); ---------- (3) [4, 3, 9]

--------- Replacing elements-----------
  let languages = ['C', 'C++', 'Java', 'JavaScript'];
  languages.splice(1, 1, 'Python');
  console.log(languages);  ---------- (4) ["C", "Python", "Java", "JavaScript"]
  
```	    
</p>
</details>

---

### 38. Template Literals

<details><summary><b>Answer</b></summary>
<p>

#### 
Template literals are literals delimited with backtick ( ` ) characters, allowing for multi-line strings, string interpolation with embedded expressions, and special constructs called tagged templates.

```
`string text`

`string text line 1
 string text line 2`

`string text ${expression} string text`

tagFunction`string text ${expression} string text`

tag`string text line 1 \n string text line 2`;
```	    
</p>
</details>

---

### 39. ES6 template literals vs. concatenated strings

<details><summary><b>Answer</b></summary>
<p>

#### 
Template literals provide us with an alternative to string concatenation .They also allow us to insert variables in to a string. Template literals were introduced in ECMAScript 2015/ ES6 as a new feature. It provides an easy way to create multi-line strings and perform string interpolation.
<br><br><br>
Why would I use this new template literal method? I would recommend switching to the new format for a few reasons.
<br><br>
  - One of which is that it requires fewer characters. So the extra space that you would need to use the plus before adds extra length to your code, causing it to look more bloated
  - It no longer needs to escape single or double quotes. Yes, that’s right. You no longer need to put back slashes in order to close quotation marks.
  - it’s much easier to read. With the dollar sign curly bracket syntax you can more clearly see what parts of your strings are using a variable.At a glance, I can quickly see that this is a variable.
  - with my code editor, it would have improved syntax highlighting
<br><br><br>

Let’s us rewrite our example from concatenated string:
```
Console.log("Hello,welcome to" +website.name+"!\"message goes here\" ") ;
```
<br><br>
To template literal:
```
Console.log(`Hello,welcome to ${website.name} !"message goes here"` ) ;
```
</p>
</details>

---

### 40. Write polyfill of Array.flat with customized nesting

<details><summary><b>Answer</b></summary>
<p>

#### 

</p>
</details>

---

### 41. There are two sorted arrays. Merge them in place of the first array

<details><summary><b>Answer</b></summary>
<p>

#### 

</p>
</details>

---

### 42. Write a js function that can be invoked like below -
calc().add(10).subtract(5).multiply(20).divide(2).getResult() . In this case, the output should be 50.

<details><summary><b>Answer</b></summary>
<p>

#### 

</p>
</details>

---

### 43. prototype chain -

<details><summary><b>Answer</b></summary>
<p>

#### 
The prototype chain is a mechanism that allows objects to inherit properties and methods from other objects. Every object can have exactly one prototype object. That prototype object can also have a prototype object, and so on, creating a chain of inheritied properties and methods. The end of this chain is called the null prototype.
Every object in JavaScript can be linked to a prototype object which is the mechanism through which inheritance is provided.
You can see the prototype an object is pointing to using the __proto__ property.

```
// Define a human constructor
function Person(name, age) {
    this.name = name;
    this.age = age;
}

// Add a method for greeting
Person.prototype.greet = function() {
    console.log(`Hi, my name is ${this.name} and I'm ${this.age} years old.`);
};

// Create a human instance
const person = new Person('John', 30);

// Access the person's name attribute and output "John"
console.log(person.name);

// Access the greet method of person and output "Hi, my name is John and I'm 30"
person.greet()
```
</p>
</details>

---

### 44. What is the first class function in JavaScript ?

<details><summary><b>Answer</b></summary>
<p>

#### 
A programming language is said to have First-class functions if functions in that language are treated like other variables. So the functions can be assigned to any other variable or passed as an argument or can be returned by another function. This means that functions are simply a value and are just another type of object.

<b>Usage of First-Class Function</b>
  -  It can be stored as a value in a variable.
  -  It can be returned by another function.
  -  It can be passed into another function as an Argument.
  -  It can also stored in an array, queue, or stack.
  -  It can have its own methods and property.

```
  const Arithmetics = {
    add: (a, b) => {
        return `${a} + ${b} = ${a + b}`;
    },
    subtract: (a, b) => {
        return `${a} - ${b} = ${a - b}`
    },
    multiply: (a, b) => {
        return `${a} * ${b} = ${a * b}`
    },
    division: (a, b) => {
        if (b != 0) return `${a} / ${b} = ${a / b}`;
        return `Cannot Divide by Zero!!!`;
    }

}

console.log(Arithmetics.add(100, 100));
console.log(Arithmetics.subtract(100, 7))
console.log(Arithmetics.multiply(5, 5))
console.log(Arithmetics.division(100, 5));


const Geek = (a, b) => {
    return (a + " " + b);
}

console.log(Geek("Akshit", "Saxena"));
```

</p>
</details>

---

### 45. What is a First Order Function in JavaScript ?

<details><summary><b>Answer</b></summary>
<p>

#### 
A First Order Function is just a normal regular function that does not take any function as one of its parameters and also it does not return another function as its return value inside its body. It is a simple function that accepts the parameters of different data types either primitive or non-primitive and it may or may not return a value as a result of the calculations performed on the passed parameters. A First Order Function can be declared using any of the methods available in JavaScript.

```
function firstOrderFunc(num1, num2){
    console.log(num1*num2);
}
firstOrderFunc(4, 35)
```

</p>
</details>

---

### 45. What is Higher Order Function in JavaScript ?

<details><summary><b>Answer</b></summary>
<p>

#### 
A higher-order function is a function that can accept a function as one of its parameters and it can also return a function as its return value or it can do both return as well as accept a function. A higher-order function can also take as well as return other types of values but it either has to take a function as a parameter or return a function as its return value with them. It can be declared using any syntax available in JavaScript to declare functions.

```
function higherOrderFunc(a){
    return function(b){
        console.log(a*b)
    }
}
higherOrderFunc(3)(53);
```

</p>
</details>

---

### 46. memoization in javascript

<details><summary><b>Answer</b></summary>
<p>

#### 
Memoization is a technique for speeding up applications by caching the results of expensive function calls and returning them when the same inputs are used again.

<b>Importance of Memoization:</b> When a function is given in input, it performs the necessary computation and saves the result in a cache before returning the value. If the same input is received again in the future, it will not be necessary to repeat the process. It would simply return the cached answer from the memory. This will result in a large reduction in a code’s execution time.

<b>Memoization in Javascript:</b> In JavaScript, the concept of memorization is based mostly on two ideas. They are as follows:
  -  Closures
  -  Higher-Order Functions

```
// Memoizer
const memoize = () => {
 
    // Create cache for results
    // Empty objects
    const results = {};
 
    // Return function which takes
    // any number of arguments
    return (...args) => {
        // Create key for cache
        const argsKey = JSON.stringify(args);
 
        // Only execute func if no cached val
        // If results object does not have 
        // anything to argsKey position
        if (!results[argsKey]) {
            results[argsKey] = func(...args)
        }
        return results[argsKey];
    };
};
 
// Wrapping memoization function
const multiply = memoize((num1, num2) => {
    let total = 0;
    for (let i = 0; i < num1; i++) {
        for (let j = 0; j < num1; j++) {
 
            // Calculating square
            total += 1 *;
        }
    }
 
    // Multiplying square with num2
    return total * num2;
});
 
console.log(multiply(500, 5000));
```
https://www.geeksforgeeks.org/how-to-write-a-simple-code-of-memoization-function-in-javascript/?ref=asr2

</p>
</details>

---

### 47. Dynamic object create ?

<details><summary><b>Answer</b></summary>
<p>

#### 
```
function createObj(obj, field, value) {
    if (Object.keys(obj).includes(field)) {
        Object.assign(obj, {[field]: value});
    } else {
        obj[field] = value;
    }
    return obj;
}

createObj({}, 'one', 'value1')
```
 
</p>
</details>

---

### 48. Reduce operator examples

<details><summary><b>Answer</b></summary>
<p>

#### 
```
const arr =[[1,2], [3,4], [5,6]];
const finalArr = arr.reduce((accumulator, current)=> accumulator.concat(current));
console.log(finalArr); // (6) [1, 2, 3, 4, 5, 6]

Sum of numbers
---------------------------------------------------------------
const numbers = [2, 4, 6, 8];
const result = numbers.reduce((accumulator, current)=> accumulator + current);
console.log(result); // 20

Average of numbers
---------------------------------------------------------------
const numbers = [2, 4, 6, 8];
const result = numbers.reduce((accumulator, current, index, arr)=> {
	accumulator+=current;
    if(index === arr.length-1)  {
    	return accumulator/arr.length;
    }
    return accumulator;
});
console.log(result); // 5

Average of numbers with initial value 100
---------------------------------------------------------------
const numbers = [2, 4, 6, 8];
const result = numbers.reduce((accumulator, current, index, arr)=> {
	accumulator+=current;
    if(index === arr.length-1)  {
    	return accumulator/arr.length;
    }
    return accumulator;
}, 100);
console.log(result); // 30

 ```
</p>
</details>

---

### 49. Arrow vs Normal functions ?

<details><summary><b>Answer</b></summary>
<p>

#### 
<strong>Access arguments</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
In regular functions, you can access all passed arguments using the arguments object, which is an array-like object containing each argument passed to the function.
```
function showArgs() {
	console.log(arguments);
}
showArgs(1, 2, 3);  // [Arguments] { '0': 1, '1': 2, '2': 3 }
```
Arrow functions do not have their own arguments object. To access arguments in arrow functions, use rest parameters (…args) to collect all arguments into an array.
```
const showArgs = (...args) => {
    console.log(args);
};
showArgs(1, 2, 3);  // [ 1, 2, 3 ]
```

<strong>Duplicate named parameters</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
In regular functions, duplicate named parameters are allowed but not recommended. The last occurrence of the parameter overwrites previous ones, and only its value is used.
```
function example(a, b, a) {
    console.log(a, b);
}
example(1, 2, 3); // 3 2
```

Arrow functions do not allow duplicate named parameters, even in non-strict mode, and will throw a syntax error if duplicates are present. Always use unique parameter names in arrow functions.
```
const example = (a, b, a) => {
    console.log(a);
}; 
// SyntaxError
```

<strong>Hosting</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
In regular functions, function declarations are hoisted to the top of their scope, allowing them to be called before they’re defined in the code.
```
greet(); // Output: Hello Geeks!

function greet() {
    console.log('Hello Geeks!');
}
```

Arrow functions are not hoisted like regular function declarations. They are treated as variables, so they cannot be called before being defined due to the temporal dead zone.
```
greet(); // ReferenceError: Cannot access 'greet' before initialization

const greet = () => {
    console.log('Hello!');
};
```

<strong>this keyword</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
In regular functions, this refers to the object that calls the function (runtime binding). Its value can vary based on how the function is called (method, event, or global).
```
const obj = {
    name: 'Geeks',
    greet: function() {
        console.log(this.name);
    }
};
obj.greet();
```

In arrow functions, this is lexically inherited from the surrounding scope, not the function itself.
```
const obj = {
    name: 'Geeks',
    greet: () => {
        console.log(this.name);
    }
};
obj.greet(); // Output: undefined (inherited from outer scope)
```
  
</p>
</details>

---
