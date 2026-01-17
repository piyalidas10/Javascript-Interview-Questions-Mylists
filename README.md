# Javascript-Interview-Questions
Javascript Interview Questions

##### 1. Pure function vs Impure functions

<details><summary><b>Answer</b></summary>
<p>

###### 

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

#### 2. What is Javascript closure ?

<details><summary><b>Answer</b></summary>
<p>

  ##### 
  https://medium.com/@piyalidas.it/closure-in-javascript-2496fdedeb7d
  https://dev.to/imranabdulmalik/mastering-closures-in-javascript-a-comprehensive-guide-4ja8#:~:text=The%20Factory%20Function%20Pattern%20uses,without%20explicitly%20class%2Dbased%20syntax.
  
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

#### 3. Javascript Event Loop

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 4. Slice vs Splice

<details><summary><b>Answer</b></summary>
<p>

##### 

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

#### 5. Spread vs Rest

<details><summary><b>Answer</b></summary>
<p>

##### 

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

#### 5. Event Propagation ( Event Bubbling and capturing )

<details><summary><b>Answer</b></summary>
<p>
  
##### 
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


#### 6. Top 10 Features of ES6 

<details><summary><b>Answer</b></summary>
<p>

##### 

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

#### 7. What is Recursion?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 8. What is Temporal Dead Zone?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 9. map() vs forEach()

<details><summary><b>Answer</b></summary>
<p>

##### 
The map() method is used to transform the elements of an array, whereas the forEach() method is used to loop through the elements of an array. The map() method can be used with other array methods, such as the filter() method, whereas the forEach() method cannot be used with other array methods.

</p>
</details>

---

#### 10. What is prototype in javascript

<details><summary><b>Answer</b></summary>
<p>

##### 
Prototypes are the mechanism by which JavaScript objects inherit features from one another. The prototype is an object that is associated with every functions and objects by default in JavaScript. 
When a programmer needs to add new properties like variables and methods at a later point of time, and these properties need sharing across all the instances, then the prototype will be very handy.

</p>
</details>

---

#### 11. When to use Prototype in JavaScript?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 12. prototype inheritance in javascript

<details><summary><b>Answer</b></summary>
<p>

##### 
Prototype inheritance in javascript is the linking of prototypes of a parent object to a child object to share and utilize the properties of a parent class using a child class.
The syntax used for prototype inheritance has the __proto__ property which is used to access the prototype of the child. The syntax to perform a prototype inheritance is as follows :

child.__proto__ = parent;

```
let animal = {
    animalEats: true,
};

let rabbit = {
    rabbitJumps: true,
};

// Sets rabbit.[[Prototype]] = animal
rabbit.__proto__ = animal;
console.log(rabbit.animalEats); // true
console.log(rabbit.rabbitJumps); // true
```

</p>
</details>

---

#### 13. Remove duplicate values from Array

<details><summary><b>Answer</b></summary>
<p>

##### 
```
arr = [1, 2,3,4,5,6,2,3,4];
finalArr = [...new Set(arr)];  ///////  [1, 2, 3, 4, 5, 6]

arr = [1, 2, 3, 3, 4, 5, 6];
const finalArr1 = arr.reduce((acc, curElm) => {
    return acc.includes(curElm)? acc : [...acc, curElm]
},[]);
console.log(finalArr1); // [1, 2, 3, 4, 5, 6]
const finalArr2 = arr.reduce((acc, curElm) => {
    return acc.includes(curElm)? acc : acc.concat(curElm)
},[]);
console.log(finalArr2); // [1, 2, 3, 4, 5, 6]
```

</p>
</details>

---

#### 14. Removing duplicate objects (based on multiple keys) from array

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 15. Why JavaScript is a single-thread language ? 

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 16. How can i make array immutable in javascript

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 17. How can i make object immutable in javascript

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 18. How can reduce Memory Leaks in Javascript ?

<details><summary><b>Answer</b></summary>
<p>

##### 
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
#### 19. Why JavaScript is single threaded?

<details><summary><b>Answer</b></summary>
<p>

##### 

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

#### 20. Primitive vs Non-Primitive

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 21. What is garbage collector ?

<details><summary><b>Answer</b></summary>
<p>

##### 

In javascript, memory should be cleared up automatically using a thing called Garbage collector. Javascript is a language who ha garbage collector meaning you don't have to manage your memory manually. It gets cleared automatically & assigned automatically.

Garbage collector consists of three steps : 1. Find the root node in your code and recursively go through every child. The root node of browser code is Window object. In Nodejs , window object is global variable. 2. Mark every child including the root as active or inactive. Active means this part of code is referenced in the memory. Inactive mens it's not referenced from anywhere. 3. Delete all these inactive ones which mean if the variable i not needed anymore in the memory simple delete it. 

window.x = 10   it's a global variable. you probably heard many times is a realy bad practive to have global variables. 
window.calc  = function() => {}  calc is a fuction and imagine it does something heavy inside. That's obviously gonna stay on the root becuase root is accessing it and garbage collectors think that is always active becuase it sits on the root. 
First thing you can do use strict to prevent you from these memory leaks becuae it i going to throw errors as soon as you have global variables. Or simple not use any global variables. 

 
</p>
</details>

---

#### 22. Difference Between Null & Undefined ?

<details><summary><b>Answer</b></summary>
<p>

##### 

Undefined means the variable has been declared, but its value has not been assigned. Null means an empty value or a blank value.
 
</p>
</details>

---

#### 23. What will be the output of undefined==null & undefined===null ? Why ?

<details><summary><b>Answer</b></summary>
<p>

##### 

undefined==null will be true because both undefined and null represent nothingness.
undefined===null will be false because they contain different data type because undefined and null are different in terms of data type.

 typeof(undefined) // 'undefined' <br/>
 typeof(null) // 'object'
</p>
</details>

---

#### 24. What is Hosting ?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 25. var vs let ?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 26. What is Arrow function ?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 27. What is IIFE(Immediately Invoked Function Expression) ?

<details><summary><b>Answer</b></summary>
<p>

##### 
a JavaScript function which calls itself, runs as soon as it is defined. This pattern encapsulates code within its own scope, preventing variable pollution in the global scope and enabling more controlled and modular code organization. 
```
    (function(){
      console.log("IIFE");
    })();
```

<strong>Use Cases</strong><br/>
<strong>Encapsulation:</strong> IIFEs are commonly used to encapsulate variables and functions, preventing them from polluting the global scope. This is particularly useful in large applications or when integrating third-party scripts.<br/>
<strong>Avoiding Variable Collision:</strong> By creating a separate scope, IIFEs help prevent variable names from conflicting with those in other scripts or libraries, thus improving code robustness.<br/>
<strong>Module Pattern:</strong> Before ES6 introduced modules, IIFEs were often used to create modules with private and public methods, providing a way to structure code and manage dependencies effectively.<br/>

<strong>Module Pattern Before ES6</strong>
```
var module = (function() {
    var privateVariable = 'I am private';

    function privateFunction() {
        return 'Private function';
    }

    return {
        publicVariable: 'I am public',
        publicFunction: function() {
            return 'Public function calls ' + privateFunction();
        }
    };
})();

console.log(module.publicVariable); // Outputs: I am public
console.log(module.publicFunction()); // Outputs: Public function calls Private function
```
Here, privateVariable and privateFunction() are encapsulated within the IIFE’s scope, while publicVariable and publicFunction() are exposed and accessible outside.

<strong>For Loop with var Before ES6</strong>
Another common use case of IIFEs was to handle for loops with var, preventing unintended closures:
```
for (var i = 0; i < 5; i++) {
    (function(index) {
        setTimeout(function() {
            console.log(index);
        }, 1000);
    })(i);
}
```

</p>
</details>

---

#### 28. What is Call, Bind, Apply ?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 29. What are the use of Map & Set ?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 30. Delete/Remove a property from an Object ?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 31. Difference Between == (loose equality) and === (strict equality operator) in Javascript ?

<details><summary><b>Answer</b></summary>
<p>

##### 
The == operator checks if two values are equal.

    let a = '10', b = 10;    
    a == b          =====>      true
    a === b         =====>      false
  
</p>
</details>

---

#### 32. Anonymous Functions in JavaScript ?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 33. Difference between Document Object & Window Object ?

<details><summary><b>Answer</b></summary>
<p>

##### 
the entire browser window is actually window object and the inner part where the content is displayed, all the content can be accessed using the document object. This also means that window is the main container or patterned or global object and document is child of window object operations related to entire browser.
![window](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/blob/main/images/window.png)
</p>
</details>

---

#### 34. Rest Operator: Delete property from an Object using Rest operator ?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 35. What is Object destructuring?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 36. Spread Operator : Practical use

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 37. Splice() method to delete existing elements, insert new elements, and replace elements in an array

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 38. Template Literals

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 39. ES6 template literals vs. concatenated strings

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 40. Write polyfill of Array.flat with customized nesting

<details><summary><b>Answer</b></summary>
<p>

##### 

</p>
</details>

---

#### 41. There are two sorted arrays. Merge them in place of the first array

<details><summary><b>Answer</b></summary>
<p>

##### 
```
arr1 = [1, 2 , 3];
arr2 = [4, 5, 6];
arr1 = arr1.concat(arr2);
arr1 = [...arr1,...arr2];
```
</p>
</details>

---

#### 42. Write a js function that can be invoked like below -
calc().add(10).subtract(5).multiply(20).divide(2).getResult() . In this case, the output should be 50.

<details><summary><b>Answer</b></summary>
<p>

##### 

</p>
</details>

---

#### 43. prototype chain -

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 44. What is the first class function in JavaScript ?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 45. What is a First Order Function in JavaScript ?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 45. What is Higher Order Function in JavaScript ?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 46. memoization in javascript

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 47. Dynamic object create ?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 48. Reduce operator examples

<details><summary><b>Answer</b></summary>
<p>

##### 
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

#### 49. Arrow vs Normal functions ?

<details><summary><b>Answer</b></summary>
<p>

##### 
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

<strong>new keyword</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
Regular functions can be used as constructors with the new keyword, allowing the creation of new object instances. The new keyword sets this to the new object inside the function.
```
function Person(name) {
    this.name = name;
}

const p = new Person('Geeks'); // Creates a new Person object
console.log(p.name); // Geeks
```
Arrow functions cannot be used as constructors and do not support the new keyword. Attempting to use new with an arrow function will result in a TypeError.
```
const Person = () => {};
const p = new Person(); // TypeError: Person is not a constructor
```
  
</p>
</details>

---


#### 50. Passing by Reference vs. Passing by Value ?

<details><summary><b>Answer</b></summary>
<p>

##### 

<strong>Pass by Reference or Call by Reference</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
When a parameter is passed by reference, the caller and the callee use the same variable for the parameter. If the callee modifies the parameter variable, the effect is visible to the caller's variable.
Pass by Reference will work with Non Primitive data types like arrays, objects, functions.
Instead of making a copy, pass-by-reference does exactly what it sounds like; a value stored in memory gets referenced.

```
let a = 5;
let b = a;
console.log(a); // 5
console.log(b); // 5

b = a + 5;
console.log(a); // 5
console.log(b); // 10
```
both variables a & b will work independently. 

<strong>Pass by Value or Call by Value</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
When a parameter is passed by value, the caller and callee have two independent variables with the same value. If the callee modifies the parameter variable, the effect is not visible to the caller.
Pass by Value will work with Primitive data types like Boolean, char, byte, int, short, long, float, and double.
pass-by-value creates a new space in memory and makes a copy of a value, whereas pass-by-reference does not.

```
let obj1 = { name: 'Piyali'};
let obj2 = obj1;
console.log(obj1); // { name: 'Piyali'}
console.log(obj2); // { name: 'Piyali'}

obj2 = {name: 'Test'};
console.log(obj1); // { name: 'Test'}
console.log(obj2); // { name: 'Test'}
```
Here with let obj2 = obj1, we are giving reference of obj1 to obj2. So, any changes in value of obj2 will change the original value of obj1.
Same thing will happen for array also.

<br/> Hindi Tutorial youtube link : https://www.youtube.com/watch?v=8y9c4SumvcQ
</p>
</details>

