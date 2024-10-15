# Guía de Redis con Node.js

## ¿Qué es Redis?

**Redis** (Remote Dictionary Server) es una base de datos NoSQL en memoria, ampliamente utilizada como **caché**, **cola de mensajes** o incluso como **almacenamiento de datos clave-valor**. Es extremadamente rápido, ya que la mayoría de sus operaciones ocurren en la memoria RAM en lugar de en el disco. Redis soporta estructuras de datos como listas, conjuntos, tablas hash, y cadenas.

Redis es muy útil en aplicaciones web para:

- **Almacenamiento temporal de datos** (caché).
- **Gestión de sesiones** en sistemas distribuidos.
- **Colas de mensajes** o tareas en sistemas de procesamiento en tiempo real.

---

## Instalación de Redis

### Paso 1: Instalar Redis en tu máquina local

Sigue las instrucciones de instalación según tu sistema operativo en la [página oficial de Redis](https://redis.io/download).

Una vez que Redis esté instalado, puedes ejecutarlo utilizando el comando:

```bash
redis-server
```

Esto iniciará el servidor Redis en tu máquina.

---

## Uso básico de Redis con Node.js

En esta sección aprenderás cómo interactuar con Redis desde una aplicación **Node.js** utilizando la librería `redis`.

### Paso 2: Instalar la librería Redis para Node.js

Primero, debes crear un proyecto Node.js (si aún no lo tienes):

```bash
mkdir redis-project
cd redis-project
npm init -y
```

Luego, instala la librería **redis**:

```bash
npm install redis
```

### Paso 3: Conectar a Redis

Para conectarte a Redis desde tu aplicación Node.js, puedes hacer lo siguiente:

```javascript
const redis = require('redis');

// Crear un cliente Redis
const client = redis.createClient({
  host: '127.0.0.1',
  port: 6379
});

// Manejar errores
client.on('error', (err) => {
  console.error('Error al conectar a Redis:', err);
});

// Conectar
client.connect()
  .then(() => console.log('Conectado a Redis'))
  .catch(err => console.error('Error al conectar a Redis:', err));
```

En este ejemplo, estamos creando un cliente que se conecta al servidor Redis en `localhost` y el puerto predeterminado `6379`.

### Paso 4: Operaciones básicas de Redis

Una vez que estás conectado a Redis, puedes realizar operaciones clave-valor básicas como **SET** y **GET**.

#### Establecer un valor (SET)

```javascript
client.set('nombre', 'Juan', (err, reply) => {
  if (err) throw err;
  console.log('Respuesta SET:', reply);
});
```

#### Obtener un valor (GET)

```javascript
client.get('nombre', (err, reply) => {
  if (err) throw err;
  console.log('Valor GET:', reply);
});
```

#### Almacenamiento con expiración (SETEX)

Puedes establecer un valor con una fecha de expiración, útil para manejar caché temporal:

```javascript
client.setEx('usuario_sesion', 3600, 'Juan Perez', (err, reply) => {
  if (err) throw err;
  console.log('Respuesta SETEX:', reply);  // Expira en 3600 segundos (1 hora)
});
```

#### Eliminar una clave (DEL)

Si deseas eliminar un valor almacenado en Redis:

```javascript
client.del('nombre', (err, reply) => {
  if (err) throw err;
  console.log('Respuesta DEL:', reply);
});
```

### Paso 5: Trabajar con estructuras de datos en Redis

Además de las operaciones básicas de clave-valor, Redis ofrece soporte para estructuras de datos más complejas como listas, conjuntos y tablas hash.

#### Listas (LPUSH y LRANGE)

**LPUSH** inserta un valor en el inicio de una lista:

```javascript
client.lPush('mis_lista', 'valor1', 'valor2', (err, reply) => {
  if (err) throw err;
  console.log('Respuesta LPUSH:', reply);  // Agrega elementos a la lista
});
```

**LRANGE** obtiene los elementos de una lista en un rango determinado:

```javascript
client.lRange('mis_lista', 0, -1, (err, reply) => {
  if (err) throw err;
  console.log('Lista LRANGE:', reply);  // Obtiene todos los elementos de la lista
});
```

#### Hashes (HSET y HGETALL)

**HSET** permite almacenar datos en formato hash (clave-valor dentro de una clave):

```javascript
client.hSet('mi_hash', 'campo1', 'valor1', 'campo2', 'valor2', (err, reply) => {
  if (err) throw err;
  console.log('Respuesta HSET:', reply);
});
```

**HGETALL** obtiene todos los campos y valores de un hash:

```javascript
client.hGetAll('mi_hash', (err, reply) => {
  if (err) throw err;
  console.log('Hash HGETALL:', reply);
});
```

#### Conjuntos (SADD y SMEMBERS)

**SADD** permite agregar miembros a un conjunto:

```javascript
client.sAdd('mi_conjunto', 'miembro1', 'miembro2', (err, reply) => {
  if (err) throw err;
  console.log('Respuesta SADD:', reply);  // Añade miembros al conjunto
});
```

**SMEMBERS** obtiene todos los miembros de un conjunto:

```javascript
client.sMembers('mi_conjunto', (err, reply) => {
  if (err) throw err;
  console.log('Miembros SMEMBERS:', reply);
});
```

---

## Uso de Redis como caché

Uno de los usos más comunes de Redis es como sistema de **caché** para mejorar el rendimiento de aplicaciones web. Vamos a ver un ejemplo básico de cómo utilizar Redis para almacenar en caché una respuesta de una API.

### Paso 6: Crear un caché con Redis

Supongamos que queremos cachear la respuesta de una consulta de usuarios en nuestra API:

```javascript
const axios = require('axios');

// Función para obtener usuarios (simula una API externa)
async function obtenerUsuarios(req, res) {
  const cacheKey = 'usuarios'; // Clave para almacenar en caché

  // Primero intenta obtener los datos de Redis
  const cacheData = await client.get(cacheKey);
  
  if (cacheData) {
    console.log('Datos obtenidos de caché');
    return res.send(JSON.parse(cacheData));  // Devuelve la respuesta en caché
  }

  // Si no hay datos en caché, realiza la consulta
  const response = await axios.get('https://jsonplaceholder.typicode.com/users');
  const users = response.data;

  // Almacena los datos en Redis durante 1 hora
  await client.setEx(cacheKey, 3600, JSON.stringify(users));

  console.log('Datos obtenidos de la API');
  res.send(users);
}
```

### Paso 7: Integrar el caché en tu API

Este código utiliza Redis para cachear la respuesta de una API externa, ahorrando tiempo y recursos si la misma consulta se hace varias veces.

---

## Conclusión

**Redis** es una base de datos NoSQL extremadamente rápida y flexible que se puede utilizar para muchos propósitos, como caché, cola de mensajes o incluso almacenamiento temporal de datos. Junto con **Node.js**, Redis se convierte en una herramienta poderosa para mejorar el rendimiento y la escalabilidad de las aplicaciones.

### Documentación oficial

Para más detalles sobre el uso de Redis, puedes consultar la documentación oficial:

- [Redis Documentation](https://redis.io/documentation)
- [Redis para Node.js (npm redis)](https://github.com/redis/node-redis)
