```
sudo docker container run -d --name web --rm nginx:alpine
sudo docker image history nginx:alpine
sudo docker container export web -o nginx.tar
sudo docker image import nginx.tar nginx:flat
sudo docker image history nginx:flat
```
