# Documento sobre Arquitectura Limpia y Clean Code

## Introducción

La **Arquitectura Limpia** (Clean Architecture) es un enfoque de diseño de software que busca mantener la independencia de los detalles de implementación, haciendo que el software sea flexible y fácil de mantener. El objetivo principal es dividir el código en capas con responsabilidades claras, separando la lógica de negocio de los detalles técnicos, como bases de datos o frameworks.

El **Clean Code** se refiere a un conjunto de prácticas que permiten escribir código legible, mantenible y comprensible. Estas prácticas fomentan la simplicidad, claridad y un diseño consistente en todas las capas del software.

Este documento explora los principios de la Arquitectura Limpia y Clean Code, aplicados a tres frameworks populares: **Angular**, **Node.js** y **React**, con un enfoque en la organización de la estructura de archivos del proyecto.

---

### Principios de la Arquitectura Limpia

1. **Independencia de Frameworks**: El sistema no depende de un framework o tecnología específica.
2. **Testabilidad**: El código debe ser fácil de probar sin depender de detalles de infraestructura.
3. **Independencia de la interfaz de usuario**: La lógica de negocio debe estar separada de la capa de presentación.
4. **Independencia de bases de datos**: Los detalles de persistencia no deben afectar la lógica del negocio.
5. **Dependencias controladas**: La lógica de negocio nunca debe depender de implementaciones concretas, sino de abstracciones.

### Principios de Clean Code

1. **Simplicidad**: El código debe ser claro y fácil de entender, sin complejidad innecesaria.
2. **Nombres significativos**: Las variables, funciones y clases deben tener nombres descriptivos que indiquen claramente su propósito.
3. **Funciones pequeñas**: Las funciones deben hacer una sola cosa y hacerla bien.
4. **Evitar duplicación de código**: Aplicar el principio DRY (Don't Repeat Yourself).
5. **Modularidad**: El código debe estar organizado en módulos cohesivos y desacoplados.

---

### Arquitectura Limpia y Clean Code en Angular

En Angular, se recomienda organizar los archivos del proyecto de manera modular, separando las responsabilidades de componentes, servicios y módulos.

#### Estructura de Archivos

```bash
src/
  app/
    core/              # Servicios, interceptores y lógica que debe compartirse en todo el proyecto.
    shared/            # Módulos y componentes reutilizables.
    features/          # Módulos funcionales que representan una característica específica.
      user/
        components/
          user-list/
          user-detail/
        services/
          user.service.ts
        user.module.ts
    app.component.ts
    app.module.ts
```

#### Ejemplo de Servicio Limpio en Angular

```typescript
@Injectable({
  providedIn: 'root',
})
export class UserService {
  constructor(private http: HttpClient) {}

  getUsers(): Observable<User[]> {
    return this.http.get<User[]>('https://api.example.com/users');
  }
}
```

Este servicio sigue principios de Clean Code al tener una responsabilidad única y usar nombres claros.

#### Ejemplo de Arquitectura Limpia en Angular

En la capa de **servicios** (core), podemos tener toda la lógica de negocio y API calls, mientras que los **componentes** de la capa de presentación se limitan a interactuar con la interfaz de usuario y llamar a los servicios.

---

### Arquitectura Limpia y Clean Code en Node.js

En Node.js, especialmente con frameworks como Express.js, es importante mantener una estructura modular y bien organizada. Aquí se recomienda separar las capas de negocio, acceso a datos y controladores.

#### Estructura de Archivos Node.js

```bash
src/
  controllers/        # Controladores para manejar las solicitudes HTTP.
    user.controller.js
  services/           # Lógica de negocio y reglas de aplicación.
    user.service.js
  repositories/       # Comunicación con la base de datos o fuentes de datos externas.
    user.repository.js
  models/             # Definición de esquemas o clases de dominio.
    user.model.js
  routes/             # Definición de las rutas de la API.
    user.routes.js
  app.js              # Punto de entrada de la aplicación.
```

#### Ejemplo de Servicio Limpio en Node.js

```javascript
class UserService {
  constructor(userRepository) {
    this.userRepository = userRepository;
  }

  async getAllUsers() {
    return await this.userRepository.findAll();
  }
}

module.exports = UserService;
```

Aquí, el **servicio** solo se enfoca en la lógica de negocio, delegando las operaciones de base de datos al **repositorio**.

---

### Arquitectura Limpia y Clean Code en React

En React, la organización del código también sigue el principio de separación de responsabilidades, donde los componentes son los encargados de la vista, mientras que la lógica de estado y las llamadas API se manejan en hooks o servicios externos.

#### Estructura de Archivos React

```bash
src/
  components/         # Componentes de UI.
    UserList.js
    UserDetail.js
  hooks/              # Lógica personalizada en forma de hooks.
    useFetchUsers.js
  services/           # Llamadas a APIs y lógica de negocio.
    userService.js
  contexts/           # Contextos para manejar el estado global.
    UserContext.js
  App.js              # Componente raíz.
```

#### Ejemplo de Hook Limpio en React

```javascript
import { useState, useEffect } from 'react';
import { getUsers } from '../services/userService';

export function useFetchUsers() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    getUsers().then((data) => {
      setUsers(data);
      setLoading(false);
    });
  }, []);

  return { users, loading };
}
```

Aquí, el **hook** `useFetchUsers` sigue principios de Clean Code al ser modular, fácil de leer y con nombres claros.

#### Ejemplo de Servicio en React

```javascript
export async function getUsers() {
  const response = await fetch('https://api.example.com/users');
  return await response.json();
}
```

---

### Conclusión

Aplicar los principios de **Arquitectura Limpia** y **Clean Code** garantiza que nuestras aplicaciones sean fáciles de mantener, escalar y entender. La organización modular y la separación de responsabilidades son esenciales para construir software flexible y resistente al cambio, independientemente de la tecnología o framework que se utilice.

Cada framework tiene sus particularidades, pero los conceptos de diseño y arquitectura son universales. Mantener una estructura limpia y clara en el proyecto facilita no solo el desarrollo, sino también la colaboración en equipo y la incorporación de nuevas funcionalidades en el futuro.
