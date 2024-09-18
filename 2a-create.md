# New (unsettled promise)

Unsettled promises are created using the Promise constructor. It accept a single argument a function called `executor`. It contains the code to initialize the promise.

The executor takes two arguments named `resolve` and `reject`.

## `resolve`

Called when the executor has finished successfully to signal that the promise is resolved.

## `reject`

Called when the executor has failed. It can be called explicitly, or it's called when an error is thrown inside an executor. There is an implicit try-catch that handles that error an then passes it to a rejection handler.

---

Executors run immediately upon creation of the promise.

#### Example 1

```javascript
const findOrFail = (success) => {
  return new Promise((resolve, reject) => {
    const user = success;
    console.log("calculating some intensive task");
    setTimeout(() => {
      if (user) {
        console.log("Resolving promise...");
        resolve("User object returned");
      }
      console.log("Rejecting promise...");
      reject("Could not find user");
    }, 500);
  });
};

const user = findOrFail(false);
console.log("user promise =>", user);

user
  .then((result) => {
    console.log("result =>", result);
  })
  .catch((reason) => {
    console.log("reason =>", reason);
  });
```
