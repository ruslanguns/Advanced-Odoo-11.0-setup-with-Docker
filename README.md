# Instalación avanzada de Odoo con linea de comandos y con compose 

En este repo reuno un par de archivos un docker-compose.yml para la instalación personalizada de Odoo con una base de datos personalizada. Esta configuración la hice fusionando varios tutoriales que vi por ahí y lo que me permite es tener volúmenes personalizados para una gestión más sencilla del desarrollo en Odoo.  

## Motivos por el cuál recomiendo este tipo de configuración

1. Localización de volúmenes.

2. Porque puedes tener varias instancias de Odoo con una base de datos en odoo distinta, evitas errores y tienes un funcionamiento más estable sin arriesgar en lo absoluto tus otras unidades en un mismo servidor.

3. Network independiente

4. Para startups y quienes no manejan Odoo ni Linux.

## Versión considerad

#### Para Odoo
Esta configuración la hice con el Odoo en su última versión odoo:latest nighly la que tiene mayor soporte y constantemente tiene corrección de errores, sin embargo si deseas usar una versión anterior basta que agregues un valor determinado según la guía oficial en:

> [Hub Odoo Oficial](https://hub.docker.com/_/odoo/)

En otras palabras debes nada más agregar tu versión ejemplo odoo:9.0 ó odoo:10.0 y listo.

#### Para Postgress

Se utiliza la versión 9.4 la cuál es la más recomendada, no me pregunten por qué.


### Métodos de instalar Odoo

En este repositorio estoy compartiendo dos formas que instalan exactamente lo mismo, la diferencia es la forma de configurarlo.

## Antes de comenzar

A continuación se utilizarán estos datos para la configuración que puedes cambiar según tus necesidades

- database: mi-demo-db

- user: mi-demo

- pass: mi-contraseña

- puerto: 8070

## [Linea de comandos](/odoo-command-line.md)
Solo hay que Buscar y reemplazar el nombre de usuario mi-demo por el usuario
Por defecto esta configurado --restart=always para que inicie en boot

- Creamos una red
> $ docker network create --driver bridge mi-demo-nw  #network

- Creamos un volumen para la base de datos

> $ docker volume create --name mi-demo-db  #volumen

### Instalación completa postgres:9.4 "la más estable"

> $ docker run -d \
 --name mi-demo-db \
 -e POSTGRES_USER=mi-demo\
 -e POSTGRES_PASSWORD=mi-contraseña \
 -e PGDATA=/var/lib/postgresql/data/pgdata \
 --restart=always \
 --network=mi-demo-nw \
 --mount source=mi-demo-db,target=/var/lib/postgresql/data/pgdata \
 postgres:9.4

- Para almacenar los addons

> $ docker volume create --name mi-demo-addons  

- Para almacenar la el odoo.conf

> $ docker volume create --name mi-demo-config  

- Para almacenar la data

> $ docker volume create --name mi-demo-data

### Instalación completa odoo:latest versión nightly

> $ docker run --name mi-demo --link mi-demo-db:db -p 8070:8069 \
 --network mi-demo-nw \
 --mount source=mi-demo-addons,target=/mnt/extra-addons \
 --mount source=mi-demo-data,target=/var/lib/odoo \
 --mount source=mi-demo-config,target=/etc/odoo \
 --restart=always \
 --env POSTGRES_USER=mi-demo \
 --env POSTGRES_PASSWORD=mi-contraseña \
 -t odoo

- Es bueno reiniciar el contenedor odoo después de la primer instación

> $ docker restart mi-demo  

### Configuración terminada

- Puedes entrar a la siguiente dirección:
> http://localhost:8070


## [Mediante docker-compose](/docker-compose.yml)
- Si deseas hacer esto mismo pero aún todavía mas fácil, te recomiendo utilizar el docker-compose.yml que lo encontrarás adjunto en [este repositorio](/docker-compose.yml).

Para que pueda funcionar debes tener antes instalado el Docker Composer

> $ curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` > ./docker-compose

> $ sudo mv ./docker-compose /usr/bin/docker-compose

> $ sudo chmod +x /usr/bin/docker-compose

Espero que esto haya servido de algo, si alguien desea ayudarme a mejorar este repositorio adelante.
