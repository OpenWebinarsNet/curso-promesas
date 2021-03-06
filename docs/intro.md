Promesas
========

Las promesas son objetos que se utilizan para conseguir asincronía en un lenguaje de progración síncrono. 

Una promesa representa un valor que puede estar disponible ahora, en el futuro, o nunca.


Sintaxis
--------

```javascript
var executor_function = function(resolve, reject) { 
    // Código asíncrono
    // Llamada a resolve(valor) o bien
    // llamada a reject("motivo del fallo");
};

var promesa = new Promise(executor_function);
```



Parámetros: 

El único parámetro de una promesa es la función ejecutora

- Función ejecutora
Una función con los argumentos resolver y rechazar. La función ejecutora es ejecutada inmediatamente por la implementación de la Promesa, pasándole las funciones resolver y rechazar (el ejecutor es llamado incluso antes de que el constructor de la Promesa devuelva el objeto creado). Las funciones resolver y rechazar, al ser llamadas, resuelven o rechazan la promesa, respectivamente. Normalmente la función ejecutor inicia un trabajo asíncrono, y luego, una vez que es completado, llama a la función resolver para resolver la promesa o la rechaza si ha ocurrido un error.
Si un error es lanzado en la función ejecutora, la promesa es rechazada y el valor de retorno del ejecutor es rechazado.

Volvamos a la definición: 

```javascript
var promesa = new Promise(function(resolve, reject) { 
    // Código asíncrono
    // Llamada a resolve(valor) o bien
    // llamada a reject("motivo del fallo");
});
```

Llamaremos a resolve(...) cuando lo que estemos haciendo de forma asíncrona se ejecute con éxito.

Aunque los ejemplos más usuales de código asíncrono son peticiones http, en el siguiente ejemplo usaremos la función setTimeout() de javascript para simular código asíncrono.

Ejemplo 1
---------

```javascript
let miPrimeraPromesa = new Promise((resolve, reject) => {
  setTimeout(function(){
    resolve("Success!"); 
  }, 2000);
});
```

Cuando pasen 2 segundos consideraremos que nuestro código asíncrono se ha ejecutado con éxito y entonces llamaremos a la función resolve(...) pasándole un parámetro con el valor que queramos.

La variable miPrimeraPromesa es una promesa y tiene un método then() a través del cual prodemos acceder al valor inyectado en la función resolve() o al error en caso de que hayamos llamado a reject() en vez de a resolve().

El método then() recibe como parámetro de entrada 2 funciones. En la primera función, obligatoria, se define el código que se ejecutará si la promesa terminó con resolve().

```javascript
miPrimeraPromesa.then(function(data) {
  console.log(data);
});
```

La segunda función es opcional y define el código que se ejecutará si la promesa terminó con reject().

```javascript
miPrimeraPromesa.then(funcion1, funcion2);
```

Aunque también existe la función catch para reescribir el código anterior de esta forma

```javascript
miPrimeraPromesa.then(funcion1).catch(funcion2);
```

Si la promesa acaba llamando a resolve() se ejecutará la función del then() y si acaba llamando a reject() se ejecutará la función del catch().


Ejemplo 2
---------

El siguiente ejemplo lanza una promesa cada vez que se apreta el botón.

```html
<button id="btn">Haz click para generar una promesa</button>
<div id="log"></div>
```

Cada promesa tarda en resolverse un tiempo al azar entre 1 y 3 segundos. Con lo cual, es posible que promesas generadas después, acaben antes que otras.


```javascript
'use strict';
var promiseCount = 0;

function testPromise() {
    let thisPromiseCount = ++promiseCount;

    let log = document.getElementById('log');
    log.insertAdjacentHTML('beforeend', thisPromiseCount +
        ') Code Started (<small>Empieza el código síncrono</small>)<br/>');

    let p1 = new Promise(

       (resolve, reject) => {
            log.insertAdjacentHTML('beforeend', thisPromiseCount +
                ') Promise started (<small>Empieza el código asíncrono</small>)<br/>');

            window.setTimeout(
                function() {
                    // We fulfill the promise !
                    resolve(thisPromiseCount);
                }, Math.random() * 4000 + 1000);
        }
    );

    p1.then(
        // Log the fulfillment value
        function(val) {
            log.insertAdjacentHTML('beforeend', val +
                ') Promise fulfilled (<small>Código asíncrono terminado</small>)<br/>');
        })
    .catch(
        // Log the rejection reason
       (reason) => {
            console.log('Handle rejected promise ('+reason+') here.');
        });

    log.insertAdjacentHTML('beforeend', thisPromiseCount +
        ') Promise hecha (<small>Código síncrono terminado</small>)<br/>');
}

if ("Promise" in window) {
  let btn = document.getElementById("btn");
  btn.addEventListener("click",testPromise);
} else {
  log = document.getElementById('log');
  log.innerHTML = "Tu navegador no soporta Promesas.";
}
```





Una Promesa (Promise) es un objeto que representa la terminación con éxito o el fracaso eventual de una operación asíncrona. 

Una promesa puede ser creada usando su constructor. Sin embargo, la mayoría de la gente son consumidores de promesas ya creadas devueltas desde funciones. Esta guía explorará por lo tanto la consumición (el uso) de promesas devueltas primero.

Esencialmente, una promesa es un objeto devuelto al cual enganchas las funciones callback, en vez de pasar funciones callback a una función.

Por ejemplo, en vez de una función del viejo estilo que espera dos funciones callback, y llama a una de ellas en caso de terminación o fallo:











Compatibilidad en navegadores
-----------------------------

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise (al final de la página)