# Introduction to Docker

1. https://en.wikipedia.org/wiki/Linux_namespaces
1. https://en.wikipedia.org/wiki/Cgroups

# Shell environment
- https://shell.cloud.google.com/

# PHP sample application

1. https://www.php.net/docs.php
2. https://www.php.net/manual/en/function.phpinfo

Create a new project folder in your home directory:
```
mkdir --parents ${PWD}/phpinfo/
```
Create a PHP file in the project folder:
```
tee ${PWD}/phpinfo/index.php 0<<EOF

<?php
// Show all information, defaults to INFO_ALL
phpinfo();
// Show just the module information.
// phpinfo(8) yields identical results.
phpinfo(INFO_MODULES);
?>

EOF
```
Execute the PHP application parsing the PHP file and launching a PHP embedded webserver:
```
php -f ${PWD}/phpinfo/index.php -S localhost:9000 &
```
Check the web server created:
```
curl localhost:9000/phpinfo/index.php -I -s
```
Find the PID of the running process:
```
ps aux|grep phpinfo --max-count 1
```
Or alternatively:
```
pidof php
```
Save this PID in an environment variable:
```
pid=$(pidof php)
```
Now you can inspect the running process:
```
ls /proc/${pid}/
```
```
strings /proc/${pid}/cmdline
```
```
strings /proc/${pid}/net/fib_trie
```

# Introduction to Docker

1. https://docs.docker.com/
2. https://hub.docker.com/

Check the version of the Docker client:
```
docker version
```
Check the status of the Docker service:
```
service docker status
```
# Example of containerization of our PHP sample application

Let us first create a network for our container:
```
docker network create phpinfo --driver bridge
```
Let us download the Docker image from Docker Hub:
```
docker pull index.docker.io/library/php:alpine

docker images
```
Let us see the Docker registry and the Dockerfile:
- https://hub.docker.com/_/php
- https://github.com/docker-library/php/blob/master/8.3/alpine3.19/cli/Dockerfile

Let us see all the options for docker run command:
```
docker run --help
```
Let us create the container:
```
docker run --cpus 0.01 --detach --env AUTHOR=Sebastian --memory 20M --memory-reservation 10M --name phpinfo --network phpinfo --publish 60000:9000 --read-only --restart always --user nobody:nogroup --volume ${HOME}/phpinfo/index.php:/var/data/index.php:ro --workdir /var/data/ index.docker.io/library/php:alpine php -f index.php -S 0.0.0.0:9000
```
# Troubleshooting the Docker container:

View the table of processes running inside you container
```
docker top phpinfo
```
View the logs of your container:
```
docker logs phpinfo
```
Show the resources consumption statistics of your container:
```
docker stats phpinfo --no-stream
```
Show the content of the working directory:
```
docker exec phpinfo ls -l
```
Test the connection to the webserver from inside the container:
```
docker exec phpinfo curl localhost:9000/index.php -I -s
```
Test the connection to the webserver from outside the container:
```
curl localhost:60000/index.php -I -s
```
In order to remove the container:
```
docker rm phpinfo --force
```

# How to deploy a Docker stack using a Docker compose file

```
tee ${PWD}/phpinfo/docker-compose.yaml 0<<EOF

# docker-compose.yaml
configs:
#secrets:
  phpinfo:
    external: false
    file: ./index.php
networks:
  phpinfo:
    external: false
    internal: false
services:
  phpinfo:
    command:
      - php
      - -f
      - index.php
      - -S
      - 0.0.0.0:9000
    configs:
    #secrets:
      - source: phpinfo
        target: /var/data/index.php
        uid: '65534'
        gid: '65534'
        mode: 0400
    deploy:
      replicas: 2
      resources:
        limits:
          cpus: '0.02'
          memory: 20M
        reservations:
          cpus: '0.01'
          memory: 10M
    image: index.docker.io/library/php:alpine
    networks:
      - phpinfo
    ports:
      - 60000:9000
    read_only: true
    user: nobody:nogroup
    volumes:
      - type: volume
        source: phpinfo
        target: /var/data/tmp/
        read_only: false
    working_dir: /var/data/
version: '3.8'
volumes:
  phpinfo:
    external: false
    
EOF
```
```
docker swarm init
```
```
docker stack rm PHPINFO ; sleep 20

docker stack deploy --compose-file ${PWD}/phpinfo/docker-compose.yaml PHPINFO
```
```
docker stack ps PHPINFO
```
```
docker stack ls
```
```
docker stack services PHPINFO
```
```
docker service logs PHPINFO_phpinfo
```
```
docker ps
```
```
ps=$(docker stack ps PHPINFO --no-trunc --quiet|head -1)
```
```
docker exec PHPINFO_phpinfo.1.${ps} df
```
```
docker exec PHPINFO_phpinfo.1.${ps} ps aux
```
```
docker top PHPINFO_phpinfo.1.${ps}
```
