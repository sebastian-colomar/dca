```bash
sudo docker swarm init
sudo docker node ls
```
```
git clone https://github.com/academiaonline/dca-phpinfo
cd dca-phpinfo
git pull
sudo docker stack deploy -c etc/swarm/manifests/dca-phpinfo.yaml dca-phpinfo
```
```
sudo docker service ls
sudo docker service logs dca-phpinfo_phpinfo
curl localhost -I
```
```
sudo docker stack rm dca-phpinfo
```
