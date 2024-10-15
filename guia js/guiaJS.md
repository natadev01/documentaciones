
# Aprendiendo JavaScript: De Cero a Experto

## Índice

- [Aprendiendo JavaScript: De Cero a Experto](#aprendiendo-javascript-de-cero-a-experto)
  - [Índice](#índice)
  - [1. Introducción a JavaScript](#1-introducción-a-javascript)
    - [Primer Programa: `Hello World`](#primer-programa-hello-world)
  - [2. Sintaxis Básica](#2-sintaxis-básica)
  - [3. Tipos de Datos](#3-tipos-de-datos)
  - [4. Operadores](#4-operadores)
    - [Operadores Aritméticos](#operadores-aritméticos)
    - [Operadores de Comparación](#operadores-de-comparación)
  - [5. Control de Flujo](#5-control-de-flujo)
    - [Condicionales: `if`, `else if`, `else`](#condicionales-if-else-if-else)
    - [Bucles: `for`, `while`, `do-while`](#bucles-for-while-do-while)
  - [6. Funciones](#6-funciones)
    - [Funciones Declarativas](#funciones-declarativas)
    - [Funciones Expresivas (Funciones Flecha)](#funciones-expresivas-funciones-flecha)
  - [7. Ámbito y Closures](#7-ámbito-y-closures)
    - [Ámbito Local y Global](#ámbito-local-y-global)
    - [Closures](#closures)
  - [8. Programación Orientada a Objetos (POO)](#8-programación-orientada-a-objetos-poo)
    - [Clases y Objetos](#clases-y-objetos)
    - [Herencia](#herencia)
  - [9. Arrays y Métodos](#9-arrays-y-métodos)
    - [Declaración de Arrays](#declaración-de-arrays)
    - [Métodos de Arrays](#métodos-de-arrays)
  - [10. Manejo de Errores](#10-manejo-de-errores)
    - [`try...catch`](#trycatch)
  - [11. Programación Funcional](#11-programación-funcional)
    - [Métodos `map`, `filter`, `reduce`](#métodos-map-filter-reduce)
  - [12. Asincronía y Promesas](#12-asincronía-y-promesas)
    - [Promesas](#promesas)
  - [13. Async/Await](#13-asyncawait)
  - [14. Manipulación del DOM](#14-manipulación-del-dom)
    - [Selección de Elementos](#selección-de-elementos)
    - [Modificación de Elementos](#modificación-de-elementos)
  - [15. Módulos en JavaScript](#15-módulos-en-javascript)
    - [Exportar e Importar Módulos](#exportar-e-importar-módulos)
      - [Archivo `matematicas.js`](#archivo-matematicasjs)
      - [Archivo `app.js`](#archivo-appjs)
  - [16. JavaScript Moderno: ES6+](#16-javascript-moderno-es6)

## 1. Introducción a JavaScript

JavaScript es un lenguaje de programación que permite crear contenido dinámico en las páginas web. Fue creado inicialmente para mejorar la interacción en el navegador, pero actualmente es usado tanto en el frontend como en el backend.

### Primer Programa: `Hello World`

```javascript
console.log("Hello World");
```

## 2. Sintaxis Básica

- Comentarios:
  
  ```javascript
  // Esto es un comentario
  /* Esto es un comentario de varias líneas */
  ```

- Variables:
  
  ```javascript
  var nombre = "Natalia";
  let edad = 25;
  const PI = 3.1416;
  ```

## 3. Tipos de Datos

- Primitivos: `string`, `number`, `boolean`, `undefined`, `null`, `symbol`, `bigint`
- Ejemplo:

  ```javascript
  let nombre = "John";  // string
  let edad = 30;  // number
  let esActivo = true;  // boolean
  ```

## 4. Operadores

### Operadores Aritméticos

```javascript
let suma = 5 + 3;
let multiplicacion = 2 * 4;
```

### Operadores de Comparación

```javascript
console.log(5 == "5");  // true
console.log(5 === "5");  // false
```

## 5. Control de Flujo

### Condicionales: `if`, `else if`, `else`

```javascript
let numero = 10;
if (numero > 5) {
  console.log("Es mayor que 5");
} else {
  console.log("Es menor o igual a 5");
}
```

### Bucles: `for`, `while`, `do-while`

```javascript
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

## 6. Funciones

### Funciones Declarativas

```javascript
function saludar() {
  console.log("Hola");
}
saludar();
```

### Funciones Expresivas (Funciones Flecha)

```javascript
const sumar = (a, b) => a + b;
console.log(sumar(2, 3));
```

## 7. Ámbito y Closures

### Ámbito Local y Global

```javascript
let globalVar = "Soy global";

function ejemplo() {
  let localVar = "Soy local";
  console.log(globalVar);
}
```

### Closures

```javascript
function hacerSuma(a) {
  return function(b) {
    return a + b;
  }
}
const suma5 = hacerSuma(5);
console.log(suma5(3));  // 8
```

## 8. Programación Orientada a Objetos (POO)

### Clases y Objetos

```javascript
class Persona {
  constructor(nombre, edad) {
    this.nombre = nombre;
    this.edad = edad;
  }
  
  saludar() {
    console.log(`Hola, soy ${this.nombre}`);
  }
}

const persona1 = new Persona("Natalia", 30);
persona1.saludar();
```

### Herencia

```javascript
class Estudiante extends Persona {
  constructor(nombre, edad, carrera) {
    super(nombre, edad);
    this.carrera = carrera;
  }

  estudiar() {
    console.log(`${this.nombre} está estudiando ${this.carrera}`);
  }
}
const estudiante1 = new Estudiante("Carlos", 22, "Ingeniería");
estudiante1.saludar();
estudiante1.estudiar();
```

## 9. Arrays y Métodos

### Declaración de Arrays

```javascript
let frutas = ["Manzana", "Banana", "Naranja"];
```

### Métodos de Arrays

```javascript
frutas.push("Pera");  // Agrega al final
frutas.pop();  // Elimina el último
```

## 10. Manejo de Errores

### `try...catch`

```javascript
try {
  let resultado = 10 / 0;
  console.log(resultado);
} catch (error) {
  console.log("Error:", error.message);
}
```

## 11. Programación Funcional

### Métodos `map`, `filter`, `reduce`

```javascript
let numeros = [1, 2, 3, 4];
let dobles = numeros.map(num => num * 2);
let pares = numeros.filter(num => num % 2 === 0);
let sumaTotal = numeros.reduce((acc, num) => acc + num, 0);
```

## 12. Asincronía y Promesas

### Promesas

```javascript
const promesa = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Datos recibidos");
  }, 2000);
});

promesa.then(datos => console.log(datos));
```

## 13. Async/Await

```javascript
async function obtenerDatos() {
  try {
    let resultado = await promesa;
    console.log(resultado);
  } catch (error) {
    console.log("Error:", error.message);
  }
}
obtenerDatos();
```

## 14. Manipulación del DOM

### Selección de Elementos

```javascript
let elemento = document.getElementById("miElemento");
let clases = document.querySelectorAll(".clase");
```

### Modificación de Elementos

```javascript
elemento.textContent = "Nuevo texto";
elemento.style.color = "red";
```

## 15. Módulos en JavaScript

### Exportar e Importar Módulos

#### Archivo `matematicas.js`

```javascript
export function sumar(a, b) {
  return a + b;
}
```

#### Archivo `app.js`

```javascript
import { sumar } from './matematicas.js';
console.log(sumar(5, 3));
```

## 16. JavaScript Moderno: ES6+

- Desestructuración
  
  ```javascript
  let persona = { nombre: "John", edad: 30 };
  let { nombre, edad } = persona;
  ```

- Spread Operator
  
  ```javascript
  let arr1 = [1, 2, 3];
  let arr2 = [...arr1, 4, 5];
  ```
