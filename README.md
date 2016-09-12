# nginx-auto-https-docker
The main goal is to have a drop in project that will automatically generate SSL certificates and protect your docker exposed containers with SSL

This project havily relies on [nginx-proxy](https://github.com/jwilder/nginx-proxy), [docker-gen](https://github.com/jwilder/docker-gen) and [letsencrypt-nginx-proxy-companion](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion).

## Running

Just clone this repo and do a `docker-compose up -d` Everything should be working

## Modifying
Checkout the [docker-compose](./docker-compose.yml) file to see some options that can be changed for your setup

## Wordpress

If you want to run a wordpress with this setup, we recommend the official image from docker, a sample entry in a docker-compose that would work with this setup:

```yml
wordpress:
  image: wordpress
  restart: always
  links:
    - mysql
  environment:
    - VIRTUAL_HOST=example.com,www.example.com #the domain of your app, any valid comma separated domains are accepted
    - LETSENCRYPT_HOST=example.com,www.example.com #the domain of your app, any valid comma separated domains are accepted
    - LETSENCRYPT_EMAIL=noreply@example.com
  volumes:
    - ~/data/wordpress/:/var/www/html # You change change where you want to store your wordpress data
  expose:
    - 80
    - 443
mysql:
  restart: always
  image: mariadb:5.5.40
  volumes:
    - ~/data/mysql/:/var/lib/mysql/
  environment:
    - MYSQL_ROOT_PASSWORD=SomeHardToGuessPassword
    - MYSQL_DATABASE=my-wordpress
    - MYSQL_USER=my-user
    - MYSQL_PASSWORD=SomeHardToGuessPassword
  expose:
    - "3306"
```
