```
docker container run --entrypoint ENTRYPOINT_BINARY_EXECUTABLE --rm IMAGE_FILESYSTEM:RELEASE ARGUMENTS_FOR_THE_ENTRYPOINT
```
```
docker container run --entrypoint echo --rm alpine:latest "HELLO WORLD"
docker container run --entrypoint /bin/echo --rm alpine:latest "HELLO WORLD"
docker container run --entrypoint /sbin/ip --rm alpine:latest route
docker container run --entrypoint /bin/ls --rm alpine:latest /bin
docker container run --entrypoint /usr/bin/which --rm alpine:latest netstat
docker container run --entrypoint /bin/netstat --rm alpine:latest --help
```
```
cat /proc/1/cgroups
docker container run --detach --entrypoint /bin/ping --name pinger --rm alpine:latest localhost
docker container logs pinger
docker container ls
docker container top pinger
cat /proc/27226/cgroup
docker container stats pinger --no-stream
docker container kill pinger
docker container run --cpus 0.05 --detach --entrypoint /bin/ping --name pinger --rm alpine:latest localhost
docker container kill pinger 
docker container run --cpus 0.05 --detach --entrypoint /bin/ping --memory 10M --name pinger --rm alpine:latest localhost
docker container stats pinger --no-stream
```
```
docker container ls
docker ps
```
