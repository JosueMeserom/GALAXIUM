# 🌌 GALAXIUM

Web para mi proyecto de 1º de ASIR.  
Aventura interplanetaria usando solamente **HTML/CSS** y un poquito de **JS**.

En principio todo lo que he usado para la confección de las páginas son métodos que aprendimos en clase, exceptuando alguna cosa mínima (como la carga de fuentes externas con `<link>` en el `<head>`, el tag para incrustar vídeos, o el uso de `<blockquote>`), el CSS de un planeta en concreto (me gustó lo que me ofreció una IA), y por otro lado los scripts.

---

En las páginas, solamente hay tres sistemas que involucran scripts:  
- El de **partida guardada**  
- El del **contador de tiempo transcurrido**  
- El de las **notas del explorador**  

También explicaré la página `reset.html`.

---

## 💾 Sistema de partida guardada

Se usa `localStorage` (el almacén que tiene el navegador para guardar datos persistentes de cada web) para un sistema de "partida guardada".  
Cada vez que se va a un enlace, se define el item `"ultimaPagina"` como el archivo HTML destino.

Después, en el `<head>` de casi todas las páginas, hay un script que verifica el item `"ultimaPagina"`:

- Si **existe**, el script, antes de cargar cualquier elemento de la página, redirige a la última página visitada, definida en ese item.  
  Con esto, si el usuario entra en una página (sea `index.html` o casi cualquier otra) escribiéndola directamente en la barra de direcciones del navegador, en lugar de entrar ahí, entra en la última que visitó, como si "cargase partida".  
  (El sistema de guardado también funciona como un sistema **anti-trampas**).

- Si `"ultimaPagina"` **no existe** porque nunca se ha definido, entonces se redirige a `index.html`  
  (esta verificación no existe en esa página, claro está), empezando una nueva partida de manera correcta, para que, de nuevo, no se puedan hacer trampas y se rompa todo el sistema.

---

## ⏱️ Contador de tiempo transcurrido

Casi todos los enlaces que te llevan a otra localización, exceptuando los que **no gastan tiempo** (como los que abren las notas, la página de reseteo, algunos movimientos, etc.) también **suman 1** al item `unidadesTiempo`, para mostrar el **contador de tiempo transcurrido** en la parte superior en casi todo momento.

Entonces, de nuevo al inicio de cada página y antes de cargar nada, se verifica si el contador es `7`, en cuyo caso:

- Se redirige a la **página de reinicio del sistema**, y desde allí,
- Vuelves al planeta inicial con el contador en `0`.

---

## 📓 Sistema de pistas del explorador

En las **notas del explorador**, al principio solamente hay una pista.  
Pero según se van descubriendo cosas al acceder a diferentes páginas, las pistas conseguidas se guardan en `localStorage`, con el siguiente comando en script:

```js
localStorage.setItem('nombre_pista', 'true');
```

Después, en `notas.html`, en el código están escritas todas las pistas en diferentes párrafos, pero con un estilo `display: none`:

```html
<p id="nombre_pista" style="display: none;">texto</p>
```

Así, por defecto, esos párrafos saldrán ocultos siempre. Pero, al final de la página, encontramos este script:

```js
var elementos = document.querySelectorAll('[id^="galaxium_pista_"]');
for (var i = 0; i < elementos.length; i++) {
	var elemento = elementos[i];
	var pistaKey = elemento.id; // El id ya incluye "galaxium_pista_"
	if (localStorage.getItem(pistaKey) === 'true') {
		elemento.style.display = 'block';
	}
}
```

Lo que hace es:

- Verificar todas las IDs que empiecen por `"galaxium_pista_"`.
- Por cada una, verifica si el item con exactamente el mismo nombre que esa ID está en `true`.
- Si es así, cambia el estilo `display` de ese ID a `block`, mostrando ese párrafo al renderizar la página.

⚠️ **Nota**: El script debe colocarse al final del todo, después de que se hayan cargado todos los párrafos de pistas (independientemente de si están "activadas" o no), porque si no, el script no podrá cambiar el estilo de algo que aún no existe.

---

Esta técnica de ocultar por defecto ciertos párrafos y luego activarlos según lo que ocurra se usa también en las páginas de **algún otro planeta**, ya que dependiendo de ciertas condiciones (por ejemplo, si usas otro medio de transporte que no sea la nave, o si el tiempo transcurrido es uno en concreto...) las descripciones cambian.

---

## ♻️ Página `reset.html`

`reset.html` lo que hace es **borrar todos los datos** de `localStorage` de esta página usando `localStorage.removeItem`, de manera que borre todos los que empiecen por `'galaxium_'` (que es el prefijo que uso para no interferir con otros items de otras páginas).

Después, te **devuelve a `index.html`**, para empezar de cero **sin pistas almacenadas ni páginas visitadas**.
