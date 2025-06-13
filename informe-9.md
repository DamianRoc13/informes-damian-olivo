# TAS9 - Contenedor Frontend

## 1. Título

Dockerización de una Aplicación Frontend con React y Backend con NestJS mediante Docker Compose.

## 2. Tiempo de duración

Aproximadamente 30 minutos para completar toda la práctica.

## 3. Fundamentos

En esta práctica se busca dockerizar una aplicación completa compuesta por un frontend en React y un backend en NestJS. Ambos servicios deben ejecutarse en contenedores separados y comunicarse a través de una red común definida en Docker Compose.

El objetivo es exponer el frontend en el navegador y que este consuma datos desde un endpoint expuesto por el backend. Esta arquitectura simula un entorno real de desarrollo y despliegue, permitiendo comprender la estructura básica de aplicaciones contenerizadas que usan múltiples servicios intercomunicados.

## 4. Conocimientos previos

Para realizar esta práctica fue necesario manejar:

* Sintaxis y lógica de `Dockerfile` para Node.js.
* Comandos básicos de Docker (`build`, `run`, `compose`, etc.).
* Estructura de proyectos en React y NestJS.
* Conceptos de redes y comunicación entre contenedores con Docker Compose.

## 5. Objetivos a alcanzar

* Verificar que ambos proyectos funcionan correctamente en entorno local.
* Crear imágenes Docker funcionales para frontend y backend.
* Ejecutar ambos servicios en contenedores interconectados mediante `docker-compose`.
* Comprobar que el frontend consume correctamente los datos desde la API.

## 6. Equipo necesario

* Docker y Docker Compose instalados.
* Proyecto React funcional.
* Proyecto NestJS funcional.

## 7. Material de apoyo

* Documentación oficial de Docker

## 8. Procedimiento

### Paso 1: Crear estructura del proyecto

Se organizó la estructura en carpetas separadas:

```

gitaf-system/
├── backend/       
├── frontend/      
└── docker-compose.yml

````

### Paso 2: Dockerizar el Backend NestJS

Se creó un archivo `Dockerfile` en la carpeta `backend`:

<p align="center"> <img src="img/informe-9/Captura de Pantalla 2025-06-12 a la(s) 4.40.46 p.m..png" alt="" width="800px"> </p>

Además, en `main.ts` se configuró el servidor para aceptar conexiones externas con CORS.



### Paso 3: Dockerizar el Frontend React

Se creó el siguiente `Dockerfile` dentro de `frontend`:

<p align="center"> <img src="img/informe-9/Captura de Pantalla 2025-06-12 a la(s) 4.40.28 p.m..png" alt="" width="800px"> </p>

### Paso 4: Modificar fetch en el frontend

En el componente `ActivacionesTable.tsx`, se apuntó a la API del backend usando el nombre del servicio en Docker Compose:

```ts
fetch('http://backend:3000/activaciones')
```

### Paso 5: Crear el archivo `docker-compose.yml`

Se creó el archivo `docker-compose.yml` en la raíz del proyecto:

```yaml
version: '3.8'

services:
  backend:
    build: ./backend
    ports:
      - "3000:3000"
    networks:
      - gitaf-net

  frontend:
    build: ./frontend
    ports:
      - "5173:4173"
    networks:
      - gitaf-net
    depends_on:
      - backend

networks:
  gitaf-net:
```

<p align="center"> <img src="img/informe-9/Captura de Pantalla 2025-06-12 a la(s) 4.39.07 p.m..png" alt="Estructura docker-compose" width="800px"> </p>

### Paso 6: Ejecutar los contenedores

Desde la raíz del proyecto se ejecutó:

```bash
docker-compose up --build
```
<p align="center"> <img src="img/informe-9/Captura de Pantalla 2025-06-12 a la(s) 4.23.54 p.m..png" alt="" width="800px"> </p>

Esto levantó los dos servicios en paralelo y se accedió a la app desde:

* Frontend: [http://localhost:5173](http://localhost:5173)
* Backend: [http://localhost:3000/activaciones](http://localhost:3000/activaciones)

<p align="center"> <img src="img/informe-9/Captura de Pantalla 2025-06-12 a la(s) 4.37.22 p.m..png" alt="App Gitaf en React mostrando datos" width="800px"> </p>

## 9. Resultados esperados

#### Figura 1.1. Aplicación React mostrando datos desde la API NestJS en contenedor Docker

<p align="center"> <img src="img/informe-9/Captura de Pantalla 2025-06-12 a la(s) 4.35.17 p.m..png" alt="Tabla con datos de activaciones" width="800px"> </p>

<p align="center"> <img src="img/informe-9/Captura de Pantalla 2025-06-12 a la(s) 4.36.15 p.m..png" alt="" width="800px"> </p>

## 10. Bibliografía

* Docker Documentation. (n.d.). [https://docs.docker.com](https://docs.docker.com)
* React Docs. (n.d.). [https://react.dev](https://react.dev)
* NestJS Docs. (n.d.). [https://docs.nestjs.com](https://docs.nestjs.com)

