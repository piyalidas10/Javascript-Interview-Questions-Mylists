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
  - 
  
  
 </p>
</details>

---
