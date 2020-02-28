# laravel-php-fpm

### docker-compose.yml
```
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
```

### nginx.conf
```
server {
  index index.php index.html;
  server_name _;
  root /var/www/public;
  error_log  /var/log/nginx/error.log;
  access_log /var/log/nginx/access.log;

  add_header X-Frame-Options "SAMEORIGIN";
  add_header X-XSS-Protection "1; mode=block";
  add_header X-Content-Type-Options "nosniff";

  index index.html index.htm index.php;

  charset utf-8;

  location / {
    try_files $uri $uri/ /index.php?$query_string;
  }

  location = /favicon.ico { access_log off; log_not_found off; }
  location = /robots.txt  { access_log off; log_not_found off; }

  error_page 404 /index.php;

  location ~ \.php$ {
    try_files $uri =404;
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_pass php:9000;
    fastcgi_index index.php;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param PATH_INFO $fastcgi_path_info;
  }

  location ~ /\.(?!well-known).* {
    deny all;
  }
}
```