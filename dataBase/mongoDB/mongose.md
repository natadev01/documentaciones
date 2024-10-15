# Guía de MongoDB con Mongoose

## ¿Qué es MongoDB?

**MongoDB** es una base de datos NoSQL orientada a documentos, lo que significa que almacena datos en formato BSON (similar a JSON). En lugar de almacenar datos en tablas como en las bases de datos relacionales, MongoDB almacena los datos en "colecciones" y cada registro es un "documento" que puede tener una estructura flexible.

MongoDB es ideal para aplicaciones modernas que necesitan manejar grandes volúmenes de datos no estructurados o semi-estructurados de manera eficiente.

## ¿Qué es Mongoose?

**Mongoose** es una biblioteca ODM (Object Data Modeling) para **Node.js** que facilita la interacción con bases de datos **MongoDB**. Proporciona una forma sencilla de modelar tus datos, definir esquemas, y hacer consultas, además de permitirte aplicar validaciones y hooks en tus datos de forma automática.

Mongoose actúa como una capa de abstracción sobre MongoDB, ayudando a estructurar tus datos y a interactuar con ellos de manera más eficiente.

### Características principales de Mongoose

- **Esquemas**: Define la estructura de los documentos dentro de una colección.
- **Modelos**: Representa colecciones dentro de MongoDB.
- **Validaciones**: Permite validar datos antes de que se guarden en la base de datos.
- **Middlewares**: Hooks pre y post que permiten realizar acciones antes o después de ciertas operaciones (como guardar o actualizar).
- **Consultas**: Métodos que permiten interactuar con la base de datos (CRUD: Crear, Leer, Actualizar, Eliminar).

---

## Instalación de MongoDB y Mongoose

### Paso 1: Instalar MongoDB

Primero, asegúrate de tener **MongoDB** instalado en tu sistema. Puedes descargar e instalar MongoDB desde su [página oficial](https://www.mongodb.com/try/download/community).

### Paso 2: Instalar Mongoose

Para usar Mongoose en tu proyecto de **Node.js**, primero crea un proyecto Node.js (si aún no lo tienes):

```bash
mkdir my-mongo-project
cd my-mongo-project
npm init -y
```

Luego, instala **mongoose**:

```bash
npm install mongoose
```

---

## Uso básico de Mongoose

### Paso 3: Conectar Mongoose con MongoDB

Para empezar a usar Mongoose, necesitas establecer una conexión con tu base de datos MongoDB.

```javascript
const mongoose = require('mongoose');

// Conectar a la base de datos MongoDB
mongoose.connect('mongodb://localhost:27017/mi_base_de_datos', {
  useNewUrlParser: true,
  useUnifiedTopology: true
})
  .then(() => console.log('Conectado a MongoDB'))
  .catch(err => console.error('Error al conectar a MongoDB:', err));
```

### Paso 4: Definir un Esquema y un Modelo

En MongoDB, los documentos pueden tener cualquier estructura, pero Mongoose nos permite definir **esquemas** que aseguran que nuestros documentos tengan una forma predefinida.

#### Definir un esquema

Vamos a definir un esquema para una colección de `Usuarios`:

```javascript
const userSchema = new mongoose.Schema({
  nombre: {
    type: String,
    required: true,
  },
  edad: {
    type: Number,
    min: 18,  // Valida que el usuario sea mayor de edad
  },
  correo: {
    type: String,
    required: true,
    unique: true  // Valida que el correo sea único
  },
  fechaCreacion: {
    type: Date,
    default: Date.now  // Establece la fecha de creación por defecto
  }
});
```

#### Crear un modelo

Un **modelo** es una clase construida a partir de un esquema. Un modelo representa una colección de documentos en MongoDB. Aquí estamos creando el modelo `User`:

```javascript
const User = mongoose.model('User', userSchema);
```

### Paso 5: Crear un documento (Create)

Para insertar un nuevo usuario en la base de datos, puedes crear una instancia del modelo `User` y guardarla:

```javascript
const nuevoUsuario = new User({
  nombre: 'Juan Perez',
  edad: 25,
  correo: 'juan.perez@example.com'
});

nuevoUsuario.save()
  .then(user => console.log('Usuario guardado:', user))
  .catch(err => console.error('Error al guardar el usuario:', err));
```

### Paso 6: Leer documentos (Read)

Para consultar documentos de la base de datos, puedes usar el método `find` del modelo:

```javascript
User.find()
  .then(usuarios => console.log('Usuarios:', usuarios))
  .catch(err => console.error('Error al consultar usuarios:', err));

// Consulta un usuario específico por ID
User.findById('id_del_usuario')
  .then(usuario => console.log('Usuario encontrado:', usuario))
  .catch(err => console.error('Error al buscar usuario:', err));
```

### Paso 7: Actualizar documentos (Update)

Para actualizar un documento existente, puedes usar el método `findByIdAndUpdate`:

```javascript
User.findByIdAndUpdate('id_del_usuario', { edad: 26 }, { new: true })
  .then(usuarioActualizado => console.log('Usuario actualizado:', usuarioActualizado))
  .catch(err => console.error('Error al actualizar usuario:', err));
```

El tercer argumento `{ new: true }` asegura que obtendremos el documento actualizado en la respuesta.

### Paso 8: Eliminar documentos (Delete)

Para eliminar un documento, puedes usar el método `findByIdAndDelete`:

```javascript
User.findByIdAndDelete('id_del_usuario')
  .then(() => console.log('Usuario eliminado'))
  .catch(err => console.error('Error al eliminar usuario:', err));
```

---

## Conclusión

**Mongoose** simplifica mucho la interacción con bases de datos **MongoDB** en proyectos **Node.js**. Con la capacidad de definir esquemas y modelos, gestionar validaciones y realizar consultas de forma eficiente, es una herramienta poderosa para mantener el código limpio y estructurado.

Además, proporciona **middleware** y **hooks** que permiten ejecutar acciones antes o después de las operaciones CRUD, lo que añade flexibilidad en el manejo de datos.

### Documentación oficial

Para más detalles sobre el uso de Mongoose, puedes consultar la documentación oficial:

- [Mongoose Documentation](https://mongoosejs.com/docs/)
