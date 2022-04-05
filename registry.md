```
docker run --detach --name registry --publish 5000:5000 --restart always --volume registry:/var/lib/registry:rw docker.io/library/registry:2
docker pull docker.io/library/busybox:latest
docker tag docker.io/library/busybox:latest localhost:5000/my_library/my_busybox:1.0
docker push localhost:5000/my_library/my_busybox:1.0
docker pull localhost:5000/my_library/my_busybox:1.0
docker volume inspect registry
sudo find /var/lib/docker/volumes/registry/_data/
```
