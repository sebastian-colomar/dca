```
docker container run --detach --entrypoint /bin/sh --name test --rm --tty library/alpine:latest
docker container exec test /usr/binwhich php
docker container exec test /sbin/apk add php
docker container exec test /usr/bin/which php
docker container commit test alpine:php
```
