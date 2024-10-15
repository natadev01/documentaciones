# Guía de Neo4j con Node.js

## ¿Qué es Neo4j?

**Neo4j** es una base de datos NoSQL orientada a grafos, diseñada específicamente para modelar relaciones complejas entre los datos. A diferencia de las bases de datos relacionales o de documentos, Neo4j almacena los datos en forma de **nodos** (entidades) y **relaciones** entre esos nodos, lo que facilita la representación y consulta de datos altamente interconectados.

Algunos casos de uso comunes de Neo4j incluyen:

- **Redes sociales** (seguidores, amigos, conexiones).
- **Recomendación de productos** basados en las interacciones de los usuarios.
- **Análisis de redes** (por ejemplo, fraude financiero).
- **Gestión de dependencias** y estructuras jerárquicas.

---

## ¿Qué es Cypher?

**Cypher** es el lenguaje de consulta utilizado por Neo4j. Permite realizar consultas de manera declarativa, similar a cómo SQL se utiliza en bases de datos relacionales, pero optimizado para explorar grafos y relaciones.

---

## Instalación de Neo4j

### Paso 1: Instalar Neo4j

Primero, instala Neo4j en tu máquina desde la [página oficial de Neo4j](https://neo4j.com/download/). Puedes elegir entre la versión **Neo4j Desktop** o **Neo4j Community Edition** para instalarla localmente.

Una vez instalado, inicia el servidor Neo4j y accede a la interfaz de administración en tu navegador (normalmente en `http://localhost:7474`).

---

## Conexión a Neo4j con Node.js

Para interactuar con Neo4j desde **Node.js**, utilizaremos el paquete oficial `neo4j-driver`.

### Paso 2: Crear un proyecto Node.js

Crea un proyecto Node.js si aún no lo tienes:

```bash
mkdir neo4j-project
cd neo4j-project
npm init -y
```

### Paso 3: Instalar el driver de Neo4j para Node.js

Instala la biblioteca oficial `neo4j-driver`:

```bash
npm install neo4j-driver
```

### Paso 4: Conectar a la base de datos Neo4j

Una vez que hayas iniciado Neo4j y tengas el driver instalado, puedes conectarte a la base de datos utilizando Node.js de la siguiente manera:

```javascript
const neo4j = require('neo4j-driver');

// Crear un driver para conectarse a la base de datos
const driver = neo4j.driver('bolt://localhost:7687', neo4j.auth.basic('neo4j', 'tu_password'));

// Crear una sesión para interactuar con la base de datos
const session = driver.session();

// Realizar una consulta de ejemplo
async function crearNodo() {
  try {
    const result = await session.run(
      'CREATE (n:Persona {nombre: $nombre, edad: $edad}) RETURN n',
      { nombre: 'Juan', edad: 30 }
    );
    console.log('Nodo creado:', result.records[0].get('n'));
  } catch (err) {
    console.error('Error al crear nodo:', err);
  } finally {
    await session.close();
  }
}

// Llamar a la función para crear el nodo
crearNodo().then(() => driver.close());
```

En este ejemplo, creamos una conexión con Neo4j y agregamos un nodo etiquetado como `Persona` con dos propiedades: `nombre` y `edad`.

---

## Operaciones CRUD en Neo4j con Node.js

### Paso 5: Crear nodos y relaciones

#### Crear un nodo

Ya vimos cómo crear un nodo en el ejemplo anterior. Aquí mostramos otro ejemplo para crear múltiples nodos y establecer relaciones entre ellos.

```javascript
async function crearRelacion() {
  try {
    const result = await session.run(
      `
      CREATE (a:Persona {nombre: $nombreA, edad: $edadA})
      CREATE (b:Persona {nombre: $nombreB, edad: $edadB})
      CREATE (a)-[:AMIGO]->(b)
      RETURN a, b
      `,
      {
        nombreA: 'Carlos', edadA: 25,
        nombreB: 'Ana', edadB: 28
      }
    );
    console.log('Relación creada:', result.records[0].get('a'), 'es amigo de', result.records[0].get('b'));
  } catch (err) {
    console.error('Error al crear relación:', err);
  } finally {
    await session.close();
  }
}

crearRelacion().then(() => driver.close());
```

En este ejemplo, creamos dos nodos `Persona` y establecemos una relación de **AMIGO** entre ellos.

#### Consultar nodos y relaciones

Para consultar los nodos y sus relaciones, utilizamos el comando **MATCH** en Cypher.

```javascript
async function consultarNodos() {
  try {
    const result = await session.run(
      'MATCH (p:Persona)-[:AMIGO]->(amigo) RETURN p, amigo'
    );
    result.records.forEach(record => {
      console.log('Persona:', record.get('p').properties, 'es amigo de', record.get('amigo').properties);
    });
  } catch (err) {
    console.error('Error al consultar nodos:', err);
  } finally {
    await session.close();
  }
}

consultarNodos().then(() => driver.close());
```

Este código consulta todas las personas y sus amigos, devolviendo la información de los nodos conectados por la relación `AMIGO`.

#### Actualizar nodos

Para actualizar un nodo, utilizamos el comando **SET**.

```javascript
async function actualizarNodo() {
  try {
    const result = await session.run(
      'MATCH (p:Persona {nombre: $nombre}) SET p.edad = $nuevaEdad RETURN p',
      { nombre: 'Juan', nuevaEdad: 31 }
    );
    console.log('Nodo actualizado:', result.records[0].get('p'));
  } catch (err) {
    console.error('Error al actualizar nodo:', err);
  } finally {
    await session.close();
  }
}

actualizarNodo().then(() => driver.close());
```

Este ejemplo actualiza la edad de `Juan` a 31 años.

#### Eliminar nodos y relaciones

Para eliminar un nodo y sus relaciones asociadas, utilizamos el comando **DELETE**.

```javascript
async function eliminarNodo() {
  try {
    const result = await session.run(
      'MATCH (p:Persona {nombre: $nombre}) DETACH DELETE p',
      { nombre: 'Juan' }
    );
    console.log('Nodo eliminado');
  } catch (err) {
    console.error('Error al eliminar nodo:', err);
  } finally {
    await session.close();
  }
}

eliminarNodo().then(() => driver.close());
```

Este código elimina el nodo con el nombre `Juan` y todas las relaciones asociadas a él.

---

## Uso avanzado de Neo4j

### Relaciones más complejas

Neo4j es particularmente útil cuando deseas modelar relaciones complejas entre tus datos. Por ejemplo, en una red social, podrías querer encontrar todas las conexiones de segundo grado (amigos de amigos) utilizando consultas como:

```javascript
async function amigosDeAmigos() {
  try {
    const result = await session.run(
      'MATCH (p:Persona {nombre: $nombre})-[:AMIGO]->(amigo)-[:AMIGO]->(amigoDeAmigo) RETURN amigoDeAmigo',
      { nombre: 'Carlos' }
    );
    result.records.forEach(record => {
      console.log('Amigo de un amigo:', record.get('amigoDeAmigo').properties);
    });
  } catch (err) {
    console.error('Error al consultar amigos de amigos:', err);
  } finally {
    await session.close();
  }
}

amigosDeAmigos().then(() => driver.close());
```

Este ejemplo encuentra todos los amigos de los amigos de `Carlos`.

### Caminos más cortos

Neo4j también puede calcular rutas o caminos más cortos entre dos nodos:

```javascript
async function caminoMasCorto() {
  try {
    const result = await session.run(
      `
      MATCH (p1:Persona {nombre: $nombre1}),
            (p2:Persona {nombre: $nombre2}),
            path = shortestPath((p1)-[*]-(p2))
      RETURN path
      `,
      { nombre1: 'Carlos', nombre2: 'Ana' }
    );
    console.log('Camino más corto:', result.records[0].get('path'));
  } catch (err) {
    console.error('Error al calcular el camino más corto:', err);
  } finally {
    await session.close();
  }
}

caminoMasCorto().then(() => driver.close());
```

---

## Conclusión

**Neo4j** es una base de datos muy poderosa para modelar y consultar datos con relaciones complejas, ideal para aplicaciones como redes sociales, motores de recomendación, y análisis de grafos. Usar **Node.js** junto con **Neo4j** y el lenguaje **Cypher** te permite explorar de manera eficiente las relaciones entre los datos.

### Documentación oficial

Para obtener más información sobre Neo4j y cómo aprovechar al máximo su potencial, puedes consultar la documentación oficial:

- [Neo4j Documentation](https://neo4j.com/docs/)
- [Cypher Query Language](https://neo4j.com/developer/cypher/)
