# Introduction to Orchestration
Reasons to use orchestration: High availability
* https://raw.githubusercontent.com/sebastian-colomar/dca/main/images/ha-failover.svg
* https://raw.githubusercontent.com/sebastian-colomar/dca/main/images/high_availability.svg
* https://raw.githubusercontent.com/sebastian-colomar/dca/main/images/rob_cam.svg

There are two well known orchestrators:
* Swarm
* Kubernetes

# Docker Compose vs Docker Swarm vs Kubernetes
1. Docker Compose is a Python script that works as wrapper of Docker commands:
    ```
    which docker-compose

    file /usr/bin/docker-compose 

    head /usr/bin/docker-compose
    ```
1. Docker Compose does not provide high availability because it is only available as a standalone host machine. If we need high availability then we need to use Docker Swarm or Kubernetes. Docker Swarm is a feature of the Docker engine that is enabled with the following commands:

    * https://labs.play-with-docker.com/

    ```
    docker swarm init --advertise-addr $( hostname -i )
    ```
1. Check the status of the cluster with the command:

    ```
    docker node ls
    ```
1. Architecture of managers and workers:

    * https://d33wubrfki0l68.cloudfront.net/2475489eaf20163ec0f54ddc1d92aa8d4c87c96b/e7c81/images/docs/components-of-kubernetes.svg