---

#### 51. Remove Null and Undefined Values from an Object (Practical Coding)

<details><summary><b>Answer</b></summary>
<p>

##### 
```
let obj = {
  a: 'test',
  b: null,
  c: undefined,
  d: 'hi'
};

Object.entries(obj).filter(([key, value]) => value != null)
print in console -------------
(2) [Array(2), Array(2)]
0: (2) ['a', 'test']
1: (2) ['d', 'hi']

Object.fromEntries(Object.entries(obj).filter(([key, value]) => value != null))
print in console -------------
{a: 'test', d: 'hi'}

```

Why have written "value != null"?
Ans. The data type of undefined & null are not same. Data type of undefined is undefined. Data type of null is object. You can check using typeof operator.

console.log(undefined == null) // true
console.log(undefined === null) // false
 
</p>
</details>

---

#### 52. Convert an Array to an Object

<details><summary><b>Answer</b></summary>
<p>

##### 

<strong>Single Dimention Array into an Object</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
```
Object.assign({}, ['a','b','c']);
{0: 'a', 1: 'b', 2: 'c'}
```

<strong>Array of Key-value pairs into an Object</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
```
let a = [['JS', 'JavaScript'], ['GFG', 'GeeksforGeeks']];
let obj = Object.fromEntries(a);
{ JS: 'JavaScript', GFG: 'GeeksforGeeks' }
```

<strong>Spread Operator</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
```
const arr = ["Geeks", "for", "Geeks"];
const obj = {...arr};
console.log(obj); //  '0': 'Geeks', '1': 'for', '2': 'Geeks' }
```

<strong>Using forEach</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
```
const arr = ["Geeks", "for", "Geeks"];
const obj = {};
arr.forEach((value, index) => obj[index] = value);
console.log(obj); // { '0': 'Geeks', '1': 'for', '2': 'Geeks' }
```

<strong>Using Reduce</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
```
const a = ['Hello', 'World', 'Welcome', 'To', 'JavaScript'];

const obj = a.reduce((acc, current, index) => {
    acc[index] = current;
    return acc;
}, {});

console.log(JSON.stringify(obj)); // {"0":"Hello","1":"World","2":"Welcome","3":"To","4":"JavaScript"}
```
 
</p>
</details>

---

#### 53. Convert Map keys to an array

<details><summary><b>Answer</b></summary>
<p>

##### 
<strong>Using array.from() Method</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
```
let myMap = new Map().set('GFG', 1).set('Geeks', 2);
let array = Array.from(myMap.keys());
console.log(myMap.keys()); // MapIterator {'GFG', 'Geeks'}
console.log(array); // (2) ['GFG', 'Geeks']
```

<strong>Using Object.keys() and Object.fromEntries()</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
```
const myMap = new Map([
    ["JavaScript", 1],
    ["Python", 2],
    ["C++", 3]
]);

// Convert Map to Object
let obj = Object.fromEntries(myMap); // {JavaScript: 1, Python: 2, C++: 3}

// Extract keys from Object
let keysArray = Object.keys(obj);

console.log(keysArray); // ['JavaScript', 'Python', 'C++']
```
 
</p>
</details>

---

#### 54. How to serialize a Map in JavaScript ?

<details><summary><b>Answer</b></summary>
<p>

##### 
Serialization is the conversion of an object or a data structure to another format that is easily transferrable on the network.

<strong>Using Array.from() and JSON.stringify() Method</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
```
const map1 = new Map([
    [1, 2],
    [2, 3],
    [4, 5]
]);

Array.from(map1); // Convert the map into an array using Array.from() method
(3) [Array(2), Array(2), Array(2)]
0: (2) [1, 2]
1: (2) [2, 3]
2: (2) [4, 5]

JSON.stringify(Array.from(map1)) // Use the JSON.stringify() method on this converted array to serialize it.
'[[1,2],[2,3],[4,5]]'

```

<strong>Using Object.fromEntries() and JSON.stringify() Method</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
```
const map1 = new Map([
    [1, 2],
    [2, 3],
    [4, 5]
]);

Object.fromEntries(map1); // Convert the map into an object using Object.fromEntries() method
{1: 2, 2: 3, 4: 5}

JSON.stringify(Object.fromEntries(map1)); // Use the JSON.stringify() method on this converted object to serialize it.
'{"1":2,"2":3,"4":5}'
```

</p>
</details>

---

#### 55. Object vs JavaScript Map ?

<details><summary><b>Answer</b></summary>
<p>

##### 
A Map in JavaScript is a collection of key-value pairs where keys can be any data type. Unlike objects, keys in a Map maintain insertion order. It provides methods to set, get, delete, and iterate over elements efficiently, making it useful for data storage and retrieval tasks.

<strong>Insertion Order</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
S6 Maps preserves the insertion order. A Map will keep the order of the inserted keys regardless of their type.
```
const myData = new Map();
myData.set(10427, "Description 10427");
myData.set(10504, "Description 10504");
myData.set(10419, "Description 10419");
console.log(myData); // Map(3) {10427 => 'Description 10427', 10504 => 'Description 10504', 10419 => 'Description 10419'}
```

const obj = {};
Object.assign(obj, {10427: "Description 10427"}); // {10427: 'Description 10427'}
Object.assign(obj, {10504: "Description 10504"}); // {10427: 'Description 10427', 10504: 'Description 10504'}
Object.assign(obj, {10419: "Description 10419"}); // {10419: 'Description 10419', 10427: 'Description 10427', 10504: 'Description 10504'}

<strong>Keys</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
A Map 's keys can be any value (including functions, objects, or any primitive). A Map holds key-value pairs where the keys can be any datatype. Map has no restriction over key names. 
The keys of an Object must be either a String or a Symbol.

<strong>Perforamce by memory heap</strong>
---------------------------------------------------------------------------------------------------------------------------------------------------
The V8 engine heap isn't very good at managing arrays of objects. If you have an array of 100,000 objects, loop through each object and delete one parameter in the middle then re-add it, the heap size will spike rather than stay constant.

// Spike the memory heap
arr.forEach(obj => {let temp = obj[0]; delete obj[0]; obj[0] = temp})
The reason the heap will spike is because V8 creates intermediate arrays during the forEach loop over the array. Once the array you're looping over becomes massive (eg. 100,000 elements) the memory spike will be too large to handle and could crash the program.

Iterables like Map and Set aren't handled by the engine this way, and you can use a library like iter-tools to get array-like transformation methods for them.
 
</p>
</details>

---

#### 56. Can we add Javascript function in JSON?

<details><summary><b>Answer</b></summary>
<p>

##### 
https://www.geeksforgeeks.org/how-to-store-a-javascript-fnction-in-json/
 
</p>
</details>

---

#### 57. Ho can add Dynamic key in Object?

<details><summary><b>Answer</b></summary>
<p>

##### 
```
let tv = 'pCode', num = 10;
let obj = {
    [tv]: 1001,
    pName: 1000,
    ['get' + num]() {
        console.log(pName);
    }
}

console.log(obj) ==============
{pCode: 1001, pName: 1000, get10: ƒ}
get10: ƒ ['get' + num]()
pCode: 1001
pName: 1000
```
 
</p>
</details>

---

#### 58. What will the output of bellow ?
function show() {
    console.log(this);
}
show();

let obj = {
    display: function() {
        console.log(this);
    }
}
obj.display();

let obj = {
    display: ()=> {
        console.log(this);
    }
}
obj.display();

<details><summary><b>Answer</b></summary>
<p>

##### 
```
function show() {
    console.log(this);
}
show(); // Window {0: global, window: Window, self: Window, document: document, name: '', location: Location, …}

let obj = {
    display: function() {
        console.log(this);
    }
}
obj.display(); // {display: ƒ}

let obj = {
    display: ()=> {
        console.log(this);
    }
}
obj.display(); // Window {0: global, window: Window, self: Window, document: document, name: '', location: Location, …}
```
 
</p>
</details>

---

#### 59. What Are Stack and Heap?

<details><summary><b>Answer</b></summary>
<p>

##### 
Stack: The Stack is used for static memory allocation, primarily for storing primitive types and function calls. It's a simple, last-in, first-out (LIFO) structure, making it very fast to access.

Heap: The Heap is used for dynamic memory allocation, where objects and arrays (non-primitive types) are stored. Unlike the Stack, the Heap is more complex and slower to access, as it allows for flexible memory allocation.

```
---------------------------------------------------------------------------------------------
Example of Stack Memory
---------------------------------------------------------------------------------------------
let myYoutubeName = "ayushyadavz"; // Primitive type stored in the Stack.
let anotherName = myYoutubeName;   // A copy of the value is created in the Stack.
anotherName = "amanyadavz";        // Changing the copy does not affect the original.

console.log(myYoutubeName); // Output: ayushyadavz (Original value remains unchanged)
console.log(anotherName);   // Output: amanyadavz (Only the copy value is changed)

---------------------------------------------------------------------------------------------
Example of Heap Memory
---------------------------------------------------------------------------------------------
let userOne = {         // The reference to this object is stored in the Stack.
    email: "user@google.com",
    upi: "user@ybl"
};                      // The actual object data is stored in the Heap.

let userTwo = userOne;  // userTwo references the same object in the Heap.

userTwo.email = "ayush@google.com"; // Modifying userTwo also affects userOne.

console.log(userOne.email); // Output: ayush@google.com
console.log(userTwo.email); // Output: ayush@google.com
```
 
</p>
</details>

---

#### 60. What's the output of console.log(this) in JavaScript?

<details><summary><b>Answer</b></summary>
<p>

##### 
```
console.log(this);
------------------------------------------------------------------------------------------------------
Window {0: global, window: Window, self: Window, document: document, name: '', location: Location, …}
```
 
</p>
</details>

---

#### 61. Write a program using Promise and Async/Await.

<details><summary><b>Answer</b></summary>
<p>

##### 
```
async function fetchData() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
  const data = await response.json();
  console.log(data);
}

fetchData();
------------------------------------------------------------------------------------------------------
{
  userId: 1,
  id: 1,
  title: ....',
  body: ....}


async function fetchData() {
  try {
    let response = await fetch('https://api.example.com/data');
    let data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Error fetching data:', error);
  }
}
```

Advantages of Async and Await
------------------------------------------------------------------------------------------------------
Improved Readability: Async and Await allow asynchronous code to be written in a synchronous style, making it easier to read and understand.
Error Handling: Using try/catch blocks with async/await simplifies error handling.
Avoids Callback Hell: Async and Await prevent nested callbacks and complex promise chains, making the code more linear and readable.
Better Debugging: Debugging async/await code is more intuitive since it behaves similarly to synchronous code.
 
</p>
</details>

---

#### 62. How would you efficiently handle 5000 records from an API call for a dropdown?

<details><summary><b>Answer</b></summary>
<p>

##### 
 
</p>
</details>

#### 62. How is Async/Await different from Promises?

<details><summary><b>Answer</b></summary>
<p>

##### 
In JavaScript, promises and async/await are two different ways to handle asynchronous operations. But they are closely related.

Promise
------------------------------------------------------------------------------------------------------
A promise is an object that eventually leads to an asynchronous operation’s completion or failure. A promise can be in one of three states: pending, fulfilled, or rejected. When the asynchronous operation is completed, the Promise will either be fulfilled with a value or rejected with an error.

Async/Await
------------------------------------------------------------------------------------------------------
Async/await is a syntactic sugar on top of promises. It provides a more concise way to write asynchronous code, making it easier to read and write. With Async/Await, you can write asynchronous code that looks similar to synchronous code, and it uses promises under the hood. In async/await, the async keyword is used to declare an asynchronous function. The await keyword is used to wait for a promise to be resolved before continuing with the execution of the function. The await keyword can only be used inside an async function.

