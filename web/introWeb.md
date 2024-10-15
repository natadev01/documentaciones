# Métodos para el Manejo del DOM con HTML, CSS y JavaScript

## ¿Qué es el DOM?

El **DOM** (Document Object Model) es una representación estructurada de un documento HTML o XML. A través del DOM, JavaScript puede acceder y manipular el contenido, la estructura y el estilo de una página web. El DOM convierte el contenido HTML en objetos que JavaScript puede modificar de manera dinámica.

## Conceptos Clave

- **Nodos**: Cada elemento HTML es un nodo dentro del DOM.
- **Elementos**: Los elementos son nodos específicos que representan etiquetas HTML.
- **Propiedades del DOM**: A través de JavaScript, puedes acceder y modificar las propiedades de los nodos, como su contenido, atributos, clases, etc.

### Estructura básica de un documento HTML

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Manipulación del DOM</title>
</head>
<body>
    <h1 id="titulo">¡Hola Mundo!</h1>
    <p class="descripcion">Este es un párrafo de ejemplo.</p>
    <button id="cambiarTexto">Cambiar Texto</button>

    <script src="script.js"></script>
</body>
</html>
```

En este ejemplo de HTML, se encuentran varios elementos que pueden ser manipulados con JavaScript a través del DOM, como el título (`<h1>`), el párrafo (`<p>`) y el botón (`<button>`).

---

## Métodos básicos para manipular el DOM con JavaScript

### 1. **Acceder a los elementos del DOM**

Para acceder a un elemento del DOM, puedes usar los siguientes métodos:

#### **`getElementById()`**

Selecciona un elemento a través de su `id`.

```javascript
const titulo = document.getElementById('titulo');
console.log(titulo);  // <h1 id="titulo">¡Hola Mundo!</h1>
```

#### **`getElementsByClassName()`**

Selecciona todos los elementos que comparten la misma clase.

```javascript
const descripcion = document.getElementsByClassName('descripcion');
console.log(descripcion);  // HTMLCollection de todos los elementos con la clase "descripcion"
```

#### **`getElementsByTagName()`**

Selecciona todos los elementos que comparten la misma etiqueta.

```javascript
const parrafos = document.getElementsByTagName('p');
console.log(parrafos);  // HTMLCollection de todos los <p> en el documento
```

#### **`querySelector()`**

Selecciona el **primer** elemento que coincide con un selector CSS. Este método es más flexible y comúnmente usado.

```javascript
const titulo = document.querySelector('#titulo');  // Usando un selector CSS
console.log(titulo);
```

#### **`querySelectorAll()`**

Selecciona **todos** los elementos que coinciden con un selector CSS, devolviendo una `NodeList`.

```javascript
const descripciones = document.querySelectorAll('.descripcion');
console.log(descripciones);  // NodeList de todos los elementos con la clase "descripcion"
```

---

### 2. **Modificar el contenido de un elemento**

Una vez que has accedido a un elemento, puedes modificar su contenido con las siguientes propiedades:

#### **`textContent`**

Cambia el contenido textual de un elemento, sin afectar el HTML dentro de él.

```javascript
const titulo = document.getElementById('titulo');
titulo.textContent = '¡Hola desde JavaScript!';  // Cambia el texto dentro del <h1>
```

#### **`innerHTML`**

Modifica el contenido HTML dentro de un elemento.

```javascript
const descripcion = document.querySelector('.descripcion');
descripcion.innerHTML = '<strong>Este es un párrafo con texto en negrita.</strong>';
```

---

### 3. **Modificar atributos y clases**

#### **`setAttribute()` y `getAttribute()`**

Estos métodos permiten establecer y obtener atributos de un elemento.

```javascript
const boton = document.getElementById('cambiarTexto');

// Cambia el valor de un atributo
boton.setAttribute('disabled', 'true');  // Deshabilita el botón

// Obtiene el valor de un atributo
const idBoton = boton.getAttribute('id');
console.log(idBoton);  // "cambiarTexto"
```

#### **`classList`**

Para manejar clases de los elementos, puedes usar la propiedad `classList` con los métodos `add()`, `remove()`, `toggle()`, y `contains()`.

```javascript
const descripcion = document.querySelector('.descripcion');

// Agregar una clase
descripcion.classList.add('resaltado');

// Eliminar una clase
descripcion.classList.remove('descripcion');

// Alternar una clase (añadirla si no existe, eliminarla si ya existe)
descripcion.classList.toggle('oculto');

// Comprobar si un elemento tiene una clase
console.log(descripcion.classList.contains('resaltado'));  // true o false
```

---

### 4. **Crear y eliminar elementos**

#### **`createElement()`**

Crea un nuevo elemento HTML que puedes agregar al DOM.

```javascript
const nuevoParrafo = document.createElement('p');
nuevoParrafo.textContent = 'Este es un nuevo párrafo agregado dinámicamente.';
document.body.appendChild(nuevoParrafo);
```

#### **`remove()`**

Elimina un elemento del DOM.

```javascript
const titulo = document.getElementById('titulo');
titulo.remove();  // Elimina el elemento <h1>
```

---

### 5. **Eventos en el DOM**

Los **eventos** permiten que JavaScript reaccione a las interacciones del usuario, como clics, teclas pulsadas o movimientos del ratón.

#### **`addEventListener()`**

Este método agrega un evento a un elemento. Se puede usar para ejecutar una función cuando ocurre un evento, como un clic en un botón.

```javascript
const boton = document.getElementById('cambiarTexto');
boton.addEventListener('click', () => {
  const titulo = document.getElementById('titulo');
  titulo.textContent = 'El texto ha cambiado después de hacer clic';
});
```

---

## Ejemplo práctico: Manejo del DOM con interacción de usuario

Supongamos que queremos cambiar el texto de un título cuando el usuario hace clic en un botón. El código completo se vería así:

### HTML

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ejemplo de Manipulación del DOM</title>
</head>
<body>
    <h1 id="titulo">Texto original</h1>
    <p class="descripcion">Este es un párrafo de ejemplo.</p>
    <button id="cambiarTexto">Cambiar Texto</button>

    <script src="script.js"></script>
</body>
</html>
```

### JavaScript (script.js)

```javascript
const boton = document.getElementById('cambiarTexto');
boton.addEventListener('click', () => {
  const titulo = document.getElementById('titulo');
  titulo.textContent = 'El texto ha sido cambiado dinámicamente.';
});
```

### CSS (Opcional, para estilo)

```css
.resaltado {
  background-color: yellow;
}

.oculto {
  display: none;
}
```

---

## Conclusión

El manejo del DOM es una habilidad fundamental para cualquier desarrollador web, ya que te permite crear interfaces interactivas y dinámicas. Con los métodos que hemos cubierto en esta guía, puedes acceder a elementos, modificar su contenido y estructura, agregar eventos y crear interactividad con JavaScript, todo mientras manejas la presentación y estilo con HTML y CSS.
