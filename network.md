```
sudo docker network ls
sudo docker network create sebastian
sudo docker network inspect sebastian
sudo docker network prune --force 
```
```
sudo docker container run -d --name web --network host nginx:alpine
sudo docker container exec web ip route
ip route
sudo docker container exec web netstat -lnt
netstat -lnt
sudo docker container exec web curl localhost
curl localhost
```
