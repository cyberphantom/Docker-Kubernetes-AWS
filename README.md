# Docker-Kubernetes-AWS (For Beginners)
Now adays, most development companies are putting these three kind of tools as a must. Therefore, I will try to cover in short the most useful and basic commands in each of Docker, Kubernetes, and AWS EC2. It will act as an introductory and reference for begineeres or students who looking forward to learn these basic instructions with minimal time.



## Some Definitions:


* **Docker:**			Containarization platform.
* **Container:**     is a sandbox process on your machine that is isolated from all other processes on the host machine. It is a runable instance of an image. In general, each container should do one thing and do it well.
* **Networking:**   allow one container to talk to another. 
* **Docker Compose:** is a tool that was developed to help define and share multi-container applications.
* **CoreOS:**			OS for containers.
* **Amazon EC2:**     A web server instance hosting Ubuntu as the operating system. Check version architecture: `uname -r`. You can use SSH to connect to Ubuntu instance on Amazon EC2 `ssh -i "youe docker.pem" ubuntu@ip address`.
* **Virtual Private Cloud (VPC):** Is your private section of AWS, where you can place resources and allow or restrict access to them.

* **Kubernetes:** is a software for managing a cluster of Docker continers (scheduling, scaling, distribution workload). Kubernetes takes the software encapsulation provided by Docker further by introducing Pods. Kubernetes is lightweight, portable (suited for the cloud architecture), and modular. Kubernetes also introduces "**labels**" using which services and replication controllers (replication controller is used to scale a cluster) identify or select the containers or pods they manage.

* **Pod:** a Pod is a collection of one or more Docker containers with single interface features such as providing networking and filesystem at the Pod level rather than at the container level.
* **Amazon Relational Database Service (RDS):** Is a distributed relational database service by Amazon Web Services. It is a web service running "in the cloud" designed to simplify the setup, operation, and scalling of a relational database for use in applications.
* **Amazon S3:** Amazon simple storage service offered by Amazon web services that prvides object storage through a web service interface. 


## [Docker Quick Start](https://github.com/cyberphantom/Docker-Kubernetes-AWS/blob/main/docker.md)

**Dockerfile: FROM WORDIR ENV COPY RUN CMD**

*Ex:*
`Dockerfile`

      FROM node:12.16.3 `must start with FROM`
      WORKDIR /code     `Specify working Dir`
      ENV PORT 80       `In ENV, specify port of the container`
      COPY package.json /code/package.json   `Copy dependencies before installing`
      RUN npm install   `RUN then exe LINUX command to install npm`
      COPY . /code      `Copy all files to the container image. Except what is mentioned in .gitignore file`
      CMD ["node", "src/server.js"]    `run the server with command`

* Docker Build (Build an image from Dockerfile. name it whatever after --tag with the file located in . which is current directory): `docker build --tag hello-world .`
* Docker run Image: `docker run -p 8080:80 --name hello -d hello-world` another example
`docker run -d -p 80:80 docker/getting-started`
* Docker main commands: `FROM WORKDIR ENV COPY RUN CMD["exe cmd","file"]`
* List docker images: `docker images`
* Check running docker images: `docker ps`
* Check all previously run dockers: `docker ps -a`
* Docker start a container (name is container name not image name): `docker start $name`
* Docker stop a container: `docker stop $name`
* Docker remove a container: `docker rm $name`
* Docker Image remove (we need to remove image before removing container): `docker rmi $image_name`
* Docker run (8080 machine port, 80 container port, name, -d detached for long term run like webserver, name of the image): `docker run -p 8080:80 --name $name -d hello-world`
* Docker logs (-f to continuously following logs): `docker logs -f $name`

**docker-compose.yml (version volumes services)**
It is better to use docker-compose when working with micro services as starting and stoping each service will be hard to manage.

*Ex:*
`docker-compose.yml`

      version: '3'
      services:
         web:
            build:
               context:
               dockerfile: Dockerfile
            container_name: web
            ports:
               - "8080:80"

         db:
            image: mongo:3.6.1
            container_name: db
            volumes:
               - mongodb:/data/db
               - mongodb_config:/data/configdb
            ports:
               - 27017:27017
            command: mongod

      volumes:
         mongodb:
         mongodb_config:




* docker-compose run: `docker-compose up -d`
* docker-compose stop: `docker-compose down`

## To install Docker on an Amazon EC2 instance [AWS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html).

1. Launch an instance with the Amazon Linux 2 or Amazon Linux AMI.
2. Connect to your instance.
3. Update the installed packages and package cache on your instance. `sudo yum update -y`
4. Install the most recent Docker Engine package.
   Amazon Linux 2: `sudo amazon-linux-extras install docker`
   Amazon Linux: `sudo yum install docker`
5. Start the Docker service. `sudo service docker start`
6. Add the `ec2-user` to the `docker` group so you can execute Docker commands without using `sudo`. `sudo usermod -a -G docker ec2-user`
7. Log out and log back in again to pick up the new `docker` group permissions. You can accomplish this by closing your current SSH terminal window and reconnecting to your instance in a new one. Your new SSH session will have the appropriate `docker` group permissions. 
8. Verify that the `ec2-user` can run Docker commands without `sudo`. `docker info`

## Push your image to Amazon Elastic Container Registry (ECR)

1. Prepare or create your docker image and test it in your local machine or the connected instance.
2. To use Amazon ECR, first install AWS [CLI](https://docs.aws.amazon.com/AmazonECR/latest/userguide/get-set-up-for-amazon-ecr.html).
3. Create youe IAM user Administrator.
4. [aws configure](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html). You can create a named profile using `aws configure --profile produser`
5. [Authenticate the Docker CLI to your default registry](https://docs.aws.amazon.com/AmazonECR/latest/userguide/getting-started-cli.html) in Step 2.
6. get-login-password `aws ecr get-login-password --region region | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com`. The password will be stored unencrypted in /home/ec2-user/.docker/config.json.
7. Create an Amazon ECR repository to store your image. `aws ecr create-repository --repository-name hello-repository --region region`.
8. Tag your image to push to your repository. `docker tag hello-world:latest aws_account_id.dkr.ecr.region.amazonaws.com/hello-world:latest`.
9. Push the images `docker push aws_account_id.dkr.ecr.region.amazonaws.com/hello-world:latest` [step4](https://docs.aws.amazon.com/AmazonECR/latest/userguide/getting-started-cli.html).
10. To avoid charge, clean up your repository. `aws ecr delete-repository --repository-name hello-repository --region region --force`
