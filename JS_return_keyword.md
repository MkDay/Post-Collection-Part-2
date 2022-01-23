# return keyword JavaScript - A hidden hero we do not care about 

Many of us, including me, do not care about some of the tiny pieces in a programming language that maybe we use more often without even thinking of it.
 

By considering JavaScript, the **_return_** keyword becomes more crucial because every built-in function or custom function may have a return value that defines using this *return* keyword.

So, let me unwrap things that make the *return* keyword so important.


### 1. Always resides inside a function

First of all, it only can be used inside a function. For instance, if we use it in the global scope or inside a loop, we will get an error message like *Uncaught SyntaxError: Illegal return statement*.

Let us try to return something in the global scope.

```javascript

/* 
* try to return a value in the global scope,
* and it only gives an error 
*/

console.log("before the return statement");

return 0;

console.log("after the return statement");

/*
output:
Uncaught SyntaxError: Illegal return statement

*/

```

Instead, we can put that block of code inside a function.

```javascript

function returnSomething() {
  console.log("before the return statement");

  return 0;

  console.log("after the return statement");
}

const returnValue = returnSomething(); 
console.log(returnValue);

/*
output:
"before the return statement"
0
*/

```

And, let us try to use the *return* keyword inside a for-loop.

```javascript

/*
* try to return a value inside a for-loop
* and it is also never going to run
*/

for (let i = 1; i <= 5; i++) {
  if (i === 4) {
    console.log("reach to the value: " + i);
    return i;
  }
  console.log("current value: " + i);
}

console.log("exit from the loop");

/*
output:
Uncaught SyntaxError: Illegal return statement
*/

```

To solve the problem, wrap it up using a function.


```javascript

function returnSomething() {
  for (let i = 1; i <= 5; i++) {
    if (i === 4) {
      console.log("reach to the value: " + i);
      return i;
    }
    console.log("current value: " + i);
  }
  console.log("exit from the loop");
}

const returnValue = returnSomething();

console.log(returnValue);

/*
output:

"current value: 1"
"current value: 2"
"current value: 3"
"reach to the value: 4"
4

*/

```

The function above will never print the last console message. That is,

`console.log("exit from the loop")`. 

Because, when the *if* condition `(i === 4)` becomes true, it will be immediately returned from the function without executing any further. So, that console message becomes **_unreacheable code_**.

 
In addition, here is how to use the *return* keyword with the *switch-case* statement.

```javascript

function returnSomething() {
  let fruit = "apple";

  switch (fruit) {
    case "banana":
      console.log("banana");
      return;

    case "orange":
      console.log("orange");
      return;

    case "apple":
      console.log("apple");
      return;

    default:
      console.log("no fruits here");
  }
  console.log("outside the switch-case");
}

const returnValue = returnSomething();
console.log(returnValue);

/*
output:
"apple"
undefined
*/

```


### 2. Use return with or without a value

It is not compulsory to use the *return* statement inside every function. 

However, still, every function has a return value that is `undefined`.

```javascript

function withoutReturn() {
  console.log("no return value for this function");
}

const returnValue = withoutReturn();
console.log(returnValue);

/*
output:

"no return value for this function"
undefined
*/

```



### 3. Automatic Semicolon Insertion (ASI)

Javascript does not allow to use line terminator between the `return` keyword and the return expression. See the below example. It returns `undefined` instead of `a + b` value. And, `a + b` becomes as *unreachable code*.

```javascript

function returnSomething(a, b) {
  return
  a + b;
}

const returnValue = returnSomething(4, 5);
console.log(returnValue); // undefined

```

Instead, we can use parentheses to wrap the return expression.

```javascript

function returnSomething(a, b) {
  return (
  a + b);
}

const returnValue = returnSomething(4, 5);
console.log(returnValue); // 9

```


### 4. Types of return values

We can return a value in any type using the `return` keyword. For instance, we can return values which are,
 
* in the primitive data types such as string, number, boolean, etc.
* in the object types such as arrays, objects, and functions. 

Plus, we can return console messages as well. 

#### (1) Primitive types 

* **return undefined:**

```javascript

function returnType() {
  console.log("return value: undefined");
  return undefined;
}

const returnValue = returnType();
console.log(returnValue);

/*
output:

"return value: undefined"
undefined
*/

```

* **return null**

```javascript

function returnType() {
  console.log("return value: null");
  return null;
}

const returnValue = returnType();
console.log(returnValue);

/*
output:

"return value: null"
null
*/

```
* **return string:**

```javascript

function returnType() {
  console.log("return value: string");
  return "Hello World";
}

const returnValue = returnType();
console.log(returnValue);

/*
output:

"return value: string"
"Hello World"
*/

```
* **return number:**

```javascript

function returnType() {
  console.log("return value: number");
  return 5;
}

const returnValue = returnType();
console.log(returnValue);

/*
output:

"return value: number"
5
*/

```
* **return boolean:**

```javascript

function returnType() {
  console.log("return value: boolean");
  return false;
}

const returnValue = returnType();
console.log(returnValue);

/*
output:

"return value: boolean"
false
*/

```

#### (2) Object types 

* **return an array:**

```javascript

function returnType() {
  let name = "Bob";
  let age = 14;
  let id = 123;
  return [name, age, id];
}

const [name, age, id] = returnType();
console.log(name, age, id);

/*
output:
"Bob" 14 123
*/

```


* **return an object:**

```javascript

function returnType() {
  let name = "Bob";
  let age = 14;
  let id = 123;
  return {name, age, id};
}

const {name, age, id} = returnType();
console.log(name, age, id);

/*
output:
"Bob" 14 123
*/

```

* **return a function:**

```javascript

function returnType() {
  return function returnFunction(name, age) {
    console.log(name + " is " + age + " years old.");
  };
}

const returnValue = returnType();
returnValue("Bob", 14);

/*
output:

"Bob is 14 years old."
*/

```

#### (3)return a console message

```javascript

function returnType() {
  return console.log("Hello World");
}

const returnValue = returnType();

/*
output:
"Hello World"
*/

```

### 5. Difference between *break* & *return*

The `return` keyword that resides inside a loop stops the function execution and immediately exits from the function. So, the code below the loop becomes as *unreachable*. 

However, if we use the `break` statement instead, we can still get the code below the loop.

```javascript

function useBreak() {
  for (let i = 1; i <= 5; i++) {
    if (i === 4) {
      console.log("reach to the value: " + i);
      break;
    }
  }
  console.log("outside the loop");
}

useBreak();

/*
output:

"reach to the value: 4"
"outside the loop"
*/

```

```javascript

function useReturn() {
  for (let i = 1; i <= 5; i++) {
    if (i === 4) {
      console.log("reach to the value: " + i);
      return;
    }
  }
  console.log("outside the loop");
}

useReturn();

/*
output:

"reach to the value: 4"
*/

```

### Conclusion

Okay, now we have finished talking about the tiny but so important piece of JavaScript. That is the *return* keyword. So to wrap this up, let me summarize things that we have discussed above.

* It only can be used inside a function. 
* Mainly, it stops the function execution at that point and returns the specified value to the function caller. 
* When executing the return statement, the code that comes after it becomes as *unreachable code*. 
* JavaScript does not allow line terminators between the *return* keyword and the expression.

I hope you enjoyed this article, and you can support me at [ko-fi](https://ko-fi.com/mkdaycode). I always appreciate your support. It really encourages me to keep going. 

 **_Happy Coding!_** 

[![image_description](https://cdn.ko-fi.com/cdn/kofi2.png?v=3)](https://ko-fi.com/mkdaycode) 
