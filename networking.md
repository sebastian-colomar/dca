```
docker container run --detach --name c0 --network none --tty library/alpine:latest

docker network create net1
docker container run --detach --name c1 --network net1 --tty library/alpine:latest

docker network create net2
docker container run --detach --name c2 --network net2 --tty library/alpine:latest
```
```
docker container exec c0 ifconfig
docker container exec c1 ifconfig
docker container exec c2 ifconfig
```
```
ip route

docker container exec c0 ip route
docker container exec c1 ip route
docker container exec c2 ip route
```
```
docker container exec c0 ping -c1 localhost
docker container exec c1 ping -c1 localhost
docker container exec c2 ping -c1 localhost
```
```
docker container exec c0 ping -c1 8.8.8.8
docker container exec c1 ping -c1 8.8.8.8
docker container exec c2 ping -c1 8.8.8.8
```
```
docker container inspect c0 | grep IPAddress
docker container inspect c1 | grep IPAddress
docker container inspect c2 | grep IPAddress
```
```
docker container exec c1 ping c2
docker container exec c2 ping c1
```
```
docker network connect net1 c2
docker container inspect c2 | grep IPAddress
docker network inspect net1

docker container exec c1 ping -c1 c2
docker container exec c2 ping -c1 c1
```
```
docker network disconnect net1 c2
docker container inspect c2 | grep IPAddress
docker network inspect net1

docker container exec c1 ping -c1 c2
docker container exec c2 ping -c1 c1
```

