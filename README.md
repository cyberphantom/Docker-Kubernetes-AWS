# Docker-Kubernetes-AWS (For Beginners)

## Definitions:


* **Docker:**			Containarization platform.
* **CoreOS:**			OS for containers.
* **Amazon EC2:**     A web server instance hosting Ubuntu as the operating system. Check version architecture: `uname -r`. You can use SSH to connect to Ubuntu instance on Amazon EC2 `ssh -i "youe docker.pem" ubuntu@ip address`.
* **Virtual Private Cloud (VPC):** Is your private section of AWS, where you can place resources and allow or restrict access to them.

* **Kubernetes:** is a software for managing a cluster of Docker continers (scheduling, scaling, distribution workload). Kubernetes takes the software encapsulation provided by Docker further by introducing Pods. Kubernetes is lightweight, portable (suited for the cloud architecture), and modular. Kubernetes also introduces "**labels**" using which services and replication controllers (replication controller is used to scale a cluster) identify or select the containers or pods they manage.

* **Pod:** a Pod is a collection of one or more Docker containers with single interface features such as providing networking and filesystem at the Pod level rather than at the container level.
* **Amazon Relational Database Service (RDS):** Is a distributed relational database service by Amazon Web Services. It is a web service running "in the cloud" designed to simplify the setup, operation, and scalling of a relational database for use in applications.
* **Amazon S3:** Amazon simple storage service offered by Amazon web services that prvides object storage through a web service interface. 


## Introduction:
**Docker:**

* Docker Build: `docker build --tag hello-world .`
* Docker main commands: `FROM WORKDIR ENV COPY RUN CMD["exe cmd","file"]`
* List docker images: `docker images`
* Check running docker images: `docker ps`
* Check all previously run dockers: `docker ps -a`
* Docker start: `docker start $name`
* Docker stop: `docker stop $name`



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