Difference
------------------------------------------------------------------------------------------------------
The only difference is the execution context between promise and async/await.
When a Promise is created and the asynchronous operation is started, the code after the Promise creation continues to execute synchronously. When the Promise is resolved or rejected, the attached callback function is added to the microtask queue. The microtask queue is processed after the current task has been completed but before the next task is processed from the task queue. This means that any code that follows the creation of the Promise will execute before the callback function attached to the Promise is executed.
On the other hand, with Async/Await, the await keyword causes the JavaScript engine to pause the execution of the async function until the Promise is resolved or rejected. While the async function waits for the Promise to resolve, it does not block the call stack, and any other synchronous code can be executed. Once the Promise is resolved, the execution of the async function resumes, and the result of the Promise is returned. If rejected, it throws an error value.

------------------------------------------------------------------------------------------------------
https://medium.com/version-1/difference-between-promise-and-async-await-95e453182f55
 
</p>
</details>

---

#### 63. Explain Node.js and the Event Loop.

<details><summary><b>Answer</b></summary>
<p>

##### 
In Node, the Event Loop is a mechanism that handles asynchronous operations. It allows Node to perform non-blocking I/O operations despite the fact that JavaScript is single-threaded. It continuously checks the call stack and message queue, executes tasks from stack processing asynchronous operations.
https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick

</p>
</details>

---

#### 64. How does this behave in Node.js? Is it the same as in the browser?

<details><summary><b>Answer</b></summary>
<p>

##### 
In Node. js all modules (script files) are executed in their own closure while browsers execute all script files directly within the global scope.

In the browser, running in the global scope, this is always window in your example
```
var obj1;
var obj2;

function x() {
    obj1 = this; // window
}

function y() {
    obj2 = this; // window
}

x();
y();

console.log(obj1 === obj2);  // window === window = true
console.log(obj1 === this);  // window === window = true
```

This is not how it works in Node. In Node.js all modules (script files) are executed in their own closure while browsers execute all script files directly within the global scope.

In other words, in just about any file running in Node, this will just be an empty object, as Node wraps the code in an anonymous function that is called immediately, and you'd access the global scope within that context with GLOBAL instead.

However, when calling a function without a specific context in Node.js, it will normally be defaulted to the global object - The same GLOBAL mentioned earlier, as it's execution context.

So outside the functions, this is an empty object, as the code is wrapped in a function by Node, to create it's own execution context for every module (script file), while inside the functions, because they are called with no specified execution context, this is the Node GLOBAL object

In Node.js you'd get
```
var obj1;
var obj2;

function x() {
    obj1 = this; // GLOBAL
}

function y() {
    obj2 = this; // GLOBAL
}

x();
y();

console.log(obj1 === obj2);  // GLOBAL === GLOBAL = true
console.log(obj1 === this);  // GLOBAL === {} = false
```
</p>
</details>

---

#### 65. Write code for mul(2)(3)(4) = 24.

<details><summary><b>Answer</b></summary>
<p>

##### 
```
function mul(x) { 
      return function(y) { 
        return function(z) { 
          return x*y*z; 
        }; 
      } 
    } 
      
    console.log(mul(2)(3)(5));  // 30
    console.log(mul(2)(3)(4)); // 24
```
</p>
</details>

---

#### 66. Solution to 25 JavaScript interview questions.

<details><summary><b>Answer</b></summary>
<p>

##### 
https://medium.com/@dynamicsa420/solution-to-25-javascript-interview-questions-4162a1f76609

</p>
</details>

---

#### 66. What is Axios ?

<details><summary><b>Answer</b></summary>
<p>

##### 
Axios is a promise-based HTTP Client for node.js and the browser. It is isomorphic (= it can run in the browser and nodejs with the same codebase). On the server-side it uses the native node.js http module, while on the client (browser) it uses XMLHttpRequests.
https://axios-http.com/docs/intro

</p>
</details>

---

#### 67. What is Axios ?

<details><summary><b>Answer</b></summary>
<p>

##### 
Prototype inheritance in javascript is the linking of prototypes of a parent object to a child object to share and utilize the properties of a parent class using a child class. Prototypes are hidden objects that are used to share the properties and methods of a parent class with child classes.

The syntax used for prototype inheritance has the proto property which is used to access the prototype of the child. The syntax to perform a prototype inheritance is as follows : 
```
child.__proto__ = parent;

// Creating a parent object as a prototype
const parent = {
  greet: function() {
    console.log(`Hello from the parent`);
  }
};

// Creating a child object
const child = {
  name: 'Child Object'
};

// Performing prototype inheritance
child.__proto__ = parent;

// Accessing the method from the parent prototype
child.greet(); // Outputs: Hello from the parent 
```

```
// Creating a prototype object
const personPrototype = {
  introduce: function() {
    console.log(`Hi, my name is ${this.name} and I am ${this.age} years old.`);
  }
};

// Creating a new object that inherits from the personPrototype
const john = Object.create(personPrototype);
john.name = 'John';
john.age = 30;

// Calling the introduce method on the john object
john.introduce(); 
// Outputs: Hi, my name is John and I am 30 years old.
```

```
// Constructor function for creating Person objects
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// Creating an instance of the Person object
const person = new Person('John', 30);

// Accessing the constructor property of the prototype
console.log(person.constructor); 
// Outputs: ƒ Person(name, age) { this.name = name; this.age = age;}
```

The prototype chain
```
let student = {
  id: 1,
};
let tution = {
  id: 2,
};
let school = {
  id: 3,
};
student.__proto__ = school; //level1 inheritance
student.__proto__.__proto__ = tution; //level2 inheritance
console.log(student.id); //the student object's property
console.log(student.__proto__.id); //school object's property
```
</p>
</details>

---

#### 68. did you know that javascript behaves differently in the browser and in the Nodejs ?

<details><summary><b>Answer</b></summary>
<p>

##### 
Node.js and Web browsers are two different but interrelated technologies in web development. JavaScript is executed in both the environment, node.js, and browser but for different use cases. 

Javascript files loaded in Nodejs are automatically wrapped in anonymous functions.So in Node what you are really running is:
```
(function(/* There are args here, but they aren't important for this answer */){
  var x = 10;
  var o = { x: 15 };
  function f(){
    console.log(this.x);
  }
  f();
  f.call(o);
})();
```

The environment of both Node.js and Browser are very different due to their different purpose Some key differences are:

The browser executes JavaScript within the Host Environment, on the client side. Browsers provide a Document Object Model(DOM) which represents the structure of web pages.
Node.js provides a runtime environment that allows JavaScript to run on a server, outside the browser. Also, it does not have a DOM like web browsers.

</p>
</details>

---

#### 69. What is the difference between `==` and `===`?

<details><summary><b>Answer</b></summary>
<p>

##### 
The “===” operator compares both content and type, whereas “==” compares only content. The == operator will compare for equality after doing any necessary type conversions. The === operator will not do type conversion, so if two values are not the same type === will simply return false .
 
</p>
</details>

---

#### 70. How do you handle errors in JavaScript?

<details><summary><b>Answer</b></summary>
<p>

##### 
Error handling in JavaScript plays an important role to make the application stable. Structures such as try-catch blocks, throw statement and finally block are used to deal with unexpected situations and ensure that the application runs correctly.

```
try {
    console.log("try block: The code is running...");
} catch (error) {
    console.log("Error Caught: " + error);
} finally {
    console.log("finally block: Executed in all cases.");
}
```
 
</p>
</details>

---

#### 71. What is globalThis ?

<details><summary><b>Answer</b></summary>
<p>

##### 
globalThis aims to consolidate the increasingly fragmented ways of accessing the global object by defining a standard global property. The globalThis proposal is currently at stage 4, which means it’s ready for inclusion in the ES2020 standard. All popular browsers, including Chrome 71+, Firefox 65+, and Safari 12.1+, already support the feature. You can also use it in Node.js 12+.

// browser environment
console.log(globalThis);    // => Window {...}

// node.js environment
console.log(globalThis);    // => Object [global] {...}

// web worker environment
console.log(globalThis);    // => DedicatedWorkerGlobalScope {...}
By using globalThis, your code will work in window and non-window contexts without having to write additional checks or tests. In most environments, globalThis directly refers to the global object of that environment. In browsers, however, a proxy is used internally to take into account iframe and cross-window security. In practice, it doesn’t change the way you write your code, though.

Generally, when you’re not sure in what environment your code will be used, or when you want to make your code executable in different environments, the globalThis property comes in very handy. You’ll have to use a polyfill to implement the feature on older browsers that do not support it, however.

On the other hand, if you’re certain what environment your code is going to be used in, then use one of the existing ways of referencing the environment’s global object and save yourself from the need to include a polyfill for globalThis.

https://blog.logrocket.com/what-is-globalthis-why-use-it/
 
</p>
</details>

---

#### 72. What is debouncing and throttling in JavaScript?

<details><summary><b>Answer</b></summary>
<p>

##### 
Execution Frequency: Debouncing postpones the execution until after a period of inactivity, while throttling limits the execution to a fixed number of times over an interval.
Use Cases: Debouncing is ideal for tasks that don’t need to execute repeatedly in quick succession, such as API calls based on user input. Throttling is suited for controlling the execution rate of functions called in response to events like scrolling or resizing.

Debouncing and throttling are powerful techniques for optimizing JavaScript applications, preventing unnecessary code executions, and improving user experience.

Debouncing Usecase :
Consider a search input field that fetches suggestions from a server as the user types. Without debouncing, every keystroke would send a request, potentially leading to hundreds of requests per minute. Debouncing allows us to delay the function call until the user has stopped typing for a predefined time.
```
function debounce(func, delay) {
  let debounceTimer;
  return function() {
    const context = this;
    const args = arguments;
    clearTimeout(debounceTimer);
    debounceTimer = setTimeout(() => func.apply(context, args), delay);
  };
}
// Usage
const fetchSuggestions = debounce(() => {
  // Fetch suggestions from the server
}, 250);
```

Throttling Usecase :
An example use case is attaching a listener to the scroll event of a webpage. Since the scroll event can fire dozens of times per second, throttling can be used to limit the number of times your callback function executes, improving performance.
```
function throttle(func, limit) {
  let inThrottle;
  return function() {
    const args = arguments;
    const context = this;
    if (!inThrottle) {
      func.apply(context, args);
      inThrottle = true;
      setTimeout(() => (inThrottle = false), limit);
    }
  };
}
// Usage
window.addEventListener('scroll', throttle(() => {
  // Handle the scroll event
}, 1000));
```
 
</p>
</details>

---

#### 73. How would you implement a deep clone of an object without using libraries?

<details><summary><b>Answer</b></summary>
<p>

##### 
Using Spread Operator to Deep Clone
```
let student1 = {
    name: "Manish",
    company: "Gfg"
}

let student2 = { ...student1 };

student1.name = "Rakesh"

console.log("student 1 name is", student1.name); // student 1 name is Rakesh
console.log("student 2 name is ", student2.name); // student 2 name is  Manish
```

Using Object.assign() Method
```
let student1 = {
    name: "Manish",
    company: "Gfg"
}
let student2 = Object.assign({}, student1);

student1.name = "Rakesh"

console.log("student 1 name is", student1.name); // student 1 name is Rakesh
console.log("student 2 name is ", student2.name); // student 2 name is  Manish
```

Using Json.parse() and Json.stringify() Methods
```
let student1 = {
    name: "Manish",
    company: "Gfg"

}
let student2 = JSON.parse(JSON.stringify(student1))

student1.name = "Rakesh"

console.log("student 1 name is", student1.name); // student 1 name is Rakesh
console.log("student 2 name is ", student2.name); // student 2 name is  Manish
```
 
</p>
</details>

---

#### 74. Event Delegation in JavaScript ?

<details><summary><b>Answer</b></summary>
<p>

##### 
Event Delegation is basically a pattern to handle events efficiently. Instead of adding an event listener to each and every similar element, we can add an event listener to a parent element and call an event on a particular target using the .target property of the event object.

Let’s see an example with and without event delegation
```
const customUI = document.createElement('ul');

for (var i = 1; i <= 10; i++) {
    const newElement = document.createElement('li');
    newElement.textContent = "This is line " + i;
    newElement.addEventListener('click', () => {
        console.log('Responding')
    })
    customUI.appendChild(newElement);
}
```
The above code will associate the function with every <li> element that is shown in the below image. We are creating an <ul> element, attaching too many <li> elements, and attaching an event listener with a responding function to each paragraph as we create it.


