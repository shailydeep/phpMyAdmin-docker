# phpMyAdmin-docker#######################
Setting Up Docker Compose for the Project:
Now, create a project directory ~/docker/lamp (let’s say) and a html/ directory inside the project directory for keeping the website files (i.e. php, html, css, js etc.) as follows:
$ mkdir -p ~/docker/lamp/html
Now, navigate to the project directory ~/docker/lamp as follows:
$ cd ~/docker/lamp
Create a php.Dockerfile in the project directory ~/docker/lamp. This is a Dockerfile which enables mysqli and PDO php extensions in the php:7.4.3-apache image from Docker Hub and builds a custom Docker image from it.
FROM php:7.4.3-apache
docker-php-ext-install mysqli pdo pdo_mysql
Now, create a docker-compose.yaml file in the project directory ~/docker/lamp and type in the following lines in the docker-compose.yaml file.

version: "3.7"
services:
  web-server:
    build:
      dockerfile: php.Dockerfile
      context: .
    restart: always
    volumes:
      - "./html/:/var/www/html/"
    ports:
      - "8080:80"
  mysql-server:
    image: mysql:8.0.19
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: secret
    volumes:
      - mysql-data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin:5.0.1
    restart: always
    environment:
      PMA_HOST: mysql-server
      PMA_USER: root
      PMA_PASSWORD: secret
    ports:
      - "5000:80"
volumes:
  mysql-data:
  
  
Now build the dockerfile
$docker build .
and
docker-compose file
$docker-composer up -d
now take container ip by using the command
$docker container inspect ctr_id
then hit the ip in brouwser
now you can see phpmyadmin web page
