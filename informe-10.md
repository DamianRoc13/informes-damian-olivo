# TAS10 - Aplicación en modo producción

## 1. Título

Despliegue de una Aplicación Completa Frontend + Backend + Base de Datos en Modo Producción usando Docker y Nginx

## 2. Tiempo de duración

Aproximadamente 35 minutos para completar toda la práctica.

## 3. Fundamentos

En esta práctica se busca comprender el proceso de desplegar una aplicación web completa en un entorno de producción utilizando contenedores Docker. Se incluye una aplicación frontend construida con React, un backend desarrollado en NestJS, y una base de datos PostgreSQL.

El enfoque de producción implica:

- Separar el proceso de compilación (`build`) del frontend en un contenedor específico con Node.js.
- Servir los archivos estáticos resultantes desde un contenedor Nginx.
- Garantizar la comunicación fluida entre contenedores mediante `docker-compose`, lo que permite orquestar todo el entorno de manera controlada y reutilizable.

## 4. Conocimientos previos

Para realizar esta práctica fue necesario manejar:

* Sintaxis de `Dockerfile` y uso de `docker-compose`.
* Comandos Docker (`build`, `run`, `logs`, etc.).
* Estructura de un proyecto React y NestJS.
* Configuración de un archivo `nginx.conf`.
* Principios básicos de redes y resolución de nombres entre contenedores.

## 5. Objetivos a alcanzar

* Construir una imagen del frontend en modo producción.
* Crear un contenedor con Nginx para servir dicha app.
* Conectar el frontend con un backend NestJS dentro de otro contenedor.
* Orquestar todos los servicios (frontend, backend, base de datos) con `docker-compose`.

## 6. Equipo necesario

* Docker y Docker Compose instalado.
* Proyecto funcional con frontend React y backend NestJS. (De semana 9)

## 7. Material de apoyo

* Documentación oficial de Docker
* Manual de Nginx

## 8. Procedimiento

### Paso 1: Verificar funcionamiento local

Se verificó que el backend y frontend funcionaban correctamente de forma separada en desarrollo, y que el endpoint `/activaciones` devolvía datos esperados.

### Paso 2: Crear Dockerfile de construcción del Frontend

Dentro de la carpeta `frontend/` se creó un Dockerfile con doble etapa:

```Dockerfile
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Producción con Nginx
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
````

<p align="center">
  <img src="img/informe-10/Captura de Pantalla 2025-06-14 a la(s) 4.34.37 p.m..png" alt="" width="800px">
</p>

### Paso 3: Crear archivo nginx.conf

Este archivo permite a Nginx redirigir llamadas a `/activaciones` al backend NestJS:

```nginx
server {
    listen 80;

    location / {
        root /usr/share/nginx/html;
        index index.html;
        try_files $uri $uri/ /index.html;
    }

    location /activaciones {
        proxy_pass http://backend:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

### Paso 4: Crear archivo docker-compose.yml

Este archivo permitió levantar los tres servicios: base de datos PostgreSQL, backend NestJS y frontend con Nginx.

```yaml
version: "3.9"
services:
  db:
    image: postgres:15
    container_name: gitaf-db
    restart: always
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: 123
      POSTGRES_DB: gitaf
    volumes:
      - db_data:/var/lib/postgresql/data
    networks:
      - gitaf-net

  backend:
    build:
      context: ./backend
    container_name: gitaf-backend
    restart: always
    ports:
      - "3000:3000"
    environment:
      DATABASE_URL: postgres://postgres:123@db:5432/gitaf
    depends_on:
      - db
    networks:
      - gitaf-net

  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    container_name: gitaf-frontend
    restart: always
    ports:
      - "8080:80"
    depends_on:
      - backend
    networks:
      - gitaf-net

volumes:
  db_data:

networks:
  gitaf-net:
```
<p align="center">
  <img src="img/informe-10/" alt="" width="800px">
</p>

### Paso 5: Construir e iniciar los contenedores

```bash
docker compose down --volumes --remove-orphans
docker compose build --no-cache
docker compose up -d
```

<p align="center">
  <img src="img/informe-10/Captura de Pantalla 2025-06-14 a la(s) 4.35.58 p.m..png" alt="" width="800px">
</p>

### Paso 6: Verificar funcionamiento de la app

Desde el navegador se accedió a:

[http://localhost:8080](http://localhost:8080)


## 9. Resultados esperados

#### Figura 1.1. Aplicación React funcionando con datos de backend en producción

<p align="center">
  <img src="img/informe-10/Captura de Pantalla 2025-06-14 a la(s) 4.33.55 p.m..png" alt="App funcionando en producción" width="800px">
</p>

## 10. Bibliografía

* Docker Docs. [https://docs.docker.com](https://docs.docker.com)
* Nginx Docs. [https://nginx.org/en/docs/](https://nginx.org/en/docs/)