Without Event Delegation

Implementing the same functionalities with an alternate approach. In this approach, we will associate the same function with all event listeners. We are creating too many responding functions (that all actually do the exact same thing). We could extract this function and just reference the function instead of creating too many functions:
```
const customUI = document.createElement('ul');

function responding() {
    console.log('Responding')
}

for (var i = 1; i <= 10; i++) {
    const newElement = document.createElement('li');
    newElement.textContent = "This is line " + i;
    newElement.addEventListener('click', responding)
    customUI.appendChild(newElement);
}
```
The functionality of the above code is shown below –


Without Event Delegation

In the above approach, we still have too many event listeners pointing to the same function. Now implementing the same functionalities using a single function and single event.
```
const customUI = document.createElement('ul');

function responding() {
    console.log('Responding')
}

for (var i = 1; i <= 10; i++) {
    const newElement = document.createElement('li');
    newElement.textContent = "This is line " + i;
    customUI.appendChild(newElement);
}
customUI.addEventListener('click', responding)
```

Now there is a single event listener and a single responding function. In the above-shown method, we have improved the performance, but we have lost access to individual <li> elements so to resolve this issue, we will use a technique called event delegation. 
 
</p>
</details>

https://www.geeksforgeeks.org/event-delegation-in-javascript/
---

#### 75. What are the microtask and macrotask within an event loop in JavaScript ?

<details><summary><b>Answer</b></summary>
<p>

##### 
```

```
 
</p>
</details>

---

#### 76. Implement a chain calculator

<details><summary><b>Answer</b></summary>
<p>

##### 
```
const calculator = {
  total: 0,
  add: function(val){
    this.total += val;
    return this;
  },
  subtract: function(val){
    this.total -= val;
    return this;
  },
  divide: function(val){
    this.total /= val;
    return this;
  },
  multiply: function(val){
    this.total *= val;
    return this;
  }
};

calculator.add(10).subtract(2).divide(2).multiply(5);
console.log(calculator.total); // 20
```
 
</p>
</details>

---

#### 77. Execute promises in sequence ?

<details><summary><b>Answer</b></summary>
<p>

##### 
In JavaScript, executing multiple promises sequentially refers to running asynchronous tasks in a specific order, ensuring each task is completed before the next begins. This is essential when tasks depend on the results of previous ones, ensuring correct flow and preventing unexpected behavior.
```
let promise1 = new Promise((resolve, reject) => {
    // Resolves immediately with "Hello! "
    resolve("Hello! ");
});

let promise2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        // Resolves after 1 second with "GeeksforGeeks"
        resolve("GeeksforGeeks");
    }, 1000);
});

// Chain the promises to execute sequentially
promise1.then((result1) => {
    // Logs result from promise1
    console.log(result1);
    // Returns promise2 to chain
    return promise2;
}).then((result2) => {
    // Logs result from promise2
    console.log(result2);
});
```

The for…of loop with async-await in JavaScript allows you to execute promises sequentially. It iterates over an array of promises, awaiting each promise’s resolution before moving to the next, ensuring proper sequential execution of asynchronous tasks.
```
// Promise 1 that resolves immediately
let promise1 = new Promise((resolve, reject) => {
    resolve("Hello! ");
});

// Promise 2 that resolves after a 1-second delay
let promise2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve("GeeksforGeeks");
    }, 1000);
});

let promiseExecution = async () => {
    // Iterates over an array of promises and awaits each one sequentially
    for (let promise of [promise1, promise2]) {
        try {
            // Waits for each promise to resolve
            const message = await promise;
            console.log(message); 
             // Logs the resolved value
        } catch (error) {
        // Catches and logs any errors
            console.log(error.message);  
        }
    }
};
promiseExecution();
```
 
</p>
</details>

---

#### 78. Implement pipe and compose functions ?

<details><summary><b>Answer</b></summary>
<p>

##### 
The compose() function is used to combine multiple functions into a single function. It takes a series of functions as arguments and returns a new function that, when executed, runs the original functions from right to left. This means the output of the rightmost function is passed as the input to the next function, and so on.
```
const compose = (...fns) => (input) => fns.reduceRight((chain, func) => func(chain), input);
const sum = (...args) => args.flat(1).reduce((x, y) => x + y);
const square = (val) => val*val; 
compose(square, sum)([3, 5]); // 64
```
Uses of Compose()
------------------------
When the sequence of transformations starts with the final result in mind.
Common in middleware composition (e.g., Redux).
Useful in functional programming to build complex operations from simple functions.

The pipe() function is similar to compose(), but it combines functions from left to right. This means the output of the leftmost function is passed as the input to the next function, and so on. It’s essentially the same as compose(), but the order of function execution is reversed.
```
const pipe = (...fns) => (input) => fns.reduce((chain, func) => func(chain), input);
const sum = (...args) => args.flat(1).reduce((x, y) => x + y);
const square = (val) => val*val; 
pipe(sum, square)([3, 5]); // 64
```
Uses of pipe()
------------------------
When the sequence of transformations starts from the initial input and flows to the final result.
Common in data processing pipelines.
Preferred in scenarios where readability and straightforward function chaining are important.

To make it short, composition and piping are almost the same, the only difference being the execution order; If the functions are executed from left to right, it's a pipe, on the other hand, if the functions are executed from right to left it's called compose.
 
</p>
</details>

---

#### 79. Create custom array polyfills

<details><summary><b>Answer</b></summary>
<p>

##### 
A polyfill is code that implements a feature on web browsers that do not support the feature.

Polyfill of forEach() method.
----------------------------------------------------------------------
Array.prototype.myForEach = function(callback){
    for(let i=0; i<this.length; i++){
      callback(this[i],i,this)
    }
}

Polyfill of map() method.
----------------------------------------------------------------------
Array.prototype.myMap = function(callback){
    const arr = [];
    for(let i=0; i<this.length; i++){
       arr.push(callback(this[i],i,this));
     }
     return arr;
}

Polyfill of filter() method.
----------------------------------------------------------------------
Array.prototype.myFilter = function(callback){
    const arr = [];
    for(let i=0; i<this.length; i++){
       if(callback(this[i],i,this)){
            arr.push(this[i]);
       }
    }
    return arr;
}

Polyfill of find() method.
----------------------------------------------------------------------
Array.prototype.myFind = function(callback){
    for(let i=0; i<this.length; i++){
      const res = callback(this[i],i,this);
        if(res){
          return this[i];
        }
     }
     return undefined;
}

Polyfill of reduce() method
----------------------------------------------------------------------
Array.prototype.myReduce=function(){
   const callback = arguments[0];
   let currentVal = arguments[1];
   for(let i=0; i<this.length; i++){
     let result = callback(currentVal, this[i], i ,this); 
        currentVal = result;
   }
   return currentVal;
}
var logicAlbums = [ ‘Bobby Tarantino’, ‘The Incredible True Story’, ‘Supermarket’, ‘Under Pressure’, ]
var withReduce = logicAlbums.myReduc(function(a, b) { return a + ‘ , ‘ + b}, ‘Young Sinatra’)

Polyfill of every() method
----------------------------------------------------------------------
Array.prototype.myEvery = function(callback){
   for(let i=0; i<this.length; i++){
     if(!callback(this[i],i,this)){
       return false;
     }
   }
   return true;
}

Polyfill of some() method
----------------------------------------------------------------------
Array.prototype.mySome = function(callback){
   for(let i=0; i<this.length; i++){
     if(callback(this[i],i,this)){
       return true;
     }
   }
   return false;
}
 
</p>
</details>

---

#### 80. Flatten a nested array

<details><summary><b>Answer</b></summary>
<p>

##### 
```
const arr1 = [0, 1, 2, [3, 4]];

console.log(arr1.flat());
// expected output: Array [0, 1, 2, 3, 4]

const arr2 = [0, 1, [2, [3, [4, 5]]]];

console.log(arr2.flat());
// expected output: Array [0, 1, 2, Array [3, Array [4, 5]]]

console.log(arr2.flat(2));
// expected output: Array [0, 1, 2, 3, Array [4, 5]]

console.log(arr2.flat(Infinity));
// expected output: Array [0, 1, 2, 3, 4, 5]
```
 
</p>
</details>

---

#### 81. Build an event emitter

<details><summary><b>Answer</b></summary>
<p>

##### 
```

```
 
</p>
</details>

---

#### 82. Create a debouncing function with leading and trailing calls

<details><summary><b>Answer</b></summary>
<p>

##### 
Debouncing is a technique that is used in programming to reduce the function invocation.

It runs with the assumption that rather than invoking the functions on every action, allows the user to complete their thought process and invoke the function only after a certain buffer between two actions.

For example, if the user is typing something in the search box, rather than making a network request on every word that the user types, we should allow the user to finish typing and only when he stops for X amount of time, the function to make a network request should be invoked.

It is one of the classic frontend interview questions, but because the classic debounce has been common, nowadays its variation is asked during interviews like debounce with immediate flag.

This is another variation of debounce in which we have to use the trailing and leading options.

If trailing is enabled, the debounce will invoke after the delay just like classic implementation. If leading is enabled, it will invoke at the beginning. If both are enabled then it will invoke twice at the beginning and after the delay.
```
const debounce = (fn, delay, option = { leading: true, trailing: true}) => {
  let timeout;
  let isLeadingInvoked = false;
  
  return function (...args) {
    const context = this;
    
    //base condition
    if(timeout){
      clearTimeout(timeout);
    }
    
    // handle leading
    if(option.leading && !timeout){
      fn.apply(context, args);
      isLeadingInvoked = true;
    }else{
      isLeadingInvoked = false;
    }
    
    // handle trailing
    timeout = setTimeout(() => {
      if(option.trailing && !isLeadingInvoked){
        fn.apply(context, args);
      }
      
      timeout = null;
    }, delay);
  }
}
```

```
const onChange = (e) => {
  console.log(e.target.value);
}
const debouncedSearch = debounce(onChange, 1000);
const input = document.getElementById("search");
input.addEventListener('keyup', debouncedSearch);
```
 
</p>
</details>

---

#### 83. Implement MapLimit functionality

<details><summary><b>Answer</b></summary>
<p>

##### 
https://learnersbucket.com/examples/interview/implement-maplimit-async-function/#:~:text=Implement%20a%20mapLimit%20function%20that,can%20occur%20at%20a%20time.
 
</p>
</details>

---

#### 84. Create a cancelable promise

<details><summary><b>Answer</b></summary>
<p>

##### 
Promises once fired, cannot be cancelled. So cancelling the promises in our current context means to ignore the result of the promise.
Example, an autocomplete text changes before results of previous state are received. If not cancelled, the result of previous request may appear after new results, overriding correct results.

Whats the simple solution?
-----------------------------------------
Two promise, 1st that resolves —2nd that resolves 1st.

The Solution
-----------------------------------------
```
function cancellablePromise(executor) {
    let isCancelled = false;
    const promise = new Promise((resolve, reject) => {
        new Promise(executor)
            .then(value => !isCancelled && resolve(value))
            .catch(error => !isCancelled && reject(error));
    });
    return {
        then: promise.then.bind(promise),
        catch: promise.then.bind(promise),
        finally: promise.then.bind(promise),
        cancel: () => isCancelled = true,
    }
}


// Test
const normalPromise = cancellablePromise(resolve => {
    setTimeout(() => resolve('Normal Promise Complete'), 500);
});

const cancelledPromise = cancellablePromise(resolve => {
    setTimeout(() => resolve('Canclled Promise Complete'), 500);
});

normalPromise.then(console.log);
// After a sec: Promise Resolved

cancelledPromise.cancel();
cancelledPromise.then(console.log)
// Will never resolve
```
https://medium.com/@daxgama/a-simple-cancellable-promise-using-two-promises-5d0af1f84e72
Use case 
----------------------------------------
1. Example, an autocomplete text changes before results of previous state are received. If not cancelled, the result of previous request may appear after new results, overriding correct results.
2. Promises once fired, cannot be cancelled. So cancelling the promises in our current context means to ignore the result of the promise. When an api call is fired inside a react component, any state update after the component is destroyed (inside the then block of the promise), will cause error.
 
