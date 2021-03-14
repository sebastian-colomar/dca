```bash
sudo docker swarm init
sudo docker node ls
```
```
git clone https://github.com/academiaonline/dca-phpinfo
cd dca-phpinfo
sudo docker stack deploy -c etc/swarm/manifests/dca-phpinfo.yaml dca-phpinfo
```
