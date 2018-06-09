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