</p>
</details>

---

#### 85. Implement currying

<details><summary><b>Answer</b></summary>
<p>

##### 
Currying is a functional programming technique where a function with multiple arguments is transformed into a series of functions, each taking a single argument.
In other words, instead of a function taking all arguments at one time, it takes the first one and returns a new function, which takes the second one and returns a new function, which takes the third one, and so on, until all arguments have been fulfilled.

Normal Function
-----------------------------------
function sum(a, b, c) {
    return a + b + c;
}
sum(1,2,3); // 6

Function Currying
-----------------------------------
```
function sum(a) {
    return function(b) {
        return function(c) {
            return a + b + c;
        }
    }
}
sum(1,2,3); // 6

function sum(a) {
    return (b) => {
        return (c) => {
            return a + b + c;
        }
    }
}
sum(1,2,3); // 6
```
Why is Currying useful in JavaScript?
----------------------------------------------------
Currying offers several advantages, especially when working with functional programming patterns:

It helps us to create a higher-order function
It reduces the chances of error in our function by dividing it into multiple smaller functions that can handle one responsibility.
It is very useful in building modular and reusable code
It helps us to avoid passing the same variable multiple times
It makes the code more readable

This example explains the currying technique with the help of closures. During the thread of execution, the sum() function will be invoked. Inside there is an anonymous function, that receives a parameter and returns some code. We are exposing our function to another function, so closure will be created. Closure always contains the function definition along with the lexical environment of the parent, both things remain connected as a bundle. Hence, it does not matter where we invoke them, the all inner functions will always hold access to the variable of their parent.

</p>
</details>

---

#### 86. Execute tasks in parallel ?

<details><summary><b>Answer</b></summary>
<p>

##### 
When you have multiple time-consuming tasks/functions to execute, there are two main solutions to optimize the execution time and speed up your app:

Run everything at once with Promise.all()
----------------------------------------------------------
If your functions are promise-based, they can easily be executed concurrently using Promise.all()
```
const axios = require('axios').default
const fetchPosts = () => axios.get('/api/to/posts')
const fetchUsers = () => axios.get('/api/to/users')
const fetchDocs = () => axios.get('/api/to/docs')
Promise.all([
  fetchPosts(),
  fetchUsers(),
  fetchDocs(),
]).then(([postsRes, usersRes, docsRes]) => {
  // do something
}).catch((err) => console.log(err))
```

Run a fixed batch concurrently
---------------------------------------------
If your functions require significant resources to execute, running them all at once with Promise.all() may cause your application to crash. A solution to this is to create a TaskQueue that can execute a fixed number of tasks concurrently.
```
class ConcurrentTaskQueue {
  constructor(taskPromisesFunc = [], batchSize = 1) {
    this.batchSize = batchSize > taskPromisesFunc.length ? taskPromisesFunc.length : batchSize
    this.todoTasks = taskPromisesFunc
    this.resolvedValues = []
  }

  run(resolve, reject) {
    if (this.todoTasks.length > 0) {
      const taskPromises = this.todoTasks.splice(0, this.batchSize);
      Promise.all(taskPromises.map((p) => p()))
        .then((resolvedValues) => {
          this.resolvedValues = [...this.resolvedValues, ...resolvedValues]
          this.run(resolve, reject)
        })
        .catch((err) => reject(err))
    } else {
      resolve(this.resolvedValues)
    }
  }

  runTasks() {
    return new Promise((resolve, reject) => {
      this.run(resolve, reject)
    })
  }
}

// some arbitrary function that consumes resources
const costlyFunction = (arg) => new Promise((resolve) => {
  // do something costly here
  resolve(arg);
})

const batchSize = 2;
const taskQueue = new ConcurrentTaskQueue([
  // wrap all functions to prevent direct execution
  () => costlyFunction(10),
  () => costlyFunction(20),
  () => costlyFunction(100),
  () => costlyFunction(50),
], batchSize);
taskQueue.runTasks()
  .then(([res1, res2, res3, res4]) => {
    console.log(res1, res2, res3, res4);
  });
```
 
</p>
</details>

---

#### 87. Find the matching element in the DOM ?

<details><summary><b>Answer</b></summary>
<p>

##### 
The easiest way to access a single element in the DOM is by its unique ID. You can get an element by ID with the getElementById() method of the document object.

JavaScript offers several methods to find and manipulate elements in the DOM. Here are some of the most common methods:
-----------------------------------------------------------
getElementById()
getElementsByName()
getElementsByClass()
getElementsByTagName()
querySelector()
querySelectorAll()
 
</p>
</details>

---

#### 88. What will the output ?
1. Write code to get array of names from given array of users
2. Get back only active users
3. Sort users by age descending

const users = [
  {
    id: 1,
    name: "Jack",
    isActive: true,
    age: 20,
  },
  {
    id: 2,
    name: "John",
    isActive: true,
    age: 18,
  },
  {
    id: 3,
    name: "Mike",
    isActive: false,
    age: 30,
  },
];

<details><summary><b>Answer</b></summary>
<p>

##### 
```
const names = users.sort((user1, user2) => (user1.age < user2.age ? 1 : -1)).filer((user) => user.isActive).map((user) => user.name);
```
 
</p>
</details>

---

#### 89. Write a function which helps to achieve multiply(a)(b) and returns product of a and b

<details><summary><b>Answer</b></summary>
<p>

##### 
```
const sum = (a) => {
	return (b) => {
      	return a + b;
    }
}

sum(1)(2); // 3
```
 
</p>
</details>

---

#### 90. Create a curry function manually

<details><summary><b>Answer</b></summary>
<p>

##### 
```
const curry = function(fn) {
	const len = fn.length;
    console.log(len);
    return function f1(...args) {
    	if (args.length >= len) {
        	console.log('enough arguments', args);
        	return fn(...args); // // Run the function when we have enough arguments
        } else {
        	console.log('not enough arguments');
            return function f2(...moreArgs) {
            	console.log(moreArgs);
            	const newArr = args.concat(moreArgs);
                return f1(...newArr);
            }
        }
    }
}
let sum = curry((a, b, c) => (a * b * c));
sum(2)(3, 4); // 24

-----------------------------------------------------------------------

const curry = (fn) => {
  const arity = fn.length;
  // Accept more than one argument at a time!
  const accumulator = (previousArgs) => (...args) => {
    const newArgs = [...previousArgs, ...args];
    console.log('previousArgs => ', previousArgs);
    console.log('args => ', args);
    if (newArgs.length < arity) return accumulator(newArgs);
    // Run the function when we have enough arguments
    return fn(...newArgs);
  };
  // Start with no arguments passed.
  return accumulator([]);
};


let sum = curry((a, b, c) => (a * b * c));
sum(2)(3, 4); // 24
```
 
</p>
</details>

---

#### 91. Merge two arrays

<details><summary><b>Answer</b></summary>
<p>

##### 
```
const array1 = [1, 2, 3];
const array2 = [4, 5, 6];
const finalArr = array1.concat(array2); // [1, 2, 3, 4, 5, 6]

const array1 = [1, 2, 3];
const array2 = [4, 5, 6];
const finalArr = [...array1, ...array2]; // [1, 2, 3, 4, 5, 6]
```
 
</p>
</details>

---

#### 92. Writet he code to Check that user with such name exists in array of objects
const users = [
  {
    id: 1,
    name: "Jack",
    isActive: true,
  },
  {
    id: 2,
    name: "John",
    isActive: true,
  },
  {
    id: 3,
    name: "Mike",
    isActive: false,
  },
];
<details><summary><b>Answer</b></summary>
<p>

##### 
```
const isExists = (name, users) => users.some(user => user.name === name);
isExists("John", users); // true
isExists("Test", users); // false
```
 
</p>
</details>

---

#### 93. Sort the array of numbers

<details><summary><b>Answer</b></summary>
<p>

##### 
```
const arr = [3, 5, 1];
const result = arr.sort((a, b) => a - b); // [1, 3, 5]
```
 
</p>
</details>

---

#### 94. Sort array of objects by author's lastname
const books = [
  { name: "Harry Potter", author: "Joanne Rowling" },
  { name: "Warcross", author: "Marie Lu" },
  { name: "The Hunger Games", author: "Suzanne Collins" },
];
<details><summary><b>Answer</b></summary>
<p>

##### 
```
const finalArr = books.sort((book1, book2) => book1.author.split(" ")[1] < book2.author.split(" ")[1] ? -1 : 1);
console.log(finalArr);
-------------------
[
    {
        "name": "The Hunger Games",
        "author": "Suzanne Collins"
    },
    {
        "name": "Warcross",
        "author": "Marie Lu"
    },
    {
        "name": "Harry Potter",
        "author": "Joanne Rowling"
    }
]

```
 
</p>
</details>

---

#### 95. How to create an array containing 1...N

<details><summary><b>Answer</b></summary>
<p>

##### 
```
Array.from(Array(10).keys())
//=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
Shorter version using spread operator.

[...Array(10).keys()]
//=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
Start from 1 by passing map function to Array from(), with an object with a length property:

Array.from({length: 10}, (_, i) => i + 1)
//=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
 
</p>
</details>

---

#### 96. Print a range of an array like 10...20

<details><summary><b>Answer</b></summary>
<p>

##### 
```
const range = (start, end) => {
    return [...Array(end - start + 1).keys()].map(elm => elm + start);
}
range(10, 20); //  [10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20]
```
 
</p>
</details>

---

#### 97. Find the number of occurences of minimum value in the list

<details><summary><b>Answer</b></summary>
<p>

##### 
```
const arr = [1, 2, 2, 3, 4];
const minNumOccurance = arr.filter(elm => elm === Math.min(...arr)).length; // 2
```
 
</p>
</details>

---

#### 98. What will be the output of bellow codes

<details><summary><b>Answer</b></summary>
<p>

##### 
```
const print = {
    name: 'test',
    display: function() {
        console.log(this);
    }
}
print.display(); // {name: 'test', display: ƒ}
-------------------------------------------------------------------------------------------------------
function display() {
    console.log(this);
}
display(); // Window {window: Window, self: Window, document: document, name: '', location: Location, …}
-------------------------------------------------------------------------------------------------------
class Item {
  name ='Test';
  display() {
  	console.log('this =>', this);
  }
}
const item = new Item();
item.display(); // this => Item {name: 'Test'}
-------------------------------------------------------------------------------------------------------
class Item {
  name ='Test';
  display() {
    function someFunc() {
  	console.log('this =>', this);
    }
    someFunc();
  }
}
const item = new Item();
item.display(); // this => undefined
-------------------------------------------------------------------------------------------------------
class Item {
  name ='Test';
  display() {
  	[1, 2, 3].map((item) => {
  		console.log('this =>', this);
    })
  }
}
const item = new Item();
item.display(); // this => Item {name: 'Test'}
-------------------------------------------------------------------------------------------------------
```
 
</p>
</details>

---

#### 99. What is Factory Functions ?

<details><summary><b>Answer</b></summary>
<p>

##### 
a factory function is simply a function that creates objects and returns them.  In JavaScript, any function can return an object. When it does so without the new keyword, it’s a factory function. Factory functions have always been attractive in JavaScript because they offer the ability to easily produce object instances without diving into the complexities of classes and the new keyword.
The Factory Function Pattern uses closures to encapsulate and return object instances with methods and properties, allowing for the creation of similar objects without explicitly class-based syntax.
```
function friendFactory(friendName) {
   return {
       friendName: friendName,
       talk() {
           console.log(`Hello I am ${friendName}.`)
       }
   }
};

let friendOne = friendFactory('Billy');
let friendTwo = friendFactory('Mary');

friendOne.talk()
friendTwo.talk()

//Output
Hello I am Billy
Hello I am Mary
```
https://heyjoshlee.medium.com/factory-functions-in-javascript-the-how-and-why-d8988bda654a
 
</p>
</details>

---

#### 100. Practical examples of Closure

<details><summary><b>Answer</b></summary>
<p>

##### 
1. Data Privacy - Closures enable the creation of private variables and functions within a function’s scope. Developers can create modules and libraries with hidden implementation details by encapsulating data within closures. This prevents external code from accessing or modifying internal state directly and promotes information hiding thus enhancing data protection.
```
function createCounter() {
    let count = 0; // This variable is private to the createCounter function

    return {
        increment: function() {
            count++;
        },
        decrement: function() {
            count--;
        },
        getCount: function() {
            return count;
        }
    };
}

