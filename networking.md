```
docker network create net1
docker container run --detach --name c1 --network net1 --tty library/alpine:latest

docker network create net2
docker container run --detach --name c2 --network net2 --tty library/alpine:latest

docker container run --detach --name c0 --network none --tty library/alpine:latest
```
