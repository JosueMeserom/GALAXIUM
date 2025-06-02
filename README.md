# GALAXIUM
Web para mi proyecto de 1º de ASIR. Aventura interplanetaria usando solamente HTML/CSS y un poquito de JS.

En principio todo lo que he usado para la confección de las páginas son métodos que aprendimos en clase, exceptuando alguna cosa mínima (como la carga de fuentes externas con <link> en el head, el tag para incrustar vídeos, o el uso de <blockquote>), el CSS de un planeta en concreto (me gustó lo que me ofreció una IA), y por otro lado los scripts.

En las páginas, solamente hay tres sistemas que involucran scripts: el de partida guardada, el del contador de tiempo transcurrido, y el de las notas del explorador. Explicaré también la página reset.html.

- Se usa localStorage (el almacén que tiene el navegador para guardar datos persistentes de cada web) para un sistema de "partida guardada". Cada vez que se va a un enlace, se define el item "ultimaPagina" como el archivo html destino. Después, en el head de casi todas las páginas, hay un script que verifica el item "ultimaPagina":

Si existe, el script, antes de cargar cualquier elemento de la página, lo que hace es redirigir a la última página visitada, definida en ese item.  Con esto, si el usuario entra en una página (sea index.html o casi cualquier otra) escribiéndola directamente en la barra de direcciones del navegador, en lugar de entrar ahí, pues entra en la última que visitó, como si "cargase partida" (el sistema de guardado también funciona como un sistema anti-trampas).

Si ultimaPagina no existe porque nunca se ha definido, entonces se redirige a index.html (esta verificación no existe en esa página, claro está) empezando una nueva partida de manera correcta, para que, de nuevo, no se puedan hacer trampas y se rompa todo el sistema.

- Casi todos los enlaces que te llevan a otra localización, exceptuando los que no gastan tiempo -como los que abren las notas, la página de reseteo, algunos movimientos, etc.- también suman 1 al item unidadesTiempo, para mostrar el contador de tiempo transcurrido en la parte superior en casi todo momento. 

Entonces, de nuevo al inicio de cada página y antes de cargar nada, se verifica si el contador es 7, en cuyo caso, se te redirige a la página de reinicio del sistema, y desde allí, vuelves al planeta inicial con el contador en 0.

- En las notas del explorador, al principio solamente hay una pista. Pero según se van descubriendo cosas al acceder a diferentes páginas, las pistas conseguidas se guardan en localStorage, con el siguiente comando en script:

localStorage.setItem('nombre_pista', 'true');
