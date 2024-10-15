# Guía de Docker: `docker run` y `docker-compose`

## ¿Qué es Docker?

**Docker** es una plataforma de contenedorización que permite a los desarrolladores empaquetar aplicaciones y sus dependencias en **contenedores**. Los contenedores son ligeros, portables y consistentes, lo que garantiza que las aplicaciones se ejecuten de la misma manera en cualquier entorno, ya sea en una máquina local, en producción o en la nube.

### Beneficios de Docker

- **Portabilidad**: Los contenedores pueden ejecutarse en cualquier sistema que tenga Docker instalado.
- **Aislamiento**: Cada contenedor tiene su propio entorno, lo que garantiza que las aplicaciones no interfieran entre sí.
- **Ligero**: Comparado con las máquinas virtuales, los contenedores son más ligeros y rápidos.
- **Escalabilidad**: Docker facilita el despliegue y la escalabilidad de aplicaciones.

---

## Uso básico de `docker run`

El comando `docker run` es la forma más directa de ejecutar contenedores. Este comando ejecuta un contenedor a partir de una imagen de Docker. Puedes descargar imágenes desde [Docker Hub](https://hub.docker.com/) o construir tus propias imágenes.

### Sintaxis básica de `docker run`

```bash
docker run [opciones] <nombre-imagen>
```

### Opciones comunes de `docker run`

1. **-d**: Ejecuta el contenedor en segundo plano (modo **desacoplado** o "detached").
2. **-p**: Mapea puertos del contenedor al host (`-p <host-port>:<container-port>`).
3. **--name**: Asigna un nombre al contenedor.
4. **-v**: Monta un volumen (directorio compartido) entre el host y el contenedor.
5. **-e**: Establece variables de entorno en el contenedor.

### Ejemplos de `docker run`

#### Ejemplo 1: Ejecutar un contenedor NGINX

```bash
docker run -d -p 8080:80 --name mi-nginx nginx
```

En este ejemplo:

- Se ejecuta un contenedor **NGINX** en segundo plano (`-d`).
- Se mapea el puerto **80** del contenedor al puerto **8080** del host (`-p`).
- Se le asigna el nombre **mi-nginx** al contenedor.

Visita `http://localhost:8080` en tu navegador para ver la página predeterminada de NGINX.

#### Ejemplo 2: Ejecutar un contenedor con una variable de entorno

```bash
docker run -d -p 3000:3000 -e NODE_ENV=production --name mi-app node
```

Aquí estamos ejecutando una aplicación **Node.js** en modo producción, asignando la variable de entorno `NODE_ENV=production` y mapeando el puerto **3000** del contenedor al puerto **3000** del host.

#### Ejemplo 3: Ejecutar un contenedor con un volumen

```bash
docker run -d -p 27017:27017 -v ~/mongo-data:/data/db --name mi-mongo mongo
```

En este ejemplo, ejecutamos un contenedor **MongoDB** y montamos un volumen (`-v`) para persistir los datos. El directorio del host `~/mongo-data` se vincula al directorio de datos del contenedor `/data/db`.

---

## Uso básico de `docker-compose`

**Docker Compose** es una herramienta que permite definir y ejecutar aplicaciones multi-contenedor. Con Compose, puedes configurar contenedores, redes, y volúmenes usando un archivo YAML llamado `docker-compose.yml`.

### Instalación de Docker Compose

Docker Compose suele venir instalado con Docker Desktop. Si no lo tienes instalado, puedes instalarlo desde la [página oficial de Docker Compose](https://docs.docker.com/compose/install/).

### Crear un archivo `docker-compose.yml`

El archivo `docker-compose.yml` define los servicios (contenedores) que quieres ejecutar, así como la configuración de red, los volúmenes y las variables de entorno.

#### Sintaxis básica de `docker-compose.yml`

```yaml
version: '3'
services:
  nombre-del-servicio:
    image: nombre-imagen
    ports:
      - "host-port:container-port"
    environment:
      - VARIABLE_ENTORNO=valor
    volumes:
      - host-directory:container-directory
```

### Ejemplo básico de `docker-compose.yml`

A continuación, veamos cómo configurar una aplicación **Node.js** con una base de datos **MongoDB** utilizando Docker Compose.

```yaml
version: '3'

services:
  app:
    image: node
    container_name: mi-app
    ports:
      - "3000:3000"
    volumes:
      - ./app:/usr/src/app
    environment:
      - NODE_ENV=production
    depends_on:
      - db
    command: "npm start"

  db:
    image: mongo
    container_name: mi-mongo
    ports:
      - "27017:27017"
    volumes:
      - ./mongo-data:/data/db
```

### Explicación del ejemplo

- **app**:
  - Utiliza la imagen **node**.
  - Mapea el puerto **3000** del host al puerto **3000** del contenedor.
  - Monta el directorio `./app` del host en `/usr/src/app` dentro del contenedor.
  - Establece la variable de entorno `NODE_ENV=production`.
  - Depende del servicio **db**, por lo que el contenedor de la base de datos MongoDB se ejecutará primero.
  - El contenedor ejecuta el comando `npm start` al inicio.
  
- **db**:
  - Utiliza la imagen **mongo**.
  - Mapea el puerto **27017** del host al puerto **27017** del contenedor.
  - Monta el directorio `./mongo-data` para persistir los datos.

### Ejecutar Docker Compose

1. Guarda el archivo anterior como `docker-compose.yml`.
2. En la misma carpeta donde está el archivo, ejecuta:

```bash
docker-compose up
```

Este comando ejecutará ambos servicios, **app** y **db**, y creará la red y los volúmenes necesarios. Para detener los contenedores:

```bash
docker-compose down
```

---

## Conclusión

- **`docker run`** es una forma rápida de ejecutar contenedores de forma individual. Es útil para realizar pruebas rápidas o ejecutar aplicaciones simples.
- **`docker-compose`** es ideal para ejecutar múltiples contenedores que necesitan interactuar entre sí, como una aplicación y su base de datos.

### Documentación oficial

- [Documentación de Docker Run](https://docs.docker.com/engine/reference/run/)
- [Documentación de Docker Compose](https://docs.docker.com/compose/)