const counter = createCounter();

console.log(counter.getCount()); // Output: 0
counter.increment();
counter.increment();
console.log(counter.getCount()); // Output: 2
counter.decrement();
console.log(counter.getCount()); // Output: 1
```
-------------------------------------------------------------------------------------------------------
2. Factory functions
```
const ArrayAnalyzer = (function() {
  function average(array) {
    const sum = array.reduce((acc, num) => acc + num, 0);
    return sum / array.length;
  }

  function maximum(array) {
    return Math.max(...array);
  }

  return {
    average: average,
    maximum: maximum
  };
}());
ArrayAnalyzer.average([1, 2, 3, 4]); // 2.5
ArrayAnalyzer.maximum([1, 2, 3, 4]); // 4
```
-------------------------------------------------------------------------------------------------------
3. Memoization functions
```
// Define memoize Function
function memoize(fn) {
    const cache = {}; // Private cache object
    return function(...args) {
        const key = JSON.stringify(args); // Create a key from arguments
        if (cache[key]) {
            return cache[key]; // Return cached result
        }
        const result = fn(...args);
        cache[key] = result; // Store result in cache
        return result;
    };
}

// Example usage:
const fibonacci = memoize(function(n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
});

console.log(fibonacci(10)); // 55
```
-------------------------------------------------------------------------------------------------------
4. Event Handling - Closures are commonly used in event handling to encapsulate data and behavior associated with an event listener. Closures are powerful tools for managing event handling in JavaScript. They allow you to attach functions to events while maintaining access to the surrounding scope, which is particularly useful for data encapsulation and avoiding global variables.
```
<button id="myButton">Click</button>

<script>
function handleCounter() {
    let count = 0;
    const button = document.getElementById('myButton');
    
    button.addEventListener('click', function() {
        count++;
       alert('Button clicked ' + count + ' times');
    });
}
handleCounter();
</script>
```
In this example, the event handler function (defined inside addEventListener) has access to the count variable from its outer lexical scope (handleCounter function). Each time the button is clicked the count variable is incremented and logged to the console even after the handleCounter function has finished executing.

-------------------------------------------------------------------------------------------------------
5. Currying - Currying is a technique where a function with multiple arguments is transformed into a sequence of functions, each taking a single argument. Closures are often used to implement currying in JavaScript.
```
function curry(fn) {
    console.log('fn => ', fn);
    	return function curried(...args) {
        	console.log('args => ', args);
        	if (args.length >= fn.length) {
            	console.log("IF");
            	return fn(...args);
            } else {
            	console.log("ELSE");
            	return function(...moreArgs) {
                	console.log('moreArgs => ', moreArgs);
                	return curried(...args, ...moreArgs);
                }
            }
        }
    }
    const add = curry((a, b, c) => a + b + c);
    console.log(add(1)(2)(3));

The output :
fn =>  (a, b, c) => a + b + c
args =>  [1]
moreArgs =>  [2]
args =>  (2) [1, 2]
moreArgs =>  [3]
args =>  (3) [1, 2, 3]
6
```

</p>
</details>

---

#### 101. Performance optiimization : how you will achieve ?

<details><summary><b>Answer</b></summary>
<p>

##### 
1. Avoid Unnecessary Variable declaration: Ensure that do not unnecessarily delcare variables that are not used or needed.
2. Utilise Weak References : Consider using WeakMap or WeakSet to store references to objects, as these data structures do not prevent their objects from being garbage collected.
3. Memoization is an optimization technique that can significantly speed up function execution by caching their results.
4. Null, Undefined, Empty Checks
5. Shorthand code wiriting
 
</p>
</details>

---

#### 102. What is Memoization ?

<details><summary><b>Answer</b></summary>
<p>

##### 
Memoization is the process of storing the results of function execution so that the next time the function is called with the same arguments, the stored result can be returned immediately. Memoization prevents redundant calculations and improves performance, especially in cases where a function is called repeatedly with the same arguments.
When dealing with functions that involve complex and resource-intensive calculations (e.g., recursive functions for calculating Fibonacci numbers or factorials), optimization can be essential.
In practice, memoization can be beneficial when developing complex applications involving heavy calculations, such as in data analysis, machine learning, etc.

<strong>Implementing a Memoized Function using normal object</strong>
------------------------------------------------------------------------------------------------
To implement memoization, we’ll create a memoize function wrapper that caches the results of a target function:
```
function square(num) {
    console.log('Calculating square of', num);
    return num * num;
}
function memoize(fn) {
    const cache = {}; // Object to store cached results

    return function(...args) {
        const key = JSON.stringify(args); // Create a key based on arguments

        if (cache[key]) {
            return cache[key]; // Return the cached result if available
        }

        const result = fn(...args); // Call the original function
        cache[key] = result; // Store the result in cache
        return result;
    };
}

const memoizedSquare = memoize(square);

console.log(memoizedSquare(4)); // Calculating square of 4 -> 16
console.log(memoizedSquare(4)); // 16 (no recalculation)
```
Now, the memoizedSquare function caches results for each unique set of arguments and retrieves them from the cache on subsequent calls.

<strong>Implementing a Memoized Function using Map</strong>
------------------------------------------------------------------------------------------------
```
function fibonacci(n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}
function memoize(fn) {
    const cache = new Map();

    return function(...args) {
        const key = JSON.stringify(args);
        if (cache.has(key)) {
            return cache.get(key);
        }
        const result = fn(...args);
        cache.set(key, result);
        return result;
    };
}
const memoizedFibbo = memoize(fibonacci);
memoizedFibbo(9); // 34
```
https://dev.to/hmpljs/memoization-in-javascript-optimizing-computations-and-improving-performance-5129

</p>
</details>

---

#### 103. What is a callback function in JavaScript ?

<details><summary><b>Answer</b></summary>
<p>

##### 
 A callback function is a function passed as an argument to another function, which is invoked within the latter function, usually for asynchronous tasks or to customize behavior.
 
Callback allows us to make some asynchronous task & wait for the result. actually here we're just providing the function from outside and it will be called later and not immediately. This is the main purpose of the callback.

And secondly, it's important to remember that inside our asynchronous function of we don't know what this callback. This is why we can build shareable things. This callback can do whatever in different cases.

For example, on the one page, we want to fetch data and maybe render this data, and on another page, we want to fetch this data and calculate the total number of posts or something like this, which actually means callback is a generic function.

This is why we can easily share our asynchronous function without specific implementation of our callback.
```
const asyncFn = (callback) => {
  console.log('callback => ', callback);
  setTimeout(() => {
    callback('msg');
  }, 1000);
};

export function start(element) {
  asyncFn((message) => {
    element.innerHTML = `callback ${message}`;
  });
}
```
 
</p>
</details>

---

#### 104. How do callbacks work in JavaScript ?

<details><summary><b>Answer</b></summary>
<p>

##### 
A callback is a function passed as an argument to another function, and a callback function can run after another function has finished. JavaScript functions execute in the sequence they get called, not in the defined sequence.
 
</p>
</details>

---

#### 105. Write callback function to execute async functions in parallel?
Implement a function in JavaScript that takes a list of async functions as input and a callback function and executes the async tasks in parallel that is all at once and invokes the callback after every task is executed.
<details><summary><b>Answer</b></summary>
<p>

##### 
const asyncFn1 = (callback) => {
  setTimeout(() => {
    callback(1);
  }, 1000);
};

const asyncFn2 = (callback) => {
  setTimeout(() => {
    callback(2);
  }, 2000);
};

const asyncFn3 = (callback) => {
  setTimeout(() => {
    callback(3);
  }, 3000);
};

const asyncParallel = (asyncFuncs, callback) => {
  const arr = [];
  asyncFuncs.forEach((asyncFn, index) => {
    asyncFn((value) => {
      arr.push(value);
      console.log(index + ': arr ', arr);
      if (asyncFuncs.length === index + 1) {
        callback(arr.join(','));
      }
    });
  });
};

export function start(element) {
  asyncParallel([asyncFn1, asyncFn2, asyncFn3], (result) => {
    element.innerHTML = `callback ${result}`;
  });
} 
</p>
</details>

---

#### 106. What are some common use cases for callbacks?

<details><summary><b>Answer</b></summary>
<p>

##### 
Common use cases for callbacks include handling asynchronous operations (like reading files or making HTTP requests), handling events (like click or key press events), and for higher-order functions (like Array's map, filter, reduce).

When you need to take action after an asynchronous process has finished, callbacks can be helpful. As an illustration, you could utilize a callback function to:

1. Update the UI after a network request has completed.
2. Process data after a file has been read.
3. Make another API call after the results of the first API call have been received.

Fetching data from a server
-----------------------------------------------------------------------------------------------------
In this case, we use a callback function to handle the data once it has been received.
```
function fetchData(url, callback) {
  fetch(url)
    .then(response => response.json())
    .then(data => callback(data))
    .catch(error => console.log(error));
}

fetchData('https://jsonplaceholder.typicode.com/todos/1', function(data) {
  console.log(data);
});
```
In this example, we define a function fetchData that takes a URL and a callback function as arguments. The function uses the fetch function to make a network request to the specified URL. Once the data is received, it is converted to JSON and the callback function is called with the data as an argument.

Finally, we call the fetchData function and pass in a callback function that logs the data to the console. When the data is received from the server, the callback function is called with the data, and the data is logged to the console.

addEventListener()
----------------------------------------------------------------------------
This function takes two arguments: an event name and a callback function. The callback function is executed when the event occurs.

In this example, querySelector('button') selects the first “<button>” element in the document. The subsequent addEventListener function adds an event listener for the "click" event on this button.

When the button is clicked, the provided callback function will be executed, logging "Button clicked!" to the console along with the event object.
```
<ul class="list">
    <li class="item">One</li>
    <li class="item">Two</li>
    <li class="item">Three</li>
    <li class="item">Four</li>
</ul>
const list = document.querySelector('.list');
list.addEventListener('click', (e) => {
    if (e.target && e.target.classList.contains('item')) {
      console.log(`${e.target.innerText} is clicked`);
    }
});
```

Promise.then()
--------------------------------------------------------------
This method takes a callback function as an argument and executes the callback function when the promise is resolved.

In this example, we create a Promise (myPromise) that simulates an asynchronous operation using setTimeout.  Inside the Promise, after a one-second delay, we resolve the Promise with the message 'The promise was resolved!'. We then chain a then() method onto myPromise. The first argument of then() is a function that will be executed if the Promise is resolved.
```
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      // Other things to do before completion of the promise
      console.log("Did something");
      // The fulfillment value of the promise
      resolve("https://example.com/");
    }, 1000);
});
promise.then((result) => {
    console.log(result);
})
```

</p>
</details>

---

#### 107. What is callback hell, and how can it be avoided?

<details><summary><b>Answer</b></summary>
<p>

##### 
Callback hell refers to deeply nested, difficult-to-read callback functions. It can be avoided by modularizing code, using Promises, or async/await syntax for better code readability and maintainability.
Callback hell, often referred to as “Pyramid of Doom,” occurs in Node.js when multiple nested callbacks lead to code that is hard to read, maintain, and debug. This situation arises when each asynchronous operation depends on the completion of the previous one, resulting in deeply nested callback functions. Fortunately, modern JavaScript and Node.js provide several techniques to mitigate this problem and write cleaner, more maintainable code.

```
// The callback function for function
// is executed after two seconds with
// the result of addition
const add = function (a, b, callback) {
  setTimeout(() => {
    callback(a + b);
  }, 2000);
};

// Using nested callbacks to calculate
// the sum of first four natural numbers.
add(1, 2, (sum1) => {
  add(3, sum1, (sum2) => {
    add(4, sum2, (sum3) => {
      console.log(`Sum of first 4 natural 
      numbers using callback is ${sum3}`);
    });
  });
});

