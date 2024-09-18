# Handlers

Son funciones a través de las cuales podemos acceder al estado settled de una promesa.

## then()

- Toma dos parámetros:

  1. Una función que va a ejecutarse cuando la promesa se resuelva exitosamente.
  2. Una función que va a ejecutarse cuando la promesa se resuelva no-exitosamente.

- Returns a new promise with the returned value.

```javascript
const promise = fetch('my-api.com/users');

promise.then(
  (res) => {
    console.log(res); // users
    // hago uso de la data
  },
  (reason) => {
    console.log(reason); // error
    // hago el manejo de error correpsondiente
  }
);
```

Para manejar solo el error entonces, se debería pasar `null` como primer parámetro:

```javascript
const promise = fetch('my-api.com/users');

promise.then(null, (reason) => {
  console.log(reason); // error
  // hago el manejo de error correpsondiente
});
```
