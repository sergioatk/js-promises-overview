# .catch()

Internamente usa `.then()`. Es una forma más clara y menos verbosa.

**It only handle errors.**

```javascript
const promise = fetch("my-api.com/users");

promise.catch((reason) => {
  console.log(reason); // error
  // hago el manejo del error
});
```

# Manejo de errores

Cuando la promesa falla, podemos manejar el error.

Si no lo hacemos, dependiendo el entorno donde estemos ejecutando nuestro código, nos va a alertar que se lanzó un error no manejado.

```javascript
const promise = new Promise((_, reject) => {
  setTimeout(() => {
    reject("Promise failed!");
  }, 1000);
});

setTimeout(() => {
  promise.catch((reason) => {
    console.log("reason =>", reason);
  });
}, 500);
```
