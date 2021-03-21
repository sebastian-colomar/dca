```bash
sudo docker swarm init
sudo docker node ls
```
```
git clone https://github.com/academiaonline/dca-phpinfo
cd dca-phpinfo
sudo docker stack deploy -c etc/swarm/manifests/dca-phpinfo.yaml dca-phpinfo
```
```
sudo docker service ls
sudo docker service logs dca-phpinfo_dca-phpinfo
curl localhost -I
```
```
sudo docker stack rm dca-phpinfo
cd
rm -rf dca-phpinfo
git clone https://github.com/academiaonline/dca-phpinfo
cd dca-phpinfo
sudo docker stack deploy -c etc/swarm/manifests/dca-phpinfo.yaml dca-phpinfo
sudo docker service ls
sudo docker service logs dca-phpinfo_dca-phpinfo
curl localhost -I
```
```
sudo docker container --help
  sudo docker container exec
  sudo docker container kill
  sudo docker container ls
  sudo docker container port
  sudo docker container prune
  sudo docker container rm
  sudo docker container run
sudo docker image --help
  sudo docker image build
  sudo docker image prune
  sudo docker image pull
  sudo docker image push
sudo docker volume --help
sudo docker network --help
```
```
sudo docker node --help
  sudo docker node ls
sudo docker service --help
  sudo docker service create
  sudo docker service ls
  sudo docker service logs
  sudo docker service ps
  sudo docker service scale
sudo docker stack --help
  sudo docker stack deploy
  sudo docker stack ls
  sudo docker stack rm
sudo docker swarm --help
  sudo docker swarm init
  sudo docker swarm join
  sudo docker swarm leave
```
```
sudo docker service create --name dca-phpinfo -p 8000:8080 academiaonline/dca-phpinfo:latest
sudo docker service ls
sudo docker service ps dca-phpinfo
sudo docker service scale dca-phpinfo=3
sudo docker service ps dca-phpinfo
sudo docker service rm dca-phpinfo
```
```bash
sudo iptables -S -t nat
```
