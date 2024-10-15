# Guía de CSS Flexbox

## ¿Qué es Flexbox?

**Flexbox** (Flexible Box Layout) es un modelo de diseño en CSS que facilita la distribución y alineación de elementos dentro de un contenedor, incluso cuando el tamaño de los elementos es dinámico o desconocido. Con **Flexbox**, puedes crear interfaces adaptables y responsivas de manera sencilla, ya que te permite controlar cómo se distribuyen y alinean los elementos en el contenedor padre (contenedor flexible).

### Ventajas de Flexbox

- Facilita la alineación horizontal y vertical.
- Hace que el espacio entre elementos sea flexible y adaptable.
- Permite que los elementos cambien de tamaño y se reorganicen automáticamente según el espacio disponible.
- Es ideal para construir layouts responsivos sin necesidad de flotantes o posiciones absolutas.

---

## Conceptos clave en Flexbox

- **Contenedor flex**: Es el contenedor padre que define un contexto flex para sus hijos.
- **Ítems flex**: Son los elementos hijos dentro de un contenedor flex.

### Propiedades principales de Flexbox

1. **En el contenedor flex (padre):**
   - `display: flex;`: Define el contenedor como un contenedor flex.
   - `flex-direction`: Establece la dirección de los ítems (fila o columna).
   - `justify-content`: Alinea los ítems en el eje principal.
   - `align-items`: Alinea los ítems en el eje transversal.
   - `align-content`: Alinea las líneas flexibles en el contenedor cuando hay espacio extra.

2. **En los ítems flex (hijos):**
   - `flex-grow`: Establece cómo crecen los ítems para ocupar el espacio disponible.
   - `flex-shrink`: Controla cómo los ítems se reducen cuando no hay suficiente espacio.
   - `flex-basis`: Define el tamaño inicial de los ítems antes de que el espacio disponible sea distribuido.
   - `align-self`: Permite que un ítem se alinee de manera diferente a los demás ítems.

---

## Propiedades en el Contenedor Flex (padre)

### 1. **`display: flex;`**

El primer paso para usar Flexbox es aplicar `display: flex;` al contenedor. Esto convierte al contenedor en un "contenedor flex", haciendo que todos sus elementos hijos se conviertan en ítems flexibles.

```css
.container {
    display: flex;
}
```

### 2. **`flex-direction`**

Controla la dirección de los ítems dentro del contenedor (eje principal). Por defecto, la dirección es **horizontal** (`row`).

```css
.container {
    display: flex;
    flex-direction: row;  /* Alinea los ítems en fila (horizontal) */
}

/* Otras opciones */
.container-column {
    flex-direction: column; /* Alinea los ítems en columna (vertical) */
}
```

Valores posibles:

- `row`: Alinea los ítems en fila.
- `row-reverse`: Fila invertida (de derecha a izquierda).
- `column`: Alinea los ítems en columna (de arriba hacia abajo).
- `column-reverse`: Columna invertida (de abajo hacia arriba).

### 3. **`justify-content`**

Alinea los ítems a lo largo del eje principal (horizontal si `flex-direction: row`).

```css
.container {
    display: flex;
    justify-content: center; /* Alinea los ítems en el centro del contenedor */
}

/* Otras opciones */
.container-space-between {
    justify-content: space-between; /* Distribuye los ítems con el máximo espacio entre ellos */
}
```

Valores comunes:

- `flex-start`: Los ítems se alinean al principio del contenedor.
- `flex-end`: Los ítems se alinean al final del contenedor.
- `center`: Los ítems se alinean en el centro del contenedor.
- `space-between`: Deja el máximo espacio posible entre los ítems.
- `space-around`: Deja espacio alrededor de los ítems (incluyendo los bordes exteriores).
- `space-evenly`: Distribuye el espacio de manera equitativa entre los ítems.

### 4. **`align-items`**

Alinea los ítems a lo largo del eje transversal (vertical si `flex-direction: row`).

```css
.container {
    display: flex;
    align-items: center; /* Alinea verticalmente los ítems en el centro del contenedor */
}
```

