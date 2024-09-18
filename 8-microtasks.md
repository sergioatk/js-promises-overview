# Microtask

Todos los handlers de promesas son ejecutados como `microtask`.

Se encolan y se ejecutan inmediatamente luego de que todo el código síncrono se haya completado.

El tema es complejo pero lo importante es saber que este enfoque minimiza el tiempo entre que una promesa se resuelve, y en el que el código reacciona a la misma y ejecuta el handler, haciendo de esta manera que las promesas sean buenas candidatas en situaciones donde la performance sea importante.
