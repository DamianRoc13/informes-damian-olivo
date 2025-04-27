# TAS4 - Red de contenedores mysql y phomyadmin

## 1. Título  
Implementación de una base de datos MySQL e interfaz phpMyAdmin mediante contenedores Docker conectados.

## 2. Tiempo de duración  
Aproximadamente 30 minutos para completar toda la práctica.

## 3. Fundamentos  

Docker es una plataforma que permite crear, desplejar y gestionar aplicaciones en contenedores. En esta práctica, se configurarán dos contenedores:  
- **MySQL**: Servidor de base de datos relacional.  
- **phpMyAdmin**: Interfaz gráfica para administrar MySQL.  

La conexión entre ambos se realizaró mediante una red personalizada en Docker, lo que garantiza comunicación aislada y segura. Esto es fundamental para entornos de desarrollo.  

## 4. Conocimientos previos  

Para realizar esta práctica fue necesario manejar:  
- Comandos básicos de Docker (`run`, `network`, `exec`).  
- Conceptos de redes en Docker.  
- Credenciales y variables de entorno en contenedores.  
- Navegación por terminal.  

## 5. Objetivos a alcanzar  

- Crear y conectar contenedores Docker para MySQL y phpMyAdmin.  
- Configurar una red personalizada en Docker.  
- Verificar la conexión mediante la creación de una base de datos de prueba.  

## 6. Equipo necesario  

- Computador con Docker instalado en mi caso, macOS.  
- Terminal de comandos Zsh.  

## 7. Material de apoyo  

- Documentación oficial de Docker: https://docs.docker.com/  
- Manual de MySQL: https://dev.mysql.com/doc/  

## 8. Procedimiento  

### Paso 1: Crear la red personalizada en Docker  

```
docker network create mysql-network
```

### Paso 2: Configurar el contenedor de MySQL
```
docker run -d \
  --name mysql-container \
  -e MYSQL_ROOT_PASSWORD=cisco \
  -e MYSQL_DATABASE=prueba \
  -e MYSQL_USER=damian \
  -e MYSQL_PASSWORD=cisco \
  -p 3306:3306 \
  --network mysql-network \
  mysql:latest
```
### Paso 3: Configurar el contenedor de phpMyAdmin
```
docker run -d \
  --name phpmyadmin-container \
  -e PMA_HOST=mysql-container \
  -e PMA_USER=damian \
  -e PMA_PASSWORD=cisco \
  -p 8080:80 \
  --network mysql-network \
  phpmyadmin:latest
```
### Paso 4: Verificar contenedores en ejecución
```
docker ps
```
### Paso 5: Acceder a phpMyAdmin
Abri en el navegador: http://localhost:8080

### Paso 6: Crear base de datos de prueba
En phpMyAdmin, clic en "Nueva" esta a la izquierda:

Nombrar la base.

Clic en "Crear"

Figura 1.1. Conexión exitosa en phpMyAdmin

<p align="center"> <img src="img/informe-4/Captura de Pantalla 2025-04-27 a la(s) 12.28.55 p.m..png" alt="" width="800px"> </p>

## 9. Resultados esperados

#### Figura 1.2. Contenedores funcionando y conectados

<p align="center"> <img src="img/informe-4/Captura de Pantalla 2025-04-27 a la(s) 11.54.41 a.m..png" alt="" width="800px"> </p>

#### Figura 1.3. Acceso a phpMyAdmin via localhost:8080 (login)

<p align="center"> <img src="img/informe-4/Captura de Pantalla 2025-04-27 a la(s) 12.21.51 p.m..png" alt="" width="800px"> </p>

#### Figura 1.4. Base de datos verificada creada

<p align="center"> <img src="img/informe-4/Captura de Pantalla 2025-04-27 a la(s) 12.37.42 p.m..png" alt="" width="800px"> </p>


## 10. Bibliografía
Docker Documentation. (n.d.). Networking in Docker. https://docs.docker.com/network/

MySQL. (n.d.). Official MySQL Docker Image. https://hub.docker.com/_/mysql