// This function returns a promise
// that will later be consumed to get
// the result of addition
const addPromise = function (a, b) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(a + b);
    }, 2000);
  });
};

// Consuming promises with the chaining of then()
// method and calculating the result
addPromise(1, 2)
  .then((sum1) => {
    return addPromise(3, sum1);
  })
  .then((sum2) => {
    return addPromise(4, sum2);
  })
  .then((sum3) => {
    console.log(
      `Sum of first 4 natural numbers using 
       promise and then() is ${sum3}`
    );
  });

// Calculation the result of addition by
// consuming the promise using async/await
(async () => {
  const sum1 = await addPromise(1, 2);
  const sum2 = await addPromise(3, sum1);
  const sum3 = await addPromise(4, sum2);

  console.log(
    `Sum of first 4 natural numbers using 
     promise and async/await is ${sum3}`
  );
})();
```

setTimeout()
-----------------------------------------------------
setTimeout() takes a callback function as an argument and executes the callback function after a specified amount of time.
It takes two arguments: the first is the callback function to be executed, and the second is the time delay in milliseconds before the callback is invoked.
In the given example, if you have a setTimeout() set to 1000 milliseconds (or 1 second), it means that the provided callback function will be executed after 1 second.
```
setTimeout(() => {
  console.log('logged after 1 sec');
}, 1000);
```

</p>
</details>

---

#### 108. Write a function to convert a string to title case
1. titleCase(“I’m a little tea pot”) should return a string.
2. titleCase(“I’m a little tea pot”) should return “I’m A Little Tea Pot”.
3. titleCase(“sHoRt AnD sToUt”) should return “Short And Stout”.
4. titleCase(“HERE IS MY HANDLE HERE IS MY SPOUT”) should return “Here Is My Handle Here Is My Spout”.
<details><summary><b>Answer</b></summary>
<p>

##### 
```
const titleCase = (str) => {
  if (typeof str !== 'string') {
    return false;
  }
  return str
    .toLowerCase()
    .split(' ')
    .map((word) => word.charAt(0).toUpperCase() + word.slice(1))
    .join(' ');
};

titleCase('I’m a little tea pot')
```
 
</p>
</details>

---

#### 109. Write Shallow vs Deep comparison function for Mutable Data Types

<details><summary><b>Answer</b></summary>
<p>

##### 
As we know, comparison of Mutable data (arrays, objects, and date) is not possible with equality operator. We have to compare arrays and objects by values and not like reference to the object and the memory.

Shallow comparison function
-------------------------------------------------------
```
const shallowCompare = (source, target) => {
  if (typeof source !== typeof target) {
    return false;
  }

  if (typeof source === 'array') {
    if (source.length !== target.length) {
      return false;
    }
    return source.every((el, index) => el === target[index]);
  }

  if (typeof source === 'object') {
    if (Object.keys(source).length !== Object.keys(target).length) {
      return false;
    }
    return Object.keys(source).every((key) => source[key] === target[key]);
  }

  if (typeof source === 'date') {
    return source.getTime() === target.getTime();
  }
  return source === target;
};

console.log(shallowCompare([1], [1])); // true
console.log(shallowCompare({ a: 1 }, { a: 1 })); // true
```

Deep comparison function
-------------------------------------------------------
```
const deepCompare = (source, target) => {
  if (typeof source !== typeof target) {
    return false;
  }

  if (typeof source === 'array') {
    if (source.length !== target.length) {
      return false;
    }
    return source.every((el, index) => deepCompare(el, target[index]));
  }

  if (typeof source === 'object') {
    if (Object.keys(source).length !== Object.keys(target).length) {
      return false;
    }
    return Object.keys(source).every((key) =>
      deepCompare(source[key], target[key])
    );
  }

  if (typeof source === 'date') {
    return source.getTime() === target.getTime();
  }
  return source === target;
};

console.log(deepCompare([[1][2]], [[1][2]])); // true
```
 
</p>
</details>

---

#### 110. Set vs Map vs Object

<details><summary><b>Answer</b></summary>
<p>

| Feature         | Set         | Map       | Object                |
| --------------- | ----------- | --------- | --------------------- |
| Stores          | Values only | Key–Value | Key–Value             |
| Duplicate keys  | ❌           | ❌         | ❌                     |
| Key types       | N/A         | Any       | String / Symbol       |
| Iteration order | Yes         | Yes       | Partial               |
| Performance     | Fast        | Fast      | Slower for large data |

Set: A Set is a collection of unique values (no duplicates).
✅ Key Features : 1. Stores unique values only 2. Can store any type (primitive or object) 3. Maintains insertion order 4. Fast lookups (O(1))
```
const set = new Set();
set.add(1);
set.add(2);
set.add(2); // ignored
set.add('hello');

console.log(set); // Set(3) {1, 2, 'hello'}
set.has(2); // true
set.has(5); // false
set.delete(1);
set.size; // 2
for (const value of set) {
  console.log(value);
}

const arr = [1, 2, 2, 3, 3];
const unique = [...new Set(arr)];
console.log(unique); // [1, 2, 3]
```

Map: A Map stores key–value pairs.
✅ Key Features : 1. Keys can be any type (object, function, primitive) 2. Maintains insertion order 3. Better than plain objects for frequent add/remove operations
```
const map = new Map();
map.set('name', 'Piyali');
map.set('age', 30);

const objKey = { id: 1 };
map.set(objKey, 'Object Key Value');
map.get('name'); // 'Piyali'
map.get(objKey); // 'Object Key Value'
map.has('age'); // true
map.delete('age');
map.size;
for (const [key, value] of map) {
  console.log(key, value);
}
```
**🔹 Common Real-World Use Cases**
```
✅ Remove duplicates (Set)
const users = ['A', 'B', 'A'];
const uniqueUsers = new Set(users);

✅ Cache / Lookup table (Map)
const cache = new Map();

function fetchData(id) {
  if (cache.has(id)) return cache.get(id);

  const data = `Data for ${id}`;
  cache.set(id, data);
  return data;
}
```
**🔹 Interview Tips 🚀**
```
Use Set when uniqueness matters
Use Map when keys are not strings
Prefer Map over Object for dynamic data
Set.has() is faster than Array.includes()
```
 
</p>
</details>

---

#### 110. Write async functions in parallel using Callback

<details><summary><b>Answer</b></summary>
<p>

##### 
Callback allows us to make some asynchronous task & wait for the result. actually here we're just providing the function from outside and it will be called later and not immediately. This is the main purpose of the callback.

And secondly, it's important to remember that inside our asynchronous function of we don't know what this callback. This is why we can build shareable things. This callback can do whatever in different cases.

For example, on the one page, we want to fetch data and maybe render this data, and on another page, we want to fetch this data and calculate the total number of posts or something like this, which actually means callback is a generic function.

This is why we can easily share our asynchronous function without specific implementation of our callback.
```
const asyncFn1 = (callback) => {
  setTimeout(() => {
    callback(1);
  }, 1000);
};

const asyncFn2 = (callback) => {
  setTimeout(() => {
    callback(2);
  }, 2000);
};

const asyncFn3 = (callback) => {
  setTimeout(() => {
    callback(3);
  }, 3000);
};

const asyncParallel = (asyncFuncs, callback) => {
  const arr = [];
  asyncFuncs.forEach((asyncFn, index) => {
    asyncFn((value) => {
      arr.push(value);
      console.log(index + ': arr ', arr);
      if (asyncFuncs.length === index + 1) {
        callback(arr.join(','));
      }
    });
  });
};

asyncParallel([asyncFn1, asyncFn2, asyncFn3], (result) => {
    element.innerHTML = `callback ${result}`;
  });
```
 
</p>
</details>

---

#### 111. Is Fetch API better than Axios?

<details><summary><b>Answer</b></summary>
<p>

##### 
Ref Link : https://medium.com/@johnnyJK/axios-vs-fetch-api-selecting-the-right-tool-for-http-requests-ecb14e39e285#:~:text=Axios%20will%20automatically%20transforms%20the,be%20stored%20in%20any%20variable.

AXIOS
--------------------------------------------------------------------------------------------------
Axios is a third-party HTTP client library for making network requests. It is promise-based and provides a clean and consistent API for handling requests and responses.
Axios simplifies HTTP requests with a chainable API which allows for straightforward configuration of request parameters, such as headers, data, and request method.

Here’s how to use Axios to send a [POST] request with custom headers to a URL. Axios automatically converts the data to JSON, so you don’t have to.
```
const axios = require('axios');

const url = 'https://jsonplaceholder.typicode.com/posts';
const data = {
  title: 'Hello World',
  body: 'This is a test post.',
  userId: 1,
};

