# Kubernetes-Docker

## Introduction:

* Docker:			Containarization platform.
* Kubernetes:		Container cluster manager (Docker and rkt containers).
* CoreOS:			OS for containers.

## Definitions:

**Kubernetes:** is a software for managing a cluster of Docker continers (scheduling, scaling, distribution workload). Kubernetes takes the software encapsulation provided by Docker further by introducing Pods. Kubernetes is lightweight, portable (suited for the cloud architecture), and modular. Kubernetes also introduces "**labels**" using which services and replication controllers (replication controller is used to scale a cluster) identify or select the containers or pods they manage.

**Pod:** a Pod is a collection of one or more Docker containers with single interface features such as providing networking and filesystem at the Pod level rather than at the container level.

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

## Push your image to Amazon Elastic Container Registry

1. Prepare or create your docker image and test it in your local machine or the connected instance.
2. Create an Amazon ECR repository to store your image. `aws ecr create-repository --repository-name hello-repository --region region`
