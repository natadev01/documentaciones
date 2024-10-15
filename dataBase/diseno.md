# Diseño de Base de Datos: SQL vs NoSQL

El diseño de bases de datos es crucial para garantizar que las aplicaciones puedan manejar eficientemente grandes cantidades de datos, mantener la integridad y ser escalables. La elección entre **bases de datos relacionales (SQL)** y **bases de datos NoSQL** depende del tipo de datos y de cómo estos se utilizarán en la aplicación.

## Comparación General

| Característica           | SQL (Relacional)                                             | NoSQL (No Relacional)                                            |
|--------------------------|--------------------------------------------------------------|------------------------------------------------------------------|
| **Estructura**            | Tablas con filas y columnas. Basado en relaciones.           | Basado en documentos, pares clave-valor, grafos o columnas.      |
| **Esquema**               | Esquema fijo y bien definido.                               | Esquema flexible, los datos pueden variar en estructura.         |
| **Normalización**         | Datos normalizados para evitar redundancia.                 | Datos denormalizados, optimizado para consultas rápidas.         |
| **Consultas**             | Lenguaje SQL (Structured Query Language).                    | No usa SQL. Dependiendo del tipo de NoSQL, puede tener consultas personalizadas (ej: MongoDB usa consultas JSON). |
| **Consistencia**          | Consistencia estricta (ACID).                               | Generalmente usa **eventual consistency** en lugar de consistencia estricta. |
| **Escalabilidad**         | Escalabilidad vertical (mejora de hardware).                 | Escalabilidad horizontal (agregar más servidores).               |
| **Mejores para**          | Sistemas transaccionales, integridad referencial.            | Grandes volúmenes de datos, aplicaciones en tiempo real, datos no estructurados. |

---

## Diseño de Bases de Datos Relacionales (SQL)

En las bases de datos SQL (como **MySQL**, **PostgreSQL**, **SQL Server**), los datos se organizan en **tablas** que tienen filas y columnas. Los datos se relacionan entre sí a través de **claves primarias** y **claves foráneas**, y es esencial diseñar un buen **modelo de datos relacional** para garantizar la eficiencia.

### Pasos para el diseño de una base de datos SQL

### 1. **Identificar las entidades y atributos**

Una entidad es algo sobre lo que se desea almacenar información. Los atributos son las propiedades de las entidades.

#### Ejemplo de entidades

- **Usuario** (atributos: `id_usuario`, `nombre`, `correo`, `fecha_creacion`).
- **Producto** (atributos: `id_producto`, `nombre`, `precio`, `categoria`).
- **Pedido** (atributos: `id_pedido`, `fecha`, `monto_total`, `id_usuario`).

### 2. **Establecer relaciones entre las entidades**

Las relaciones entre las entidades pueden ser de diferentes tipos:

- **Uno a uno**: Ej. un usuario tiene un único perfil.
- **Uno a muchos**: Ej. un usuario puede tener muchos pedidos.
- **Muchos a muchos**: Ej. un pedido puede tener muchos productos, y un producto puede estar en muchos pedidos.

### 3. **Normalización**

El proceso de **normalización** divide los datos en tablas más pequeñas para eliminar la redundancia. Por ejemplo, los pedidos deben estar en una tabla separada de los usuarios para evitar que se repitan datos del usuario en cada pedido.

### 4. **Definir las claves primarias y foráneas**

- **Clave primaria (PK)**: Un identificador único para cada registro en una tabla (Ej: `id_usuario` en la tabla de usuarios).
- **Clave foránea (FK)**: Un campo en una tabla que hace referencia a la clave primaria de otra tabla para establecer relaciones (Ej: `id_usuario` en la tabla de pedidos que referencia a la tabla de usuarios).

### Ejemplo de diseño SQL

#### Diagrama de tablas (MySQL)

1. **Usuarios**:

   ```sql
   CREATE TABLE Usuarios (
       id_usuario INT PRIMARY KEY AUTO_INCREMENT,
       nombre VARCHAR(100) NOT NULL,
       correo VARCHAR(100) UNIQUE NOT NULL,
       fecha_creacion DATE NOT NULL
   );
   ```

2. **Productos**:

   ```sql
   CREATE TABLE Productos (
       id_producto INT PRIMARY KEY AUTO_INCREMENT,
       nombre VARCHAR(100) NOT NULL,
       precio DECIMAL(10, 2) NOT NULL,
       categoria VARCHAR(50)
   );
   ```

3. **Pedidos**:

   ```sql
   CREATE TABLE Pedidos (
       id_pedido INT PRIMARY KEY AUTO_INCREMENT,
       fecha DATE NOT NULL,
       monto_total DECIMAL(10, 2) NOT NULL,
       id_usuario INT,
       FOREIGN KEY (id_usuario) REFERENCES Usuarios(id_usuario)
   );
   ```

