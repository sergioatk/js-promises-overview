# Async functions.

Feature that makes programming with promises more similar to programming without promises. Instead of worry about tracking promises and their various handlers, async functions abstract awai the promises. The end result is code that follows a familiar top-down sequence.

```
In computer science, syntactic sugar is syntax within a programming language that is designed to make things easier to read or to express
```

## Defining async functions

Can be used anywhere synchronous functions can be used. All you need to do is add the async keyword before any function or method definition to make it asynchronous.

## Why write it?

It's important for the engine to know ahead of time if a functino is asynchronous because it behaves differently than async function. It goes throught a lot of trouble to ensure that.

## What makes async functions different?

1. The return value is always a promise
2. Thrown errors are promise rejections
3. The await expression can be used
4. The for-await-of loop can be used

### The resturn value is always a promise

Async functions always return a promise refardless of the type of value you specify with return. If you return a number, for example, that number is wrapped in a promise.

```javascript
const get22 = async () => {
  return 22;
};

const result = get22();

console.log("result =>", result);

result.then((value) => {
  console.log("value =>", value);
});
```

Async functions call `Promise.resolve()` behind the scenes to ensure a promise is always returned. No matter wht you do inside the async function, it will always return a promise.

That is also the case when an error is thrown.

```javascript
const throwError = async () => {
  throw new Error("upppss");
};

try {
  const result = throwError();
  console.log("result =>", result);
} catch (error) {
  console.log("handling error =>", error);
}
```

In this case, the error will not be handled, because the async function returns a rejected promise. To catch errors, we need to provide a rejection handler, like so:

```javascript
const throwError = async () => {
  throw new Error("upppss");
};

const result = throwError().catch((reason) => {
  console.log("reason =>", reason);
});
```

### Using `await` expression

<span style="background-color: #7f00ff">await => te voy a hacer un manejo bien piola de esta función.</span>

This expression is designed to make working with promises simple. Instead of manually assigning fulfillment and rejection handlers, the expression returns:

- the fulfilled value of a promise when it succeeds
- throws the rejection value when the promise fails.

This allows you to easily assign the result of an await expression to a variable and catch any rejections using a try catch.

```javascript
const getNumber = async (number) => {
  if (typeof number !== "number") {
    console.log("I have to throw an error because the number is not a number");
    throw new Error("only numbers are allowed");
  }
  return number;
};

const handleThings = async () => {
  try {
    return await getNumber(1);
  } catch (error) {
    console.log("PLEASE STOP THE MONEY TRANSFER PLS PLS PLS PLS PLS =>");
    throw error;
  }
};

const main = async () => {
  try {
    const things = await handleThings();
    console.log("things =>", things);
  } catch (error) {
    console.log("Im handing the error, dont worry boss");
    console.log("error to return to client =>", error.message);
  }
};

main();
```

![App Platorm](https://t4.ftcdn.net/jpg/00/92/81/43/360_F_92814346_DYOPLEjk9XKZPAz8hopvx4Uv51oXHq8K.jpg)

It's the await expression that causes rejection to be thrown as errors. So if you omit await, the promise rejection will only be caught by a rejection handler.

#### Using await on non promise values

You can use await on non promise values, as the values are passed through Promise.resolve()

```javascript
const getNumber = async (number) => {
  if (typeof number !== "number") {
    console.log("I have to throw an error because the number is not a number");
    throw new Error("only numbers are allowed");
  }
  return number;
};

const main = async () => {
  const number = await getNumber(10);
  console.log("number =>", number);
};

main();
```

This means we don't get penalized if you guess incorrectly about the value veing used.

### Top level await

`await` can be used at the top level of a js module outside an async function. A js module acts as an async functino wrapped around the entire module by default.