Valores comunes:

- `flex-start`: Los ítems se alinean en la parte superior.
- `flex-end`: Los ítems se alinean en la parte inferior.
- `center`: Los ítems se alinean en el centro.
- `stretch`: Los ítems se estiran para llenar el espacio (valor predeterminado).
- `baseline`: Los ítems se alinean con la línea base de texto del primer ítem.

### 5. **`align-content`**

Alinea las líneas flexibles cuando hay espacio adicional en el eje transversal. Esto solo tiene efecto cuando los ítems se envuelven a múltiples líneas (cuando `flex-wrap` está habilitado).

```css
.container {
    display: flex;
    flex-wrap: wrap;  /* Permite que los ítems se envuelvan a múltiples líneas */
    align-content: center;  /* Alinea las líneas flexibles en el centro */
}
```

---

## Propiedades en los Ítems Flex (hijos)

### 1. **`flex-grow`**

Define cómo crece un ítem en relación a los otros ítems dentro del contenedor cuando hay espacio adicional.

```css
.item {
    flex-grow: 1;  /* Todos los ítems crecen equitativamente */
}
```

- `flex-grow: 0`: El ítem no crecerá más allá de su tamaño inicial.
- `flex-grow: 1`: El ítem puede crecer para llenar el espacio disponible (valor predeterminado).

### 2. **`flex-shrink`**

Controla cómo los ítems se reducen cuando no hay suficiente espacio en el contenedor.

```css
.item {
    flex-shrink: 1;  /* Los ítems pueden reducirse si es necesario */
}
```

- `flex-shrink: 0`: El ítem no se encogerá.
- `flex-shrink: 1`: El ítem se reducirá si es necesario (valor predeterminado).

### 3. **`flex-basis`**

Especifica el tamaño inicial de un ítem antes de que se distribuya el espacio adicional o antes de que se reduzca.

```css
.item {
    flex-basis: 200px;  /* El tamaño base del ítem será 200px */
}
```

### 4. **`align-self`**

Permite modificar el alineamiento de un ítem de manera individual en el eje transversal, ignorando la propiedad `align-items` del contenedor.

```css
.item {
    align-self: center;  /* Solo este ítem se alinea en el centro */
}
```

---

## Ejemplo Práctico con Flexbox

A continuación, un ejemplo completo de cómo usar Flexbox para crear un diseño simple y responsivo:

### HTML

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flexbox Ejemplo</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <div class="item">Item 1</div>
        <div class="item">Item 2</div>
        <div class="item">Item 3</div>
    </div>
</body>
</html>
```

### CSS (styles.css)

```css
/* Contenedor Flex */
.container {
    display: flex;
    justify-content: space-between;  /* Espacio máximo entre ítems */
    align-items: center;  /* Alinea ítems verticalmente en el centro */
    height: 100vh;  /* El contenedor ocupa toda la altura de la ventana */
    padding: 20px;
    background-color: #f0f0f0;
}

/* Ítems Flex */
.item {
    background-color: #4CAF50;
    color: white;
    padding: 20px;
    flex-basis: 30%;  /* Los ítems ocupan el 30% del ancho del contenedor */
    text-align: center;
    border-radius: 5px;
}
```

### Explicación

- El contenedor `.container` utiliza `display: flex`, lo que lo convierte en un contenedor flexible.
- Los ítems `.item` se distribuyen de manera equitativa en el espacio disponible gracias a `flex-basis: 30%` y `justify-content: space-between`.
- `align-items: center` alinea verticalmente los ítems en el centro del contenedor.

---

## Conclusión

**Flexbox** es una herramienta poderosa en CSS que te permite crear diseños flexibles y adaptables con un mínimo de código. Ya sea para distribuir elementos horizontal o verticalmente, alinear ítems dentro de un contenedor o hacer que los elementos cambien de tamaño en función del espacio disponible, Flexbox te facilita el desarrollo de layouts responsivos y bien organizados.

### Recursos adicionales

- [Guía completa de Flexbox en MDN](https://developer.mozilla.org/es/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox)
- [CSS Tricks - A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
