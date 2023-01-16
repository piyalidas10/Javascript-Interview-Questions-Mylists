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
  JavaScript is single-threaded: only one task can run at a time. Usually that’s no big deal, but now imagine you’re running a task which takes 30 seconds.. Ya.. During that task we’re waiting for 30 seconds before anything else can happen (JavaScript runs on the browser’s main thread by default, so the entire UI is stuck) 😬 It’s 2019, no one wants a slow, unresponsive website.

Luckily, the browser gives us some features that the JavaScript engine itself doesn’t provide: a Web API. This includes the DOM API, setTimeout, HTTP requests, and so on. This can help us create some async, non-blocking behavior.


When we invoke a function, it gets added to something called the call stack. The call stack is part of the JS engine, this isn’t browser specific. It’s a stack, meaning that it’s first in, last out (think of a pile of pancakes). When a function returns a value, it gets popped off the stack.
![popped off the stack](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/images/call_stack.gif)


The respond function returns a setTimeout function. The setTimeout is provided to us by the Web API: it lets us delay tasks without blocking the main thread. The callback function that we passed to the setTimeout function, the arrow function () => { return 'Hey' } gets added to the Web API. In the meantime, the setTimeout function and the respond function get popped off the stack, they both returned their values!
![setTimeout](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/images/setTimeout.gif)


In the Web API, a timer runs for as long as the second argument we passed to it, 1000ms. The callback doesn’t immediately get added to the call stack, instead it’s passed to something called the queue.
![callback_queue](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/images/callback_queue.gif)


This can be a confusing part: it doesn't mean that the callback function gets added to the callstack(thus returns a value) after 1000ms! It simply gets added to the queue after 1000ms. But it’s a queue, the function has got to wait for its turn!

Now this is the part we’ve all been waiting for… Time for the event loop to do its only task: connecting the queue with the call stack! If the call stack is empty, so if all previously invoked functions have returned their values and have been popped off the stack, the first item in the queue gets added to the call stack. In this case, no other functions were invoked, meaning that the call stack was empty by the time the callback function was the first item in the queue.
![event_loop](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/images/event_loop.gif)


The callback is added to the call stack, gets invoked, and returns a value, and gets popped off the stack.
![callstack_executed](https://github.com/piyalidas10/Javascript-Interview-Questions-Mylists/images/callstack_executed.gif)


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
![output](https://github.com/Javascript-Interview-Questions-Mylists/images/output.gif)

- 1. We invoke bar. bar returns a setTimeout function.
- 2. The callback we passed to setTimeout gets added to the Web API, the setTimeout function and bar get popped off the callstack.
- 3. The timer runs, in the meantime foo gets invoked and logs First. foo returns (undefined),baz gets invoked, and the callback gets added to the queue.
- 4. baz logs Third. The event loop sees the callstack is empty after baz returned, after which the callback gets added to the call stack.
- 5. The callback logs Second.
  
  
 </p>
</details>

---
