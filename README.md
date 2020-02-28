# laravel-php-fpm

version: "3"
services:
  php:
    image: czbas23/laravel-php-fpm:latest
    volumes:
      - ./:/var/www
  nginx:
    image: nginx:latest
    depends_on:
      - php
    ports:
      - "8000:80"
    volumes:
      - ./:/var/www
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
