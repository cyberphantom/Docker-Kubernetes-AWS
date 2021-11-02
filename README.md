# Kubernetes-Docker

## Introduction:

* Docker:			Containarization platform.
* Kubernetes:		Container cluster manager (Docker and rkt containers).
* CoreOS:			OS for containers.

## Definitions:

**Kubernetes:** is a software for managing a cluster of Docker continers (scheduling, scaling, distribution workload). Kubernetes takes the software encapsulation provided by Docker further by introducing Pods. Kubernetes is lightweight, portable (suited for the cloud architecture), and modular.

**Pod:** a Pod is a collection of one or more Docker containers with single interface features such as providing networking and filesystem at the Pod level rather than at the container level.

Kubernetes also introduces "**labels**" using which services and replication controllers (replication controller is used to scale a cluster) identify or select the containers or pods they manage.   
