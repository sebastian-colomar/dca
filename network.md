```
docker network ls
docker network create sebastian
docker network inspect sebastian
docker network prune --force 
```
```
docker container run -d --name web --network host nginx:alpine
docker container exec web ip route
ip route
docker container exec web netstat -lnt
netstat -lnt
docker container exec web curl localhost
curl localhost
```
