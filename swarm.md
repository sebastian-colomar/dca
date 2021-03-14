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
sudo docker image --help
sudo docker volume --help
sudo docker network --help
```
```
sudo docker service --help
sudo docker stack --help
sudo docker swarm --help
```
