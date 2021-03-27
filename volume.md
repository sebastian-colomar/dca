```bash
sudo docker volume create sebastian
sudo ls /var/lib/docker/volumes/sebastian/_data -l
sudo docker container run --rm --name test -d -v sebastian:/opt alpine ping localhost 
sudo docker container exec test ls /opt -l
sudo docker container exec test touch /opt/sebastian
sudo docker container exec test ls /opt -l
sudo ls /var/lib/docker/volumes/sebastian/_data -l
```
```
mkdir example
sudo docker container run --rm --name test2 -d -v $PWD/example:/opt alpine ping localhost 
sudo docker container exec test2 ls -l /opt
ls example -l
sudo docker container exec test2 touch /opt/helloworld
sudo docker container exec test2 ls -l /opt
ls example -l
```
