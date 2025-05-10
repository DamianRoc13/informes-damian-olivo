# TAS6 - Wordpress con docker compose YML

## 1. Título  
Configuración de WordPress con PostgreSQL y pgAdmin usando Docker Compose.

## 2. Tiempo de duración  
Aproximadamente 60 minutos para completar toda la práctica.

## 3. Fundamentos  

Docker Compose es una herramienta que permite definir y ejecutar aplicaciones multicontenedor. En esta práctica, se configurarán tres servicios:  
- **PostgreSQL**: Base de datos relacional para WordPress.  
- **pgAdmin**: Interfaz gráfica para administrar PostgreSQL.  
- **WordPress**: Plataforma para la creación de sitios web.  

La conexión entre los servicios se realizará mediante una red personalizada en Docker Compose, lo que garantiza comunicación aislada y segura.

## 4. Conocimientos previos  

Para realizar esta práctica fue necesario manejar:  
- Sintaxis y configuración de archivos `docker-compose.yml`.  
- Conceptos de redes y volúmenes en Docker.  
- Configuración de variables de entorno en contenedores.  
- Navegación por terminal.  

## 5. Objetivos a alcanzar  

- Configurar un entorno WordPress utilizando PostgreSQL como base de datos.  
- Administrar PostgreSQL mediante pgAdmin.  
- Verificar la conexión y funcionalidad del sitio WordPress.  

## 6. Equipo necesario  

- Computador con Docker y Docker Compose instalados (en este caso, macOS).  
- Terminal de comandos Zsh.  

## 7. Material de apoyo  

- Documentación oficial de Docker Compose: https://docs.docker.com/compose/  
- Manual de PostgreSQL: https://www.postgresql.org/docs/  
- Documentación de WordPress: https://wordpress.org/support/  

## 8. Procedimiento  

### Paso 1: Crear el archivo `docker-compose.yml`  

Creamos un archivo llamado `docker-compose.yml` con el siguiente contenido:

```yaml
version: '3.8'

services:
  db:
    image: postgres:13
    container_name: postgres_db
    environment:
      POSTGRES_USER: damian
      POSTGRES_PASSWORD: DamianDB2024
      POSTGRES_DB: wordpress
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - wordpress_net
    restart: unless-stopped


  pgadmin:
    image: dpage/pgadmin4
    container_name: pgadmin
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@damian.com
      PGADMIN_DEFAULT_PASSWORD: PgAdmin2024
    volumes:
      - pgadmin_data:/var/lib/pgadmin
    ports:
      - "8080:80"
    networks:
      - wordpress_net
    restart: unless-stopped

  
  wordpress:
    image: wordpress:latest
    container_name: wordpress
    depends_on:
      - db
    environment:
      WORDPRESS_DB_HOST: db:5432
      WORDPRESS_DB_USER: damian
      WORDPRESS_DB_PASSWORD: DamianDB2024
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - wordpress_data:/var/www/html
    ports:
      - "8000:80"
    networks:
      - wordpress_net
    restart: unless-stopped


volumes:
  postgres_data:
  pgadmin_data:
  wordpress_data:


networks:
  wordpress_net:
    driver: bridge
```

<p align="center"> <img src="img/informe-6/Captura de Pantalla 2025-05-10 a la(s) 11.05.43 a.m..png" alt="" width="800px"> </p>

### Paso 2: Ejecutar los servicios  

Ejecutamos el siguiente comando:

```bash
docker-compose up -d
```

<p align="center"> <img src="img/informe-6/Captura de Pantalla 2025-05-10 a la(s) 10.53.44 a.m..png" alt="" width="800px"> </p>

### Paso 3: Verificar contenedores en ejecución  

Ejecutamos
 el siguiente comando para verificar que los contenedores están funcionando:

```bash
docker ps
```

<p align="center"> <img src="img/informe-6/Captura de Pantalla 2025-05-10 a la(s) 11.18.40 a.m..png" alt="" width="800px"> </p>

### Paso 4: Acceder a los servicios  

- **WordPress**: [http://localhost:8000](http://localhost:8000)  
- **pgAdmin**: [http://localhost:8080](http://localhost:8080)  

## 9. Resultados esperados  

#### Figura 1.1. Contenedores funcionando   

<p align="center"> <img src="img/informe-6/Captura de Pantalla 2025-05-10 a la(s) 11.10.09 a.m..png" alt="" width="800px"> </p>

#### Figura 1.2. Acceso a pgAdmin via localhost:8080  

<p align="center"> <img src="img/informe-6/Captura de Pantalla 2025-05-10 a la(s) 11.05.28 a.m..png" alt="" width="800px"> </p>

## 10. Credenciales usadas en el codigo:
PostgreSQL: damian | DamianDB2024    
pgAdmin: admin@damian.com | PgAdmin2024 

## 11. Bibliografía  
- Docker Compose Documentation. (n.d.). https://docs.docker.com/compose/  
- PostgreSQL Documentation. (n.d.). https://www.postgresql.org/docs/  
- WordPress Documentation. (n.d.). https://wordpress.org/documentation/  