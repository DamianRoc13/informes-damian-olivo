# TAS7 - Crear imagen personalizada

## 1. Título

Dockerización de una Aplicación React con Nginx utilizando Dockerfile.

## 2. Tiempo de duración

Aproximadamente 15 minutos para completar toda la práctica.

## 3. Fundamentos

En esta práctica se busca comprender el proceso de dockerizar una aplicación frontend desarrollada con React, utilizando como servidor web Nginx, todo configurado desde un único `Dockerfile`. Esta es una práctica común en entornos de producción para garantizar consistencia, portabilidad y facilidad de despliegue.

La app se sirve dentro de un contenedor, eliminando la dependencia del entorno local y facilitando su distribución. La estructura final se apoya en dos fases dentro del `Dockerfile`: una para compilar la app con Node y otra para servirla con Nginx.

## 4. Conocimientos previos

Para realizar esta práctica fue necesario manejar:

* Sintaxis y lógica de `Dockerfile`.
* Comandos básicos de Docker (`build`, `run`, `ps`, etc.).
* Funcionamiento del sistema de construcción de React (`npm run build`).
* Estructura básica de un archivo `nginx.conf`.

## 5. Objetivos a alcanzar

* Verificar que el proyecto React funciona correctamente en entorno local.
* Construir una imagen Docker funcional para servir la app.
* Ejecutar y verificar el funcionamiento del contenedor con Nginx.

## 6. Equipo necesario

* Computador con Docker instalado.
* Proyecto React funcional (carpeta `suda-frontend-s6`).
* Terminal de comandos Bash o Zsh.
* Editor de código Visual Studio Code.

## 7. Material de apoyo

* Documentación oficial de Docker
* Documentación de React
* Manual de Nginx

## 8. Procedimiento

### Paso 1: Verificar que el Frontend Funciona Localmente

Antes de dockerizar, comprobamos que la aplicación React corre sin errores en entorno local.

```bash
cd suda-frontend-s6

npm install

npm dev
```

Accedemos desde el navegador a [http://localhost:3000](http://localhost:3000)

### Paso 2: Crear el archivo `Dockerfile`

Creamos el archivo `Dockerfile` dentro de `suda-frontend-s6`:

```Dockerfile

FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build


FROM nginx:alpine
COPY --from=builder /app/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

<p align="center"> <img src="img/informe-7/Captura de Pantalla 2025-05-21 a la(s) 3.05.38 p.m..png" alt="Dockerfile creado" width="800px"> </p>

### Paso 3: Crear el archivo `nginx.conf`

Este archivo configura cómo Nginx debe servir los archivos estáticos de React.

Creamos el archivo `nginx.conf` en la misma carpeta:

```nginx
server {
    listen 80;
    location / {
        root /usr/share/nginx/html;
        index index.html;
        try_files $uri $uri/ /index.html;
    }
}
```

<p align="center"> <img src="img/informe-7/Captura de Pantalla 2025-05-21 a la(s) 2.51.34 p.m..png" alt="Archivo nginx.conf" width="800px"> </p>

### Paso 4: Construir la imagen Docker

Desde la terminal, generamos la imagen:

```bash
docker build -t suda-frontend .
```

Verificamos que la imagen fue creada:

```bash
docker images
```

<p align="center"> <img src="img/informe-7/Captura de Pantalla 2025-05-21 a la(s) 3.06.14 p.m..png" alt="Imagen creada" width="800px"> </p>

### Paso 5: Crear y ejecutar el contenedor

Ejecutamos la app en un contenedor:

```bash
docker run -d -p 3000:80 --name suda-frontend-container suda-frontend
```

Verificamos que el contenedor está en ejecución:

```bash
docker ps
```

Accedemos a la aplicación desde el navegador:

[http://localhost:3000](http://localhost:3000)

<p align="center"> <img src="img/informe-7/Captura de Pantalla 2025-05-21 a la(s) 3.07.43 p.m..png" alt="App funcionando en Docker" width="800px"> </p>

## 9. Resultados esperados

#### Figura 1.1. Aplicación React funcionando dentro del contenedor Docker

<p align="center"> <img src="img/informe-7/Captura de Pantalla 2025-05-21 a la(s) 3.08.27 p.m..png" alt="App React en contenedor" width="800px"> </p>


## 10. Bibliografía

* Docker Documentation. (n.d.). [https://docs.docker.com](https://docs.docker.com)
* React Docs. (n.d.). [https://react.dev](https://react.dev)
* Nginx Docs. (n.d.). [https://nginx.org/en/docs/](https://nginx.org/en/docs/)

