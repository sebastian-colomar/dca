```
docker container run --entrypoint ENTRYPOINT_BINARY_EXECUTABLE --rm IMAGE_FILESYSTEM:RELEASE ARGUMENTS_FOR_THE_ENTRYPOINT
```
```
docker container run --entrypoint echo --rm alpine:latest "HELLO WORLD"
docker container run --entrypoint /bin/echo --rm alpine:latest "HELLO WORLD"
docker container run --entrypoint /sbin/ip --rm alpine:latest route
docker container run --entrypoint /bin/ls --rm alpine:latest /bin
docker container run --entrypoint /usr/bin/which --rm alpine:latest netstat
```
