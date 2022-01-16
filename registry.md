```
docker run --detach --name registry --publish 5000:5000 --restart always --volume registry:/var/lib/registry:rw library/registry:2
docker pull library/busybox:latest
docker tag library/busybox:latest localhost:5000/my_busybox:1.0
docker push localhost:5000/my_busybox:1.0
docker pull localhost:5000/my_busybox:1.0
docker volume inspect registry
sudo find /var/lib/docker/volumes/registry/_data/
```
