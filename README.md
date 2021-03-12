# docker_doc
## local php7.4 + apache + mysql5.7
### 1 php + apache
docker container run \
  --rm \
  --name wordpress \
  --volume "$PWD/":/var/www/html \
  -p 3001:80 \
php:7.4-apache
### 2 mysql
docker container run \
  -d \
  --rm \
  --name wordpressdb \
  --env MYSQL_ROOT_PASSWORD=123456 \
  --env MYSQL_DATABASE=wordpress \
  mysql:5.7
### 3 build image
Dockerfile
FROM php:7.4-apache
RUN docker-php-ext-install mysqli
CMD apache2-foreground

docker build -t phpwithmysql .
### 4 link mysql
$ docker container run \
  --rm \
  --name wordpress \
  --volume "$PWD/":/var/www/html \
  -p 3001:80 \
  --link wordpressdb:mysql \
  phpwithmysql
