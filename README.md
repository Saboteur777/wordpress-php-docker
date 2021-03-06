
# wordpress-php-docker

![Docker Build status](https://img.shields.io/docker/cloud/build/webmenedzser/wordpress-php.svg)
![Docker Build mode](https://img.shields.io/docker/cloud/automated/webmenedzser/wordpress-php.svg)
[![Docker layers count](https://images.microbadger.com/badges/image/webmenedzser/wordpress-php.svg)](https://microbadger.com/images/webmenedzser/wordpress-php)
![Docker Pull count](https://badgen.net/docker/pulls/webmenedzser/wordpress-php)
![Last commit](https://badgen.net/github/last-commit/Saboteur777/wordpress-php-docker)
![Keybase.io PGP](https://badgen.net/keybase/pgp/Saboteur777)

**This Docker image aims to be as simple as possible to run WordPress - if you have special dependencies, define this image as a base in your Dockerfile (FROM: webmenedzser/wordpress-php:latest) and extend it as you like.**

The image will be based on the php:fpm-alpine image, which ships the latest stable PHP.

Current PHP version is **7.3.6**

### Change user and group of PHP
You can change which user should run PHP - just build your image by extending this one, e.g.:

**Dockerfile**
```
FROM: webmenedzser/wordpress-php:latest

[...]
RUN apk add shadow && usermod -u 1000 www-data && groupmod -g 1000 www-data
[...]
```

This will change both the UID and GID of `www-data` user (which is the default to run PHP) to 1000.

### Add custom PHP settings
You can easily add new PHP settings to the image. Just place your `.ini` file in e.g. the `.docker/php` folder, and `COPY` it:

**Dockerfile**
```
FROM: webmenedzser/wordpress-php:latest

[...]
COPY .docker/php/settings-override.ini /usr/local/etc/php/conf.d/
[...]
```
Opcache is enabled by default, so if you want to disable it (for local dev) you can do it with the method mentioned above:

**disable-opcache.ini**
```
opcache.enable = 0;
```

### Example usage

**docker-compose.yml**

```
volumes:
  database_volume: {}

version: '3.6'
services:

  web:
    image: webmenedzser/wordpress-nginx:latest
    volumes:
      - ./:/var/www/

  php:
    image: webmenedzser/wordpress-php:latest
    volumes:
      - ./:/var/www/

  database:
    image: mariadb:latest
    volumes:
     - database_volume:/var/lib/mysql
```

Sister image: [wordpress-nginx](https://github.com/Saboteur777/wordpress-nginx-docker)
