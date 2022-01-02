**Dockerfile:**

* Docker Build: `docker build --tag hello-world .`
* Docker main commands: `FROM WORKDIR ENV COPY RUN CMD["exe cmd","file"]`
* List docker images: `docker images`
* Check running docker images: `docker ps`
* Check all previously run dockers: `docker ps -a`
* Docker start (name is container name not image name): `docker start $name`
* Docker stop: `docker stop $name`
* Docker remove: `docker rm $name`
* Docker Image remove (we need to remove image before removing container): `docker rmi $image_name`
* Docker run (8080 machine port, 80 container port, name, -d detached for long term run like webserver, name of the image): `docker run -p 8080:80 --name $name -d hello-world`
* Docker log (-f to continuously following logs): `docker log -f $name`

**docker-compose.yaml**
It is better to use docker-compose when working with micro services as starting and stoping each service will be hard to manage.

* docker-compose run: `docker-compose up -d`
* docker-compose stop: `docker-compose down`