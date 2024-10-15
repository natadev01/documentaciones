# Guía de MongoDB

## ¿Qué es MongoDB?

**MongoDB** es una base de datos **NoSQL** orientada a documentos, diseñada para manejar grandes cantidades de datos sin necesidad de esquemas rígidos. A diferencia de las bases de datos relacionales que almacenan datos en tablas con filas y columnas, MongoDB almacena los datos en documentos JSON (en formato BSON internamente) dentro de colecciones.

### Características de MongoDB

- **Flexible**: No requiere un esquema fijo para los datos.
- **Escalable**: Soporta particionamiento horizontal (sharding).
- **Alto rendimiento**: Es rápida y escalable, ideal para aplicaciones con grandes volúmenes de datos.
- **Consultas ricas**: Soporta consultas complejas con agregaciones y operaciones de búsqueda avanzadas.
- **Soporte para replicación**: Permite la replicación para alta disponibilidad.

---

## Instalación de MongoDB

### Paso 1: Instalar MongoDB

Puedes instalar MongoDB en tu máquina local desde la [página oficial de MongoDB](https://www.mongodb.com/try/download/community). Dependiendo de tu sistema operativo, sigue las instrucciones de instalación.

### Paso 2: Ejecutar MongoDB

Una vez instalado, puedes iniciar el servidor MongoDB con el siguiente comando:

```bash
mongod
```

Esto iniciará el servidor MongoDB localmente en el puerto `27017` (por defecto).

### Paso 3: Conectar a MongoDB Shell

Para conectarte a MongoDB y comenzar a interactuar con la base de datos, abre una nueva terminal y escribe:

```bash
mongo
```

Esto iniciará el **MongoDB Shell** donde puedes ejecutar comandos directamente contra la base de datos.

---

## Operaciones CRUD en MongoDB

MongoDB utiliza comandos básicos para realizar operaciones CRUD (**Create, Read, Update, Delete**). Veamos cómo realizar estas operaciones básicas.

### Paso 4: Crear una base de datos y una colección

En MongoDB, los datos se organizan en **bases de datos** que contienen **colecciones** de documentos. Para seleccionar o crear una base de datos, usa el comando `use`:

```bash
use mi_base_de_datos
```

Este comando selecciona la base de datos llamada `mi_base_de_datos`. Si no existe, MongoDB la crea automáticamente cuando insertas datos.

Para crear una colección, simplemente inserta un documento en ella.

### Paso 5: Insertar documentos (Create)

Los documentos en MongoDB se representan como objetos JSON. Para insertar un documento en una colección, usamos el comando `insertOne` o `insertMany`.

#### Insertar un solo documento

```javascript
db.usuarios.insertOne({
  nombre: "Juan Pérez",
  edad: 28,
  correo: "juan.perez@example.com"
});
```

#### Insertar múltiples documentos

```javascript
db.usuarios.insertMany([
  { nombre: "Ana García", edad: 24, correo: "ana.garcia@example.com" },
  { nombre: "Carlos Ruiz", edad: 35, correo: "carlos.ruiz@example.com" }
]);
```

### Paso 6: Leer documentos (Read)

Para consultar documentos en una colección, usamos el comando `find`. Puedes pasarle criterios de búsqueda o dejarlo vacío para devolver todos los documentos.

#### Consultar todos los documentos

```javascript
db.usuarios.find();
```

#### Consultar con criterios de búsqueda

Por ejemplo, buscar un usuario por su nombre:

```javascript
db.usuarios.find({ nombre: "Juan Pérez" });
```

#### Consultar con condiciones avanzadas

Puedes buscar documentos con condiciones más complejas:

```javascript
db.usuarios.find({ edad: { $gt: 25 } });  // Usuarios mayores de 25 años
```

Aquí `$gt` significa "greater than" (mayor que).

### Paso 7: Actualizar documentos (Update)

Para modificar documentos existentes, puedes usar `updateOne`, `updateMany` o `findOneAndUpdate`. Estas operaciones te permiten modificar uno o varios documentos que coincidan con un criterio.

#### Actualizar un documento

```javascript
db.usuarios.updateOne(
  { nombre: "Juan Pérez" },  // Criterio de búsqueda
  { $set: { edad: 29 } }     // Actualización
);
```

En este ejemplo, se actualiza la edad de Juan Pérez a 29.

#### Actualizar múltiples documentos

```javascript
db.usuarios.updateMany(
  { edad: { $lt: 30 } },     // Usuarios menores de 30 años
  { $set: { activo: true } } // Agregar el campo "activo"
);
```

### Paso 8: Eliminar documentos (Delete)

Para eliminar documentos, puedes usar `deleteOne` o `deleteMany`.

#### Eliminar un documento

```javascript
db.usuarios.deleteOne({ nombre: "Carlos Ruiz" });
```

#### Eliminar múltiples documentos

```javascript
db.usuarios.deleteMany({ edad: { $gte: 35 } });  // Eliminar usuarios mayores de 35
```

---

## Índices en MongoDB

MongoDB permite crear índices en colecciones para mejorar la velocidad de las consultas, especialmente cuando se buscan datos en grandes volúmenes.

### Crear un índice

Puedes crear un índice en un campo específico para acelerar las consultas:

```javascript
db.usuarios.createIndex({ correo: 1 });
```

Aquí, el `1` indica que el índice se crea en orden ascendente. MongoDB utilizará este índice cuando se realicen búsquedas en el campo `correo`.

### Ver los índices existentes

```javascript
db.usuarios.getIndexes();
```

### Eliminar un índice

Si deseas eliminar un índice, puedes hacerlo con el siguiente comando:

```javascript
db.usuarios.dropIndex({ correo: 1 });
```

---

## Uso avanzado: Agregaciones en MongoDB

MongoDB tiene un poderoso **framework de agregación** que permite realizar operaciones complejas como sumar, agrupar y procesar datos en una secuencia de etapas.

### Ejemplo básico de agregación

Supongamos que queremos calcular el promedio de edad de los usuarios.

```javascript
db.usuarios.aggregate([
  {
    $group: {
      _id: null,            // No queremos agrupar por un campo específico
      promedioEdad: { $avg: "$edad" }  // Calcular el promedio de la edad
    }
  }
]);
```

### Filtrar y agrupar datos

Podemos combinar varias etapas, como `match` (para filtrar) y `group` (para agrupar):

```javascript
db.usuarios.aggregate([
  { $match: { edad: { $gt: 25 } } },  // Filtrar usuarios mayores de 25 años
  { $group: { _id: null, total: { $sum: 1 } } }  // Contar cuántos usuarios cumplen la condición
]);
```

---

## Replicación y Sharding en MongoDB

MongoDB es excelente para aplicaciones escalables, ya que soporta **replicación** y **sharding**.

- **Replicación**: Permite mantener copias de los datos en múltiples servidores para mejorar la disponibilidad. Si un servidor falla, otro puede tomar su lugar.
  
- **Sharding**: Permite dividir los datos en diferentes nodos, distribuyendo la carga y permitiendo manejar grandes volúmenes de datos.

---

## Conclusión

**MongoDB** es una poderosa base de datos NoSQL que ofrece flexibilidad y escalabilidad para manejar grandes cantidades de datos no estructurados. Su capacidad para realizar operaciones CRUD simples, junto con características avanzadas como el marco de agregación, índices, replicación y sharding, la convierte en una opción ideal para aplicaciones modernas.

### Documentación oficial

Para más detalles sobre cómo utilizar MongoDB y todas sus características avanzadas, te recomendamos que revises la documentación oficial:

- [MongoDB Documentation](https://docs.mongodb.com/)
