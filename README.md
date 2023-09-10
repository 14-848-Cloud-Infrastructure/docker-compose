# Docker Workshop
Lab : Docker Compose

---

## Install docker-compose

 - install docker-compose (if you already don't have it):
```
sudo apt install docker-compose
```

## Preparations

 - Create a new folder for the lab:
```
mkdir docker-compose-lab
cd docker-compose-lab
```

## Instructions

- Create a **"docker-compose.yml"** file with the content below:

```
version: '3.3'

services:
   db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: admin
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: admin
       WORDPRESS_DB_PASSWORD: wordpress

volumes:
    db_data:
```
 
 - Start all the environment in detached mode:

```
 docker-compose up -d
```
 
 - Wait until docker complete to start the containers, meanwhile let's inspect the wordpress logs:

```
docker-compose logs -f wordpress
```

 - Check the running containers:

```
docker-compose ps
```

 - Browse to the wordpress portal:
 
```
http://<Your-Server-IP>:8000
```
  
 - Enter to the mysql container:
   
```
docker-compose exec db bin/bash
```

 - Print the environment variable **"MYSQL_USER"** (passed in the yaml file):
   
```
echo $MYSQL_USER
```

 - Exit from the container
```
exit
```
  
 - Update the mysql image used in the **docker-compose.yml** file to "5.7.24":
```
image: mysql:5.7.24
```
```
db:
     image: mysql:5.7.24
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: admin
       MYSQL_PASSWORD: wordpress
```

 - Apply the changes to the running environment:

```
docker-compose up -d
```
  
 - Let's clean our environment (remove the containers defined in the docker-compose file):

```
docker-compose down
```

 - Note that volumes are not deleted by default, so you can recreate the environment at any time

```
docker volume ls
```

 - To remove the volumes as well you can use "docker-compose down --volume"
 
 
