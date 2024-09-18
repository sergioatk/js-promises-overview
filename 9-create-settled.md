# Create Settled Promises

If you want a promise to represent a previously computed value there's no need to create a promise executor and handle a hardcoded `resolve` or `reject`.

## `Promise.resolve()`

Method that accept a single argument and return a promise in the fulfilled state.

```javascript
const promiseA = Promise.resolve(2);
const promiseB = new Promise((resolve) => {
  resolve(2);
});

console.log("promiseA =>", promiseA);
console.log("promiseB =>", promiseB);
```

If you pass a promise to Promise.resolve(), then the same promuse is returned.

```javascript
const userPromise1 = Promise.resolve(user);
const userPromise2 = Promise.resolve(promise1);

console.log(userPromise1 === userPromise2);
```

## `Promise.reject()`

Works just like `Promise.resolve()`, except the created promise is in the `rejected` state.

```javascript
const userPromise = Promise.resolve("failed!");

userPromise.catch((reason) => {
  console.log("reason ->", reason); // 'failed!'
});
```

```javascript
// QUIZZ: could I use multiple catchs?
const promise = Promise.reject(2);

promise
  .catch((reason) => {
    console.log("reason 1 =>", reason);
    return reason;
  })
  .catch((reason) => {
    console.log("reason 2 =>", reason);
    return reason;
  })
  .catch((reason) => {
    console.log("reason 3 =>", reason);
  })
  .then((value) => {
    console.log("value =>", value);
  });
```
