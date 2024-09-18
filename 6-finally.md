# .finally()

Es un handler que se va a ejecutar tanto cuando la promesa resuelve exitosamente, como si no.

Lo usaremos cuando, sin importar el resultado de la misma, queramos realizar cierto procedimiento.

No toma ningún argumento.

## Con `then` y `catch`

```javascript
const promise = Promise.reject(3);

promise
  .then((res) => {
    console.log('Success, stopping spinner');
    console.log(res); // datos recibidos
  })
  .catch((reason) => {
    console.log('Failure, stopping spinner');

    console.log(reason); // error
  });
```

## Usando finally

```javascript
const promise = Promise.resolve(3);

promise
  .then((res) => {
    console.log('Success');
    console.log(res); // datos recibidos
  })
  .catch((reason) => {
    console.log('Failure');
    console.log(reason); // error
  });
// .finally(() => {
//   console.log('Either way spinner is stoppe'); // No repetimos código. La intención es clara. Sin importar qué pase, queremos detener el spinner.
//   throw new Error('error!');
// });

// process.on('unhandledRejection', (reason, promise) => {
//   console.log('Unhandled Rejection at:', promise, 'reason:', reason);
//   // Recommended: throw the error to crash the app and handle it in a process manager
//   throw reason;
// });
```

## Returned value

A caviat with finally is that it returns a promise, but with the same value as the previous resolved promise.

```javascript
const promise = Promise.resolve(3);

promise
  .then((res) => {
    console.log('Success');
    console.log(res); // datos recibidos

    return res;
  })
  .catch((reason) => {
    console.log('Failure');
    console.log(reason); // error
  })
  .finally((finallyValue) => {
    console.log('finallyValue =>', finallyValue);
    console.log('Either way spinner is stopped'); // No repetimos código. La intención es clara. Sin importar qué pase, queremos detener el spinner.
    return 'pepito';
  })
  .then((reason) => {
    console.log('reason =>', reason);
  });
```

The exceptions to this are:

- When a rejected promise is returned from it.
- When a value is thrown from it.

In those cases ot will return a promise with that rejected reason.

```javascript
const promise = Promise.reject(43);

promise
  .finally(() => {
    console.log('finally called');

    // throw 'new error';
    return 'new error';
  })
  .catch((reason) => {
    console.log('reason in catch =>', reason);
  });
```
