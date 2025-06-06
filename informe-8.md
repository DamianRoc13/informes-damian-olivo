# TAS8 - Despliegue backend con base de datos local

## 1. Título

Automatización del despliegue de una aplicación backend con PostgreSQL y pgAdmin usando Docker Compose.

## 2. Tiempo de duración

Aproximadamente 20 minutos para completar toda la práctica.

## 3. Fundamentos

En esta práctica se busca comprender cómo automatizar el despliegue de una aplicación backend junto con una base de datos PostgreSQL y su panel de administración pgAdmin, utilizando Docker y Docker Compose. Se parte del repositorio base:  
[https://github.com/maguaman2/tendencias-mar22-security.git](https://github.com/maguaman2/tendencias-mar22-security.git)

El objetivo es lograr un entorno local reproducible, con persistencia de datos y conectividad entre servicios, aplicando además la técnica de multi-stage builds para optimizar la imagen de la aplicación.

## 4. Conocimientos previos

Para realizar esta práctica fue necesario manejar:

* Sintaxis y lógica de `Dockerfile` y `docker-compose.yml`.
* Comandos básicos de Docker y Docker Compose.
* Conceptos de redes y volúmenes en Docker.
* Variables de entorno y archivos `.env`.
* Fundamentos de bases de datos PostgreSQL.

## 5. Objetivos a alcanzar

* Crear servicios para PostgreSQL y pgAdmin utilizando Docker Compose.
* Configurar volúmenes y redes para persistencia y conectividad.
* Construir la imagen de la aplicación backend usando multi-stage builds.
* Levantar el contenedor de la aplicación y verificar la conexión con la base de datos.
* Centralizar la configuración mediante variables de entorno.

## 6. Equipo necesario

* Computador con Docker y Docker Compose instalados.
* Proyecto backend Java (repositorio base).
* Terminal Bash o Zsh.
* Editor de código Visual Studio Code.

## 7. Material de apoyo

* Documentación oficial de Docker y Docker Compose.
* Documentación de PostgreSQL y pgAdmin.

## 8. Procedimiento

### Paso 1: Crear el archivo `docker-compose.yml`

Se definieron los servicios de PostgreSQL y pgAdmin, junto con sus volúmenes y red personalizada, tambien el backend:

```yaml
version: 
services:
  postgres:
    image: postgres:15
    container_name: postgres_db
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: admin123
      POSTGRES_DB: app_db
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - backend_net

  pgadmin:
    image: dpage/pgadmin4
    container_name: pgadmin
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@admin.com
      PGADMIN_DEFAULT_PASSWORD: admin123
    ports:
      - "8080:80"
    depends_on:
      - postgres
    networks:
      - backend_net

networks:
  backend_net:
    driver: bridge

volumes:
  postgres_data:
```

### Paso 2: Verificar la conexión entre pgAdmin y PostgreSQL

Se accedió a [http://localhost:8080](http://localhost:8080) y se configuró la conexión a la base de datos usando los datos de las variables de entorno.

<p align="center"> <img src="img/informe-8/Captura de Pantalla 2025-06-06 a la(s) 9.24.17 a.m..png" alt="App " width="800px"> </p>

### Paso 3: Crear el Dockerfile multi-stage para la aplicación backend

Se implementó un `Dockerfile` multi-stage para compilar y empaquetar la aplicación Java:

```dockerfile
FROM maven:3.9.6-eclipse-temurin-21 AS build
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

FROM eclipse-temurin:21-jre
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### Paso 4: Agregar el servicio backend al `docker-compose.yml`

Se añadió el servicio backend, asegurando la conexión con PostgreSQL:

```yaml
  backend:
    build: .
    container_name: backend_app
    environment:
      DB_SERVER: postgres
      DB_PORT: 5432
      DB_NAME: app_db
      DB_USER: admin
      DB_PASSWORD: admin123
    depends_on:
      - postgres
    ports:
      - "8081:8081"
    networks:
      - backend_net
```
<p align="center"> <img src="img/informe-8/Captura de Pantalla 2025-06-06 a la(s) 10.32.07 a.m..png" alt="App " width="800px"> </p>

### Paso 5: Crear el archivo `.env`

Se centralizaron las variables sensibles y de configuración:

```
POSTGRES_USER=admin
POSTGRES_PASSWORD=admin123
POSTGRES_DB=app_db
PGADMIN_DEFAULT_EMAIL=admin@admin.com
PGADMIN_DEFAULT_PASSWORD=admin123
```

### Paso 6: Desplegar los servicios

Desde la terminal, se ejecutó:

```bash
docker-compose up --build
```
<p align="center"> <img src="img/informe-8/Captura de Pantalla 2025-06-06 a la(s) 8.55.21 a.m..png" alt="App " width="800px"> </p>
Se verificó el acceso a:

- Backend: [http://localhost:8081](http://localhost:8081)

<p align="center"> <img src="img/informe-8/Captura de Pantalla 2025-06-06 a la(s) 10.39.19 a.m..png" alt="App " width="800px"> </p>

- pgAdmin: [http://localhost:8080](http://localhost:8080)

<p align="center"> <img src="img/informe-8/Captura de Pantalla 2025-06-06 a la(s) 10.38.13 a.m..png" alt="App " width="800px"> </p>

## 9. Resultados esperados

- El backend se ejecuta correctamente y se conecta a la base de datos PostgreSQL.
- pgAdmin permite administrar la base de datos desde el navegador.
- Los datos de la base persisten gracias al volumen configurado.
- El tamaño de la imagen backend es óptimo gracias al multi-stage build.

## 10. Investigación

Los Multi-Stage Builds en Docker son una técnica que permite dividir la construcción de imágenes en etapas independientes, donde cada etapa puede usar una imagen base diferente y solo los archivos esenciales se copian a la etapa final. Esto reduce significativamente el tamaño de la imagen resultante al eliminar herramientas de compilación y archivos temporales, mejora la seguridad al minimizar componentes innecesarios y optimiza el uso de la caché durante el proceso de construcción.

En mi implementación, utilicé dos etapas claramente diferenciadas: una primera etapa con Maven y el JDK para compilar el proyecto Java y generar el archivo JAR, y una segunda etapa que solo incluye el JRE para ejecutar la aplicación. Mediante el comando COPY --from, transferí únicamente el JAR compilado a la imagen final, descartando todo el entorno de compilación. Este enfoque garantiza que la imagen de producción sea más liviana y segura, ya que no contiene herramientas ni dependencias que solo son necesarias durante la fase de construcción.

## 11. Bibliografía

* Docker Documentation. [https://docs.docker.com](https://docs.docker.com)
* PostgreSQL Docs. [https://www.postgresql.org/docs/](https://www.postgresql.org/docs/)
* pgAdmin Docs. [https://www.pgadmin.org/docs/](https://www.pgadmin.org/docs/)