# JavaScript - The Hard Parts: Closure, Scope, and Execution Context

## Whenever a function is defined inside another funciton the inner function has access to the outer function's variables
```javascript
//Scope A
function outer() {
  //Scope B
  var counter = 0; //initialize the counter at 0
  function incrementCounter() {
    //Scope C <-- counter is accessible here because it is defined at the same time as counter is set equal to 0
    counter++;
  }
}

outer()
```

Notice we haven't even called incrementCounter yet - we're just saying inside it, we could access Scope B and Scope A - based on its definition location

This is called:
- static scope
- lexical scope

    Call stack
|====================|
|--------------------|
|                    |
|                    |
|                    |
|      -------       |
| incrementCounter() |
|      -------       |
|      Outer()       |
|      -------       |
|      Global()      |
|--------------------|
|====================|


Now lets run our code and call incrementCounter in the same scope in which it was defined
```javascript
function outer() {
  var counter = 0; // accessible outside
  //Declare incrementCounter() with the functionality of increasing counter one time infinitely
  function incrementCounter() {
    counter++;
  }
  return incrementCounter;
}

incrementCounter();
```
What happens here?

There is a way to run a function outside where it was defined - return the function and assign it to a new variable
```javascript
function outer() {
  var counter = 0; // accessible on a global level
  //Declare incrementCounter() with the functionality of increasing counter one time without constraints
  function incrementCounter() {
    counter++;
  }
  // return the functionality of incrementCounter (counter++) run in real time
  return incrementCounter;
}

var myNewFunction = outer(); // myNewFunction = incrementCounter

LOCAL v.e (variable env): counter: 0 - incrementCounter: {f} = +
```
myNewFunction() - calling creates a new local scope (aka execution context with a global memory) - adds to the call stack so currently we have:

|====================|
|--------------------|
|                    |
|                    |
|                    |
|                    |
|                    |
|      -------       |
|      myNewfunc()   |
|      -------       |
|      Global()      |
|--------------------|
|====================|

myNewFunction()
counter++ = functionality put on the primary local stack

myNewFunction() called again - calls first line of code which is counter++ because we already declared the variable counter = 0; which is accessible between all functions, therefore counter++ is accessible between all memory of the functions (i.e. all this data is passed between all functions)



|====================|
|--------------------|
|                    |
|                    |
|                    |
|                    |
|                    |
|      -------       |
|      myNewfunc()   |
|      -------       |
|      Global()      |
|--------------------|
|====================|


What happens if we execute myNewFunction again?
```javascript
function outer() {
  var counter = 0;
  function incrementCounter(){
    counter++;
  }
  return incrementCounter;
}

var myNewFunction = outer(); // myNewFunction = incrementCounter
myNewFunction();
myNewFunction();
```
[] = secret scope, that references the whole life store of data where the function is being defined.

So global memory is set as follows:

Global Memory
---

outer: [{f}]

myNewFunction: [{f}] -> counter: != 0 (basically)

As the backpack gets bigger and bigger, make sure you're referencing backpacks on backpacks (references go further and further out)

Heap
---

Unstructured data store, where any objects, functions, variables that we don't interact with. All the data inside the backpack is referencing the data in the heap

if you were to console.log that function you wouldn't get anything it's only acting as an object

---

If we had counter in global memory - as soon as javascript hits a character it stops.

-----Call this persistence lexical scope references----- (this is how closure is defined as as it is an ambiguous name)


What if we run 'outer' again and store the returned 'incrementCounter' in 'anotherFunction'?

// closed over the variable environment is a closure
```javascript
function outer (){
  var counter = 0;
  function incrementCounter() {
    counter++;
  }
  return incrementCounter;
}

var myNewFunction = outer();
myNewFunction();

var anotherFunction = outer(); // myNewFunction = incrementCounter
anotherFunction();
```
[[]] = hidden scope property that holds all data.
