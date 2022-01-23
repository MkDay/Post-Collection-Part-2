# JS return keyword

Many of us including me, don't care about some of the tiny pieces in a programming language that maybe play a major role in most of the cases.

By considering JavaScript, **_return_** keyword becomes more crucial, because every built-in function or custom function may have a return value that defines using this *return* keyword.

So, let me unwrap some of important things about the *return* keyword.


### 1. Always resides inside a function

First of all it only can be used inside a function. For instance, if we try to use it in a global scope or in a loop, we will get an error message like *Uncaught SyntaxError: Illegal return statement*.

Let's try to return something in a global scope.

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

And, let's try to use return keyword inside a for-loop.

```javascript

/*
* try to return a value inside a for-loop
* and it's also never going to run
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

To solve the problem let's wrap it up using a function.


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

By looking at the above function, we never get the last console message which is `console.log("exit from the loop")`. Because, when the if condition `(i === 4)` becomes true, it will be immediately return from the function without executing any further. So, that console message becomes **_unreacheable code_**.

 
In addition, here is how to use *return* keyword with the *switch-case* statement.

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

It is not compulsory to use `return` statement in every function but still every function has a return value which is `undefined`.

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



### 3. automatic semicolon insertion

Javascript doesn't allow to use line terminator between the `return` keyword and the return expression. See the below example it returns `undefined` instead of `a + b` value. And, `a + b` becomes as *unreachable code*.

```javascript

function returnSomething(a, b) {
  return
  a + b;
}

const returnValue = returnSomething(4, 5);
console.log(returnValue); // undefined

```

Instead we can use parentheses to wrap the return expression.

```javascript

function returnSomething(a, b) {
  return (
  a + b);
}

const returnValue = returnSomething(4, 5);
console.log(returnValue); // 9

```


### 4. types of return values

We can return any type of values using `return` keyword. For instance we can return values which are,
 
* in the primitive data types such as string, number, boolean, etc.
* in the object types such as arrays, objects and functions. 

Plus, we can return console messages as well. 

#### (1) primitive types 

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

#### (2) object types 

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

### 5. difference between *break* & *return*

If we use `return` keyword inside a loop, it will stop the function execution at that point and exit immediately from the function. So, the code below the loop becomes as *unreachable*. 

However, if we use `break` statement instead, we can still get the code that resides below the loop.

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

Okay, now we're finished with talking about the tiny but very important piece of JavaScript which is *return* keyword. So wrap this up, let me summarize things that we've disscussed above.

* It only can be used inside a function. 
* Mainly, it stops the function execution at that point and return the specified value to the function caller. 
* Also if the return statement will be executed then the code that comes after it, becomes as *unreacheable code*. 
* JavaScript doesn't allow the line terminator between the *return* keyword and the expressiin.
