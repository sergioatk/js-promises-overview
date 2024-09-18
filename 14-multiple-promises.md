# `Promise.all()`.

Accepts one argument, an iterable of promises.

Returns one promise, that is settled ONLY when EVERY promise in the iterable is resolved.

It fulfills ONLY when EVERY promise in the iterable is fulilled. It resolves to a iterable of the provided fulfilled promises in the provided order.

```javascript
const promise1 = Promise.resolve(42);

const promise2 = new Promise((resolve) => {
  console.log('thinking...');
  setTimeout(() => {
    resolve(43);
  }, 400);
});

const promise3 = Promise.resolve(44);

Promise.all([promise1, promise2, promise3])
  .then((values) => {
    console.log('values =>', values);
  })
  .catch((reason) => {
    console.log('reason =>', reason);
  });
```

If any promise is rejected, the returned promise is immediately rejected without waiting for the other promises to complete

```javascript
const promise1 = Promise.reject('ERROR');

const promise2 = new Promise((resolve) => {
  console.log('thinking...');
  setTimeout(() => {
    resolve(43);
  }, 400);
});

const promise3 = Promise.resolve(44);

Promise.all([promise1, promise2, promise3])
  .then((values) => {
    console.log('values =>', values);
  })
  .catch((reason) => {
    console.log('reason =>', reason);
  });
```

The rejection handler always receives a single value rather than an iterable, and the vlaue is the rejectino value from the promise that was rejected.

QUIZ: Why?

Also, non-promise values in te iterable argument is passed to Promise.resolve() to convert it into a promise.

QUIZ: What happens to the other promises?

## When to use it?

When we are working with multiple promises to fulfill, and any failure should cause the entire operation to fail.

# `Promise.allSettled()`.

A slight vriation of `Promise.all()` where the method waits until all promises in the specified iterable are settled, regardless of weather they are fulfilled or rejected. The return value of Promise.allSettled() is always a promises that is fulfilled with the oterable of result objects.

```javascript
const promise1 = Promise.reject('ERROR');

const promise2 = new Promise((resolve) => {
  console.log('thinking...');
  setTimeout(() => {
    resolve(43);
  }, 400);
});

const promise3 = Promise.resolve(44);

Promise.allSettled([promise1, promise2, promise3])
  .then((values) => {
    console.log('values =>', values);
  })
  .catch((reason) => {
    console.log('reason =>', reason);
  });
```

The result `fulfilled` objects have two properties:

- `status`: always set to the string 'fulfilled'
- `value`: the fulfilled value

The result `rejected` objects have two properties:

- `status`: always set to the string 'rejected'
- `reason`: the rejected value

## When to use it? -> QUIZ

When you want to ignore rejections, handle rejections differently or allow partial success.

# `Promise.any()`.

Accepts an iterable of promises and returns a fulfilled promise when any of the provided promises are fulfilled. The operation short-circuits as soon as one of the promises is fulfilled.

This is the opposite of `Promse.all()`, where the operation short-circuits as soon as one promise is rejected.

If all provided promises are rejected, then the returned promises is rejected with an `AggregateError`.

## When to use it? -> QUIZ

Any situation where you want any one of the promises to fulfill and you don't care how many others reject unless they all reject:

- Hedge requests: where the client makes request to multiple servers an accepts the response from the first that resplies.

```javascript
const promise1 = new Promise((resolve) => {
  setTimeout(() => {
    resolve('43. Source: fast page');
  }, 400);
});

const promise2 = new Promise((resolve) => {
  setTimeout(() => {
    resolve('43. Source: slow page');
  }, 800);
});

Promise.any([promise1, promise2])
  .then((values) => {
    console.log('values =>', values);
  })
  .catch((reason) => {
    console.log('reason =>', reason);
  });
```

Another example

```javascript
const promise1 = new Promise((_, reject) => {
  setTimeout(() => {
    reject('Returning an error very fast');
  }, 400);
});

const promise2 = new Promise((resolve, _) => {
  setTimeout(() => {
    resolve('43. Source: slow page');
  }, 800);
});

Promise.any([promise1, promise2])
  .then((values) => {
    console.log('values =>', values);
  })
  .catch((reason) => {
    console.log('reason =>', reason);
  });
```

# `Promise.race()`.

Accepts an iterable of promises and returns a settled promise as soon as the first promise is settled. Regardless of the status.

Those promises are truly on a race to see which is settled first.

- If the first promise to settle is fulfilled, then the returned promise is fulfilled.
- If the first promise to settle is rejected, then the returned promis is rejected.

This is the opposite of `Promse.all()`, where the operation short-circuits as soon as one promise is rejected.

## When to use it? -> QUIZ

Unlike Promise.any(), where you specifically want one of the promises to succeed, and only care if all promises fail, with Promise.race() you want to know even if one promise fails as long as it fails before any other promise fulfills.

```javascript
function timeout(ms) {
  return new Promise((_, reject) => {
    setTimeout(() => {
      reject('----> TIMEOUT!!');
    }, ms);
  });
}
const promise1 = new Promise((resolve) => {
  setTimeout(() => {
    resolve('Returning results but veeeeery slooooow');
  }, 40000);
});

Promise.race([promise1, timeout(2000)])
  .then((values) => {
    console.log('values =>', values);
  })
  .catch((reason) => {
    console.log('reason =>', reason);
  });
```

Note: promise1 process will not be cancelled, it will continue waiting for a response, even thought the response will be ignored.
