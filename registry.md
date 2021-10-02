```
docker run -d -p 5000:5000 --name registry registry:2
docker pull busybox
docker tag busybox localhost:5000/my_busybox:1.0
docker push localhost:5000/my_busybox:1.0
docker pull localhost:5000/my_busybox:1.0
```
