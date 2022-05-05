To pull docker image
> docker pull [image_name:tag]
> docker pull redis
> docker pull redis:alpine

To run docker image and pull it if not exist
> docker run image_name:tag
> docker run hello-world

To run in detached mode
> docker run -d image_name:tag

To run and publish to port
> docker run --detach|-d --publish|-p host_port:container_port --name|-n container_name image_name:tag
> docker run -d -p 81:80 -n my_ngin nginx
 
To list running [all] containers
> docker container ls [-a]
> docker ps [-a]

To list docker local images
> docker image ls [-a]
> docker images

To remove container
> docker container rm container_name|container_id

To remove image
> docker image rm image_name:[tag]|image_id
> docker rmi image_name|image_id

To show container logs
> docker logs container_name|container_id

To show container stats (memory, cpu usage ..)
> docker stats container_name|container_id

To show low-level information on Docker objects (e.g IP Address, exposed ports)
> docker inspect container_name|container_id
> docker inspect image_name:[tag]|image_id

To information about docker images and containers
> docker info

To start/stop a container
> docker start/stop container_id|container_name

To execute command on a container
> docker exec [options] container_id|container_name command
> docker exec -it container_id|container_name bash

To Show the history of an image
> docker history image_id|image_name
(e.g.) 
> docker history redis

To tag an image
> docker tag image_id|image_name tag

(e.g.)
> docker tag nginx mo7amed3ly/myngin:dev

To push image to dockerhub
> docker login
> username:
> password:
> docker push docker_image_tag
> docker push mo7amed3ly/myngin:dev

Dockerfile
```
FROM alpine
CMD ["echo", "Hello world"]
```
To build docker image
> docker build --tag image_tag .

## Dockerfile for dotnet v6.0 web application
```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /source
COPY *.csproj .
RUN dotnet restore
COPY . .
RUN dotnet publish -c release -o /app

FROM mcr.microsoft.com/dotnet/aspnet:6.0
WORKDIR /app
COPY --from=build /app .
ENTRYPOINT [ "dotnet", "hrapp.dll" ]
```
## Dockerfile for node app
### Pre-requists
- install node
- install angular/cli (> ```npm install @angular/cli```)
- create angular app (> ```ng new helpdesk```)

```dockerfile
FROM node:lts-alpine3.15 AS build
RUN mkdir /app
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
RUN npm run build --prod

FROM nginx:alpine
COPY --from=build /app/dist/helpdesk /usr/share/nginx/html
```

To bound nginx html to a directory in the host
```bash
docker run -d -p 8081:80 -v ${pwd}/dash:/usr/share/nginx/html nginx
```
### Create Volume
```bash
docker volume create vol-name
```
### Run MariaDb with volume
```bash
 docker run -d -v vol-name:/var/lib/mysql -e MARIADB_ROOT_PASSWORD=1234 mariadb
 ```
 # Docker Networking
 ### List Networks
 ```bash
 docker network ls
 ```
 ### Inspect Network
 ```bash
 docker network inspect network_id|network_name
 ```
 ### Create Network
 ```bash
 docker network create --driver bridge --subnet x.x.x.x/x network_name
 docker network create --driver bridge --subnet 172.25.0.0/16 n1 
 ```
### Run container in network
```bash
docker run --network=n1 -d -it --name=inst1 alpine
```
### Disconnect container from network
```bash
docker network disconnect n1 inst1
```
### Connect container to network
```bash
docker network disconnect n2 inst1
```
## Docker Compose
### Create docker-compose.yml file
```yaml
version: "3"
services:
    mysql_database:
        image: mysql
        restart: always
        environment:
            MYSQL_ROOT_PASSWORD: 1234
            MYSQL_DATABASE: wp_db
            MYSQL_USER: wp_user
            MYSQL_PASSWORD: 1234
        volumes:
            - mysql:/var/lib/mysql
    wordpress:
        depends_on: 
            - mysql
        image: wordpress
        restart: always
        ports:
            - "8000:80"
        environment:
            WORDPRESS_DB_HOST: mysql_database:3306
            WORDPRESS_DB_USER: wp_user
            WORDPRESS_DB_PASSWORD: 1234
            WORDPRESS_DB_NAME: wp_db
        volumes:
            - "./:/var/www/html"
    phpmyadmin:
        depends_on: 
            - mysql_database
        image: phpmyadmin/phpmyadmin
        restart: always
        ports:
            - "8080:80"
        environment:
            PMA_HOST: mysql_database
            MYSQL_ROOT_PASSWORD: 1234
volumes:
    mysql: {}
```
