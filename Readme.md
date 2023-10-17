# Configuración de Docker Compose para WordPress

Este archivo de Docker Compose está diseñado para configurar un entorno de desarrollo local de WordPress junto con una base de datos MySQL. Permite ejecutar WordPress en un contenedor y utilizar una base de datos MySQL en otro contenedor.

## Servicios

### Servicio `db` (MySQL)

```
services:
   db:
     image: mysql:5.7
     ports:
       - "3307:3306"
     volumes:
       - db_data:/var/lib/mysql
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress
```
Este servicio define un contenedor MySQL con la siguiente configuración:

magen: Usamos la imagen de MySQL versión `5.7`.

Puertos: Mapea el puerto `3307` del host al puerto `3306` del contenedor MySQL.

Volúmenes: Guarda los datos de MySQL en un volumen llamado db_data para que los datos persistan incluso después de detener o eliminar el contenedor.

### Variables de entorno:

 - MYSQL_ROOT_PASSWORD: Contraseña para el usuario root de MySQL.

 - MYSQL_DATABASE: Nombre de la base de datos que se creará.

## Servicio wordpres

```
   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "8000:80"
     volumes:
       - ./html:/var/www/html
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
       WORDPRESS_DB_NAME: wordpress

```
Este servicio define un contenedor de WordPress y depende del servicio `db`. Las configuraciones son las siguientes:

Imagen: Se utiliza la imagen más reciente de WordPress.

Puertos: Mapea el puerto `8000` del host al puerto `80` del contenedor de WordPress.

Volúmenes: Monta el directorio *`./html`* del host en *`/var/www/html`* del contenedor de WordPress para permitir la personalización de archivos y temas de WordPress.

### Variables de entorno:

 - WORDPRESS_DB_HOST: Especifica el nombre del servicio MySQL `(db)` y el puerto `(3306)` al que se conectará el WordPress.

 - WORDPRESS_DB_USER: Nombre de usuario de la base de datos (coincide con el definido en el servicio db).

## Volúmenes

```
volumes:
    db_data: {}
```
- db_data: Este volumen se utiliza para almacenar los datos de la base de datos MySQL, asegurando la persistencia de los datos entre ejecuciones.