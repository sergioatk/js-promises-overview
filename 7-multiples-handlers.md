# Múltiples handlers

Un handler va a ejecutarse aunque la promesa ya haya sido manejada con otro handler con anterioridad.

Esto permite agregar nuevos handlers en cualquier punto y tener la garantía que van a volver a ejecutarse.

## Con `then` y `catch`

```javascript
const promise = fetch('my-api.com/users');

// fulfillment handler original
promise.then((res) => {
  console.log(res); // datos recibidos
});

// agregamos otro handler a la misma promesa
promise.then((res) => {
  console.log(res); // datos recibidos
});
```
