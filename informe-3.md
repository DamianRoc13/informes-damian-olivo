# TAS3 - Persistencia de datos en PostgreSQL con Docker

## 1. Título  
Creación y manejo de contenedores Docker con PostgreSQL: comparativa de comportamiento con y sin volúmenes.

## 2. Tiempo de duración  
Aproximadamente 20 minutos para la práctica.

## 3. Fundamentos  
PostgreSQL es un sistema de gestión de bases de datos relacional avanzado. Docker permite empaquetar aplicaciones en contenedores, pero por defecto los datos son efímeros. En esta práctica exploramos cómo los volúmenes Docker permiten persistir información de bases de datos incluso al eliminar contenedores.

## 4. Conocimientos previos  
- Comandos básicos de Docker (run, stop, rm, volume)  
- Conceptos de contenedores y persistencia de datos  
- Conexión a PostgreSQL con clientes gráficos  
- Sintaxis SQL básica (CREATE, INSERT, SELECT)  

## 5. Objetivos a alcanzar  
- Crear contenedores PostgreSQL con y sin volúmenes  
- Demostrar la pérdida/persistencia de datos al recrear contenedores  
- Manejar volúmenes Docker para gestión de bases de datos  

## 6. Equipo necesario  
- Computador con Docker instalado  
- Cliente PostgreSQL (TablePlus o similar)  
- Terminal para ejecutar comandos  

## 7. Material de apoyo  
- Documentación oficial de Docker Volumes  
- Cheatsheet de comandos PostgreSQL  

## 8. Procedimiento  

### Parte 1: Sin volumen  

1. Crear contenedor PostgreSQL:  
```
docker run --name server_db1 -e POSTGRES_PASSWORD=cisco -d -p 5432:5432 damian
```
2. Conectar con TablePlus (usuario: damian, contraseña: cisco)

3. Ejecutar SQL:  
```sql
CREATE DATABASE test;
CREATE TABLE customer (id SERIAL PRIMARY KEY, fullname VARCHAR(100), status VARCHAR(50));
INSERT INTO customer (fullname, status) VALUES ('Damian Olivo', 'Activo');
```
### Eliminar contenedor:
```
docker stop server_db1 && docker rm server_db1
```
Recrear contenedor y verificar pérdida de datos.

### Parte 2: Con volumen
Crear volumen:
```
docker volume create pgdata
```
Crear contenedor con volumen:
```
docker run --name server_db2 -v pgdata:/var/lib/postgresql/data -e POSTGRES_PASSWORD=cisco -d -p 5433:5432 damian
```

Se repite la creación de base de datos, tabla e inserción de datos.

Eliminar y recrear contenedor usando el mismo volumen, verificando persistencia.

## 9. Resultados esperados
Sin volumen: Los datos se pierden completamente al eliminar el contenedor.

Con volumen: Toda la información persiste tras recrear el contenedor.

## 10. Bibliografía

Docker Documentation. (2023). Manage data in Docker

PostgreSQL. (2023). Official Documentation