4. **Pedidos_Productos** (Tabla intermedia para la relación muchos a muchos):

   ```sql
   CREATE TABLE Pedidos_Productos (
       id_pedido INT,
       id_producto INT,
       cantidad INT NOT NULL,
       FOREIGN KEY (id_pedido) REFERENCES Pedidos(id_pedido),
       FOREIGN KEY (id_producto) REFERENCES Productos(id_producto)
   );
   ```

### Ventajas de SQL

- **Integridad de los datos**: El uso de claves foráneas asegura la consistencia entre tablas relacionadas.
- **Consultas complejas**: Con SQL, puedes realizar consultas complejas y uniones (joins) para obtener datos de múltiples tablas.
- **Transacciones ACID**: Las bases de datos SQL proporcionan consistencia fuerte a través de transacciones.

---

## Diseño de Bases de Datos NoSQL

Las bases de datos **NoSQL** (como **MongoDB**, **Cassandra**, **Redis**) están diseñadas para manejar datos no estructurados o semi-estructurados a gran escala. No siguen el modelo relacional, y los datos se almacenan en formas flexibles como documentos JSON, pares clave-valor o grafos.

### Tipos de bases de datos NoSQL

1. **Documentos**: Ej. **MongoDB**, **CouchDB** (almacenan datos en documentos JSON o BSON).
2. **Clave-Valor**: Ej. **Redis**, **DynamoDB** (almacenan pares clave-valor).
3. **Grafos**: Ej. **Neo4j** (modelan relaciones complejas entre datos como grafos).
4. **Columnas**: Ej. **Cassandra**, **HBase** (almacenan datos en formato de columnas).

### Pasos para el diseño de una base de datos NoSQL (MongoDB)

### 1. **Identificar las colecciones y documentos**

En lugar de tablas, las bases de datos como MongoDB almacenan datos en **colecciones** y los registros en **documentos** (similares a objetos JSON).

#### Ejemplo de colecciones

- **Usuarios** (documentos: `id`, `nombre`, `correo`, `fecha_creacion`).
- **Productos** (documentos: `id`, `nombre`, `precio`, `categoria`).
- **Pedidos** (documentos: `id_pedido`, `fecha`, `monto_total`, `usuario`, `productos`).

### 2. **Denormalización de datos**

A diferencia de SQL, en NoSQL a menudo **denormalizas** los datos para mejorar el rendimiento y reducir la cantidad de consultas. Esto significa que puedes almacenar datos relacionados directamente en un documento en lugar de en tablas separadas.

#### Ejemplo

En lugar de tener una tabla de usuarios y una tabla de pedidos separada, puedes almacenar los pedidos directamente dentro del documento del usuario.

```json
{
    "_id": "usuario_1",
    "nombre": "Juan Pérez",
    "correo": "juan.perez@example.com",
    "pedidos": [
        {
            "id_pedido": "pedido_1",
            "fecha": "2023-01-01",
            "monto_total": 250.00,
            "productos": [
                { "nombre": "Producto A", "precio": 100.00 },
                { "nombre": "Producto B", "precio": 150.00 }
            ]
        }
    ]
}
```

### 3. **Consultas rápidas**

En NoSQL, optimizas el diseño para realizar **consultas rápidas**. Esto significa que puedes duplicar datos si eso reduce el número de consultas necesarias.

### Ejemplo de diseño NoSQL

#### Colección de Usuarios (MongoDB)

```json
{
    "_id": "usuario_1",
    "nombre": "Juan Pérez",
    "correo": "juan.perez@example.com",
    "fecha_creacion": "2023-01-01",
    "pedidos": [
        {
            "id_pedido": "pedido_1",
            "fecha": "2023-01-01",
            "monto_total": 250.00,
            "productos": [
                { "id_producto": "prod_1", "nombre": "Producto A", "precio": 100.00 },
                { "id_producto": "prod_2", "nombre": "Producto B", "precio": 150.00 }
            ]
        }
    ]
}
```

#### Consultar documentos en MongoDB

```javascript
// Obtener un usuario por su ID
db.usuarios.find({ _id: "usuario_1" });

// Consultar pedidos de un usuario específico
db.usuarios.find({ _id: "usuario_1" }, { pedidos: 1 });
```

### Ventajas de NoSQL

- **Esquema flexible**: No es necesario definir un esquema fijo, los documentos pueden variar en estructura.
- **Escalabilidad horizontal**: Fácil de escalar agregando más servidores
  