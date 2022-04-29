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
