# Handle error

With handler chaining we can handle an error thrown in any link in the chain.

```javascript
const promise = Promise.resolve(42);

promise
  .then((value) => {
    console.log('resolved value ->', value);
  })
  .then(() => {
    console.log('Last .then()');
    throw new Error('User was not found in db');
  })
  .catch((value) => {
    // will handle any of the .then()
    console.log('An error happened!');
    console.log('value ->', value);
  })
  .catch((value) => {
    console.log('value2 =>', value);
  })
  .finally(() => {
    // view position
    console.log('stop webapp fetch spinner');
  });
```

## Vs try-catch

Unlike a try-catch synchronous statement, you don't want finally to be the last part of the chain. Becuse if an error could be thrown in finally, we would like that error to be handled, thats why use catch at the end of the chain.
