# Thenable

All Promise-like objects implement the Thenable interface. A thenable implements the .then() method, which is called with two callbacks: one for when the promise is fulfilled, one for when it's rejected. Promises are thenables as well.

To interoperate with the existing Promise implementations, the language allows using thenables in place of promises

```javascript
const thenable = {
  then: function (resolve, reject) {
    resolve("Handled by Promise.resolve!");
  },
};

Promise.resolve(thenable)
  .then((value) => {
    console.log("value =>", value);
  })
  .catch((reason) => {
    console.log("reason =>", reason);
  });
```
