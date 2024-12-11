# How to deploy a Docker stack using a Docker compose file

* https://docs.docker.com/compose/compose-file/compose-file-v3/

```
tee ${PWD}/phpinfo/docker-compose.yaml 0<<EOF

# docker-compose.yaml
configs:
  phpinfo-config:
    external: false
    file: index.php
networks:
  phpinfo-network:
    external: false
    internal: false
secrets:
  phpinfo-secret:
    external: false
    file: index.php
services:
  phpinfo-svc:
    command:
      - -f
      - index.php
      - -S
      - 0.0.0.0:8080
    configs:
      - source: phpinfo-config
        target: /var/data/index.php
        uid: '65534'
        gid: '65534'
        mode: 0400
    deploy:
      replicas: 2
      resources:
        limits:
          cpus: '0.2'
          memory: 200M
        reservations:
          cpus: '0.2'
          memory: 200M
    entrypoint:
      # docker exec phpinfo which php
      - /usr/local/bin/php
    image: index.docker.io/library/php:alpine@sha256:ab23b416d86aec450ee7b75727f6bbec272edc2764a1b6fad13bc2823c59bb6b
    networks:
      - phpinfo-network
    ports:
      - 8080:8080
    read_only: true
    # docker exec phpinfo touch sebastian
    secrets:
      - source: phpinfo-secret
        target: /var/data/index2.php
        uid: '65534'
        gid: '65534'
        mode: 0400
    user: nobody:nogroup
    # docker exec phpinfo id
    # docker exec phpinfo whoami
    volumes:
      - type: volume
        source: my_volume
        target: /var/data/my_volume/
        read_only: false
    working_dir: /var/data/
version: '3.8'
volumes:
  my_volume:
    external: false
    
EOF
```
```
docker swarm init
```
```
docker stack rm PHPINFO ; sleep 20

docker stack deploy --compose-file ${PWD}/phpinfo/docker-compose.yaml PHPINFO

docker stack ps PHPINFO --no-trunc

docker stack ls

docker stack services PHPINFO

docker service logs PHPINFO_phpinfo-svc

docker ps
```
```
docker exec PHPINFO_php-svc.4.z51xlgdlmt1ifpjmjvemh8z1r df
```