axios
  .post(url, data, {
    headers: {
      Accept: 'application/json',
      'Content-Type': 'application/json;charset=UTF-8',
    },
  })
  .then(({ data }) => {
    console.log("POST request successful. Response:", data);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

FETCH
--------------------------------------------------------------------------------------------------
Fetch, like Axios, is a promise-based HTTP client. It is a built-in API; hence we don’t have to install or import anything. It’s available in all modern browsers
```
const url = "https://jsonplaceholder.typicode.com/todos";

const options = {
  method: "POST",
  headers: {
    Accept: "application/json",
    "Content-Type": "application/json;charset=UTF-8",
  },
  body: JSON.stringify({
    title: "Hello World",
    body: "This is a test post.",
    userId: 1,
  }),
};

fetch(url, options)
  .then((response) => response.json())
  .then((data) => {
    console.log("POST request successful. Response:", data);
  });
```

Fetch uses the body property for sending data in a POST request, whereas Axios utilizes the data property. Axios will automatically transforms the server’s response data, while with Fetch, you need to call the response.json method to parse the data into a JavaScript object. Furthermore, Axios delivers the data response within the data object, whereas Fetch allows the final data to be stored in any variable.

Handling Response and Errors
--------------------------------------------------------------------------------------------------
Fetch offers precise control over the loading process but introduces complexity by requiring the handling of two promises. Additionally, when dealing with responses, Fetch requires parsing JSON data using the .json() method. The final data retrieved can then be stored in any variable.

For HTTP response errors, Fetch does not automatically throw an error. Instead, it considers the response as successful even if the server returns an error status code (e.g., 404 Not found).

To handle HTTP errors explicitly in Fetch, developers have to use conditional statements within the .then() block to check the response.ok property. If response.ok is false, it indicates that the server responded with an error status code, and developers can handle the error accordingly.

Here is a [GET] request using fetch:
```
fetch('https://jsonplaceholder.typicode.com/todos')
  .then(response => {
    if (!response.ok) {
      throw Error(`HTTP error: ${response.status}`);
    }
    return response.json();
  })
  .then(data => {
    console.log('Data received:', data);
  })
  .catch(error => {
    console.error('Error message:', error.message);
  });
```

Axios, on the other hand, simplifies response handling by directly providing access to the data property. It automatically rejects responses outside of the 200–299 range (successful responses). By utilizing a .catch() block, information about the error can be obtained, including whether a response was received and, if so, its status code. This contrasts with fetch, where unsuccessful responses are still resolved.

Here is a [GET] request using Axios:
```
const axios = require("axios");

axios
  .get("https://jsonplaceholder.typicode.com/todos")
  .then((response) => {
    console.log("Data received:", response.data);
  })
  .catch((error) => {
    if (error.response) {
      console.error(`HTTP error: ${error.response.status}`);
    } else if (error.request) {
      console.error("Request error: No response received");
    } else {
      console.error("Error:", error.message);
    }
  });
```
By automatically throwing errors for unsuccessful responses, Axios simplifies error handling and allows developers to focus on implementing appropriate error-handling logic without having to manually check each response for success or failure.

</p>
</details>

---

#### 112. Dynamic object create ?
function sum(num1, num2) {  
    num1 = 3, num2 = 7;  
    return arguments[0] + arguments[1];  
}  
console.log(sum(2,2)); // 10  
How you will restrict the changes of num1 & num2 inside function to get sum 4 ?  

<details><summary><b>Answer</b></summary>
<p>

##### 
```
function sum(num1, num2) {
    "use strict";
    num1 = 3, num2 = 7;
    return arguments[0] + arguments[1];
}
console.log(sum(2,2)); // 4
```
 
</p>
</details>

---

#### 113. WeakSet vs WeakMap
<details><summary><b>Answer</b></summary>
<p>

##### 
**🔹 What “Weak” Means**
	-	Holds weak references
	-	Garbage Collector can clean them
	-	No memory leaks

WeakMap
-------------------------------------------------------------
| Feature           | WeakMap        |
| ----------------- | -------------- |
| Key type          | Object only    |
| Iteration         | ❌ Not iterable |
| Garbage collected | ✅              |
| Size              | ❌ No `.size`   |

**📌 Real Angular Use Case – Private metadata**
```
const metadata = new WeakMap<object, any>();
export class UserComponent {
  constructor() {
    metadata.set(this, { loadedAt: Date.now() });
  }
  ngOnDestroy() {
    // No need to manually clean
  }
}
```
➡️ When component is destroyed → metadata auto removed.

WeakSet
-------------------------------------------------------------
| Feature           | WeakSet      |
| ----------------- | ------------ |
| Stores            | Objects only |
| Iteration         | ❌            |
| Garbage collected | ✅            |
```
const activeComponents = new WeakSet<object>();

class ModalComponent {
  constructor() {
    activeComponents.add(this);
  }
}
```
**🔹 WeakMap vs Map (Interview Gold 🥇)**
| Feature           | Map      | WeakMap     |
| ----------------- | -------- | ----------- |
| Key types         | Any      | Object only |
| Iteration         | Yes      | No          |
| Garbage collected | No       | Yes         |
| Memory leaks      | Possible | Prevented   |

</p>
</details>

---

#### 114. Why Map over Object in Angular services?
<details><summary><b>Answer</b></summary>
<p>

##### 
	-	Keys can be objects
	-	Faster for frequent add/remove
	-	Predictable iteration
	-	No prototype pollution
 
</p>
</details>

---

#### 115. Set vs Array performance?
<details><summary><b>Answer</b></summary>
<p>

##### 
	-	Set.has() → O(1)
	-	Array.includes() → O(n)
Use Set for large collections.
 
</p>
</details>

---

#### 116. Event Loop & Asynchronous Task Execution in JavaScript
<details><summary><b>Answer</b></summary>
<p>

##### 
JavaScript is single-threaded, but non-blocking via the event loop.  

**Key components**
	-	Call Stack – Executes synchronous code
	-	Web APIs – Browser-managed async operations (setTimeout, fetch, DOM events)
	-	Task (Macro-task) Queue – setTimeout, setInterval, UI events
	-	Microtask Queue – Promises (then, catch), queueMicrotask
	-	Event Loop – Orchestrates execution order
**Execution order**
	-	Run synchronous code (call stack)
	-	Execute all microtasks
	-	Render (browser paint)
	-	Execute one macrotask
	-	Repeat
```
console.log('A');
setTimeout(() => console.log('B'));
Promise.resolve().then(() => console.log('C'));
console.log('D');
// Output: A D C B
```
 
</p>
</details>

---

#### 117. 𝗘𝘃𝗲𝗻𝘁 𝗟𝗼𝗼𝗽 𝗱𝗲𝘁𝗮𝗶𝗹 𝗱𝗲𝗰𝗶𝗱𝗲𝘀 𝗶𝗻𝘁𝗲𝗿𝘃𝗶𝗲𝘄 𝗼𝘂𝘁𝗰𝗼𝗺𝗲𝘀: 𝗠𝗶𝗰𝗿𝗼𝘁𝗮𝘀𝗸𝘀 𝘃𝘀 𝗠𝗮𝗰𝗿𝗼𝘁𝗮𝘀𝗸𝘀 𝘃𝘀 𝗾𝘂𝗲𝘂𝗲𝗠𝗶𝗰𝗿𝗼𝘁𝗮𝘀𝗸.
<details><summary><b>Answer</b></summary>
<p>

##### 

```
console.log('Start');
setTimeout(() => console.log('Timeout'), 0);
Promise.resolve().then(() => console.log('Promise'));
queueMicrotask(() => console.log('Microtask'));
console.log('End');
```

> Output: Start → End → Promise → Microtask → Timeout

**𝗪𝗵𝘆 𝘁𝗵𝗶𝘀 𝗼𝗿𝗱𝗲𝗿?** 
JavaScript doesn’t just have “async”.
-------------------------------------------------------
	-	It has priorities.
	-	Execution Order
 	-	Call Stack runs first
 	-	Microtask Queue (Promises, queueMicrotask)
	-	Render
	-	Macrotask Queue (setTimeout, events)

𝗕𝗲𝗰𝗮𝘂𝘀𝗲:
	-	Promises go to the Microtask Queue
	-	setTimeout goes to the Macrotask Queue
	-	Microtasks always run first

𝗧𝗵𝗲 𝗲𝘃𝗲𝗻𝘁 𝗹𝗼𝗼𝗽 𝗽𝗿𝗶𝗼𝗿𝗶𝘁𝗶𝘇𝗲𝘀 𝘁𝗮𝘀𝗸𝘀 𝗱𝗶𝗳𝗳𝗲𝗿𝗲𝗻𝘁𝗹𝘆:
-------------------------------------------------------
	-	𝗠𝗶𝗰𝗿𝗼𝘁𝗮𝘀𝗸𝘀 (Promises, queueMicrotask) execute immediately after the current script, before ANY macrotask. They run until the queue is completely empty.
	-	𝗠𝗮𝗰𝗿𝗼𝘁𝗮𝘀𝗸𝘀 (setTimeout, setInterval, I/O) wait their turn. Even setTimeout(0) goes to the back of the line.
	-	𝗾𝘂𝗲𝘂𝗲𝗠𝗶𝗰𝗿𝗼𝘁𝗮𝘀𝗸 explicitly adds to the microtask queue - same priority as Promise callbacks.

Why 𝗾𝘂𝗲𝘂𝗲𝗠𝗶𝗰𝗿𝗼𝘁𝗮𝘀𝗸 exist?
-------------------------------------------------------
	-	𝗾𝘂𝗲𝘂𝗲𝗠𝗶𝗰𝗿𝗼𝘁𝗮𝘀𝗸 schedules microtasks but doesn’t provide Promise semantics like chaining, value propagation, or error handling.
	-	Promise chaining works because each .then() returns a new Promise.

𝗧𝗵𝗶𝘀 𝗮𝗳𝗳𝗲𝗰𝘁𝘀 𝗿𝗲𝗮𝗹 𝗮𝗽𝗽𝗹𝗶𝗰𝗮𝘁𝗶𝗼𝗻𝘀:
-------------------------------------------------------
	-	If you chain too many microtasks, you can block rendering.
	-	If you need guaranteed execution order across async operations, microtasks are your friend.

𝗜𝗺𝗽𝗮𝗰𝘁 :
-------------------------------------------------------
	-	Smooth UI
	-	Predictable behavior
	-	Clean mental model

 
</p>
</details>

---

#### 118. If Cookies in Devtools application tab then how it is secured? Attacker can see the Cookie value ?
<details><summary><b>Answer</b></summary>
<p>

##### 
DevTools visibility ≠ insecurity

If an attacker can open your browser DevTools (Application → Cookies / LocalStorage) 👉 your security is already broken Because that means the attacker is running code in the user’s browser. At that point:
Cookies, LocalStorage, Session memory, JS variables, Network calls are ALL compromised.
> Security is not about hiding from the user. Security is about protecting against the attacker.
> A remote attacker cannot open DevTools on someone else’s machine.
> 3 REAL attack vectors : Network attacks, Cross-Site Scripting (XSS), Cross-Site Request Forgery (CSRF). DevTools visibility protects against none of these

**1️⃣ Cookies Security (why seeing them is OK). Secure cookies are protected by scope & access rules**
```
Set-Cookie: sessionId=abc123;
HttpOnly;
Secure;
SameSite=Strict;
```
> User can see HttpOnly cookies in DevTools, but JavaScript cannot read them
That means: ❌ XSS can’t steal them ❌ JS can’t exfiltrate them ✅ Browser still sends them automatically

**2️⃣ LocalStorage Security (why it’s weaker)**
```
localStorage.getItem('token');
```
✅ Readable by any JS on the page. That includes: Malicious scripts, XSS payloads, Browser extensions.
> LocalStorage is not for secrets. It is for: UI state, Feature flags, Cached preferences, Non-sensitive tokens (sometimes)
> If XSS exists, LocalStorage is instantly compromised

**3️⃣ “But attacker can still SEE cookies in DevTools!”**  
Yes — and that’s fine. Because:  
> Attacker already must have: 1) Physical access OR 2) Malware on the machine OR 3) Remote desktop access 
> At that point: Your banking apps, Password managers, OS credentials are already compromised.

**4️⃣ What DevTools visibility does NOT mean**  
❌ It does NOT mean: 1) Anyone on the internet can see your cookies 2) Cookies are “exposed” 3) LocalStorage is automatically insecure

**5️⃣ Real security boundary (this is the key insight)**  
Browser enforces origin isolation
```
https://bank.com
https://evil.com
```
>👉 evil.com CANNOT: 1) Read bank.com cookies 2) Read bank.com localStorage 3) Access bank.com JS memory, Even if attacker opens DevTools on evil.com.

**6️⃣ Compare attack scenarios (interview-ready)**
| #  | Attack Scenario                | Attack Vector (How it happens)         | Simple Example                         | Impact                     | Key Prevention / Mitigation             |
| -- | ------------------------------ | -------------------------------------- | -------------------------------------- | -------------------------- | --------------------------------------- |
| 1  | **XSS (Cross-Site Scripting)** | Injecting malicious JS into web pages  | `<script>alert(1)</script>` in comment | Session hijack, data theft | Output encoding, CSP, avoid `innerHTML` |
| 2  | **CSRF**                       | Exploits authenticated browser session | Auto-submit hidden form                | Unauthorized actions       | CSRF tokens, SameSite cookies           |
| 3  | **SQL Injection**              | Malicious SQL via input fields         | `' OR 1=1 --`                          | Data leakage, DB takeover  | Prepared statements, ORM                |
| 4  | **Command Injection**          | OS command execution via inputs        | `; rm -rf /`                           | Server compromise          | Input validation, least privilege       |
| 5  | **Broken Authentication**      | Weak auth/session handling             | Guessable passwords                    | Account takeover           | MFA, strong password policies           |
| 6  | **IDOR**                       | Insecure direct object reference       | `/api/user/123`                        | Unauthorized data access   | Authorization checks                    |
| 7  | **Man-in-the-Middle (MITM)**   | Intercepting traffic                   | Fake Wi-Fi hotspot                     | Credential theft           | HTTPS, HSTS                             |
| 8  | **Replay Attack**              | Reusing valid requests                 | Resend payment request                 | Duplicate actions          | Nonces, timestamps                      |
| 9  | **Clickjacking**               | Hidden UI overlays                     | Invisible iframe                       | Forced user actions        | `X-Frame-Options`, CSP                  |
| 10 | **Brute Force**                | Repeated login attempts                | Password guessing                      | Account compromise         | Rate limiting, CAPTCHA                  |
| 11 | **DDoS**                       | Flooding server with traffic           | Botnet traffic                         | Service outage             | Rate limiting, CDN, WAF                 |
| 12 | **Supply Chain Attack**        | Compromised dependencies               | Malicious npm package                  | Full app compromise        | Lockfiles, dependency audits            |
| 13 | **Deserialization Attack**     | Unsafe object deserialization          | Crafted JSON payload                   | RCE                        | Avoid native deserialization            |
| 14 | **Privilege Escalation**       | Gaining higher permissions             | User → Admin                           | System takeover            | Role checks, least privilege            |
| 15 | **SSRF**                       | Server makes internal requests         | Access `localhost`                     | Cloud metadata leak        | URL allow-list, network isolation       |

**7️⃣ So what actually makes it secure?**
Defense-in-depth : 1) HttpOnly cookies 2) SameSite=Strict/Lax 3) CSP (Content Security Policy) 4) XSS prevention 5) Short-lived sessions 6) Token rotation, Not “hiding values from DevTools”.


</p>
</details>

---
