# Let us create the container:
```
mkdir --parents ${HOME}/phpinfo/

tee ${HOME}/phpinfo/index.php 0<<EOF

<?php
phpinfo();
?>

EOF

docker network create phpinfo-network --driver bridge

docker run --cpus 0.100 --detach --env AUTHOR=Sebastian --memory 100M --memory-reservation 100M --name phpinfo-container --network phpinfo-network --read-only --restart always --user nobody:nogroup --volume ${HOME}/phpinfo/index.php:/data/index.php:ro --workdir /data/ docker.io/library/php:alpine php -f index.php -S 0.0.0.0:8080

docker ps | grep phpinfo-container
```
# How to connect to the container network
The Docker container is running but only accessible from inside:
```
$ docker exec phpinfo-container wget localhost:8080 -O - -q --spider -S
  HTTP/1.1 200 OK
  Host: localhost:8080
  Date: Sat, 15 Oct 2022 14:46:11 GMT
  Connection: close
  X-Powered-By: PHP/8.0.24
  Content-type: text/html; charset=UTF-8
  
$ wget localhost:8080 -O - -q --spider -S
$ 
```
In order to connect from outside I need to publish a Node Port from the host machine mapped into the container network:
```
docker rm phpinfo-container --force

docker run --cpus 0.100 --detach --env AUTHOR=Sebastian --memory 100M --memory-reservation 100M --name phpinfo-container --network phpinfo-network --publish 8080 --read-only --restart always --user nobody:nogroup --volume ${HOME}/phpinfo/index.php:/data/index.php:ro --workdir /data/ 172.31.10.220/library/php:alpine php -f index.php -S 0.0.0.0:8080

docker exec phpinfo-container wget localhost:8080 -O - -q --spider -S

NODE_PORT=$( docker port phpinfo-container | grep --max-count=1 tcp | cut --delimiter : --field 2 )

wget localhost:${NODE_PORT} -O - -q --spider -S
```
It is recommended to keep the Node Port as a random value but if you want to fix the Node Port then you can do that with the following command:
```
docker rm phpinfo-container --force

NODE_PORT=30000

docker run --cpus 0.100 --detach --env AUTHOR=Sebastian --memory 100M --memory-reservation 100M --name phpinfo-container --network phpinfo-network --publish ${NODE_PORT}:8080 --read-only --restart always --user nobody:nogroup --volume ${HOME}/phpinfo/index.php:/data/index.php:ro --workdir /data/ docker.io/library/php:alpine php -f index.php -S 0.0.0.0:8080

docker exec phpinfo-container wget localhost:8080 -O - -q --spider -S

wget localhost:${NODE_PORT} -O - -q --spider -S
```
# Docker networking
There are three main kind of network drivers in Docker:
* bridge
* host
* null

The default network driver for Docker is bridge.
Any Docker installation will create a default bridge network also called "bridge".
Any container will be attached to that network by default.
That means that any container will be able to talk to any other container in that bridge network (by default).
Therefore, for security reasons, it is better not to use the default network.
The recommended practice is to always create a custom bridge for your containers:
```
docker network create phpinfo-network --driver bridge
```
Only containers in the same network will be able to talk to each other.
# Bridge Docker network
It is the default Docker network.
Docker will create a default bridge network after first installation.
Any Docker container will connect to this bridge network by default.
Two Docker containers need to be connected to the same Docker network in order to be able to communicate to each other.
Therefore, by default any Docker container will be able to talk to each other through this default bridge network.

1. List the default Docker networks:

    ```
    docker network ls
    ```
2. Create one container connected to the default bridge network:

    ```
    docker run --detach --name test1 --network bridge --tty index.docker.io/library/busybox:latest
    ```
3. Test the container network pinging different targets:

    ```
    docker exec test1 ping -c 1 localhost
    
    docker exec test1 ping -c 1 google.com
    
    docker exec test1 ping -c 1 8.8.8.8
    ```
1. Check the IP address of this first container:

    ```
    docker inspect test1
    ```
3. Create a second container in the same default bridge network:

    ```
    docker run --detach --name test2 --network bridge --tty index.docker.io/library/busybox:latest
    ```
1. Test the container network pinging the IP address of the previous test container (172.17.0.2 in my case):

    ```
    docker exec test2 ping -c 1 172.17.0.2
    ```
1. Create a custome bridge network:

    ```
    docker network create my_bridge --driver bridge
    ```
1. Check the list of networks:

    ```
    docker network ls
    ```
1. Create two containers connected to this custom bridge:

    ```
    docker run --detach --name custom1 --network my_bridge --tty busybox
    
    docker run --detach --name custom2 --network my_bridge --tty busybox
    ```
1. Test the container network pinging different targets:

    ```
    docker exec custom1 ping -c 1 localhost

    docker exec custom1 ping -c 1 google.com

    docker exec custom1 ping -c 1 8.8.8.8

    docker exec custom1 ping -c 1 custom2
    
    docker exec custom1 ping -c 1 test1
    
    docker exec custom1 ping -c 1 172.17.0.2
    ```
1. Compare the following two commands:

    ```
    docker exec custom1 ping -c 1 custom2

    docker exec test2 ping -c 1 test1
    
    docker exec test2 ping -c 1 172.17.0.2
    ```
1. Let us create a second custom bridge:

    ```
    docker network create my_bridge_2 --driver bridge
    ```
1. Let us create a container in this network:

    ```
    docker run --detach --name custom3 --network my_bridge_2 --tty index.docker.io/library/busybox:latest
    ```
1. Check the isolation of this container:

    ```
    docker exec custom3 ping -c 1 localhost
    
    docker exec custom3 ping -c 1 google.com
    
    docker exec custom3 ping -c 1 8.8.8.8
    
    docker exec custom3 ping -c 1 172.17.0.2
    
    docker exec custom3 ping -c 1 custom1
    ```
1. Let us connect this new container to both custom networks:

    ```
    docker exec custom3 ifconfig

    docker network connect my_bridge custom3
    
    docker exec custom3 ifconfig
    ```
1. Check the connectivity between the containers:

    ```
    docker exec custom3 ping -c 1 custom1
    
    docker exec custom3 ping -c 1 custom2
    
    docker exec custom3 ping -c 1 172.17.0.2
    ```
1. Inspect the Docker network:

    ```
    docker inspect my_bridge
    
    docker inspect my_bridge_2
    ```
1. Bridges are isolated by IPtables firewall:

    ```
    iptables -S -t filter | grep A.DOCKER-ISOLATION-STAGE-2.*DROP
    ```
# Experimenting with the Host network:
1. List the network interface configuration of the host machine:

    ```
    ifconfig
    ```
3. Create a container connected to the Host network:

    ```
    docker run --detach --name host1 --network host --tty busybox
    ```
1. List the network interface configuration from inside the container. It will be the same configuration as the host machine because the container is connected to the Host network:

    ```
    docker exec host1 ifconfig
    ```
# Experimenting with the Null network:
1. Create a container using the null network:

    ```
    docker run --detach --name none1 --network none --tty busybox
    ```
1. When we create a container attached to the null network there is no network interface defined inside the container:

    ```
    docker exec none1 ifconfig
    ```
1. You can still send or retrieve files from the isolated container using `cp` and `exec`:

    ```
    docker cp examples.desktop none1:/tmp/
    
    sudo docker exec none1 ls /tmp/
    ```
