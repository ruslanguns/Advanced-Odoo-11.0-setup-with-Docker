# Basic setup de Odoo en Linux con Docker

Te daré una corta guía para que veas lo fácil instalar Odoo desde cero:

## 1. INSTALAR DOCKER

    > sudo apt-get update 
    
    > sudo apt-get install -y docker  

> ### Ubuntu 18.04
> sudo apt update

> sudo apt install -y docker.io

    sudo groupadd docker
    sudo usermod docker $USER
    sudo usermod -a -G docker
    sudo service docker start

## 2. INSTALAR ODOO MODO BÁSICO


    sudo apt-get update *(para Ubuntu 18.04 es " sudo apt update ")  


  a. Instalar Posgres


    docker run -d -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo --restart=always --name db postgres:9.4


  b. Instalar Odoo


    docker run -p 8069:8069 --name odoo --restart=always --link db:db -t odoo


ESTO ES TODO, ingresa a tu IP con puerto 8069, si este puerto ya esta ocupado, te recomiendo cambiar el 8086:8086 (ejemplo: 8070:8086)


    http://localhost:8069 ó al puerto que haz configurado.
