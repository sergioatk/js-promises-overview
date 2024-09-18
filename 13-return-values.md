# Returning values

Promise chain can pass value from one promise to the next.

If the promise handler returns a value, that value becomes the value fo the newly created promise from then/catch.

```javascript
const promise = Promise.resolve(42);

promise
  .then((value) => {
    console.log('value in then 1 =>', value);
    return value + 1;
  })
  .then((value) => {
    console.log('value in then 2 =>', value);
  });
```

The failure of one promise can allow the recovery of the chain. Even though this return value is coming from a rejection handler, it is still used in the fulfillment handler of the next promise in the chain.

```javascript
const promise = Promise.resolve(42);

promise
  .then((value) => {
    console.log('value in then 1 =>', value);
    return value + 1;
  })
  .then((value) => {
    console.log('value in then 2 =>', value);
    throw new Error('Error while fetching house');
  })
  .catch((reason) => {
    console.log('reason =>', reason.message);

    return 'new value after failure';
  })
  .then((value) => {
    console.log('value in then 3 =>', value);
  });
```

# Returning promises

When returning a promise from a promise, a new promise is created.

```javascript
const promise1 = Promise.resolve(42);

const promise2 = Promise.resolve(43);

promise1
  .then((value) => {
    console.log('value promise 1 =>', value);
    console.log('promise2 =>', promise2);

    return promise2;
  })
  .then((value) => {
    console.log('returned value =>', value);
  });
```

Returning a rejected promise from a settlement handler is equivalent to throwing an error: the returned promise is rejected with the specified reason.

```javascript
const promise1 = Promise.resolve(42);

const promise2 = Promise.resolve(43);

promise1
  .then((value) => {
    console.log('value promise 1 =>', value);
    console.log('promise2 =>', promise2);

    return promise2;
  })
  .then((value) => {
    console.log('returned value =>', value);

    return Promise.reject(43);
  })
  .catch((reason) => {
    console.log('reason =>', reason);
  });
```
