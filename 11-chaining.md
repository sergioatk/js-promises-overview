# Chaining promises

Each call to `then()`, `catch()` or `finally()` returns another promise. This new promise settles only after the first has been fulfilled or rejected.

```javascript
const promise = Promise.resolve(42);

promise
  .then((value) => {
    console.log('value ->', value);
  })
  .then((value) => {
    console.log('Finished!');
  });
```
