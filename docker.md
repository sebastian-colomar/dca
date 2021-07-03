 ```bash
 1998  sudo docker run --name test -d alpine
 1999  sudo docker ps 
 2000  sudo docker ps -a
 2001  sudo ls /var/lib/docker/container
 2002  sudo ls /var/lib/docker/containers
 2003  sudo docker ps -a --no-trunc
 2004  sudo docker start f01ec86941fdd6384d948914cd3a2cefa8c2fdc87a5df575ef8e57835bfdcf7f
 2005  sudo docker ps
 2006  sudo docker kill f01ec86941fdd6384d948914cd3a2cefa8c2fdc87a5df575ef8e57835bfdcf7f
 2007  sudo docker ps
 2008  sudo docker ps -a --no-trunc
 2009  sudo docker run --name test -d -t alpine 
 2010  sudo docker ps -a
 2011  sudo docker rm test nginx 
 2012  sudo docker run --name test -d -t alpine 
 2013  sudo docker ps
 2014  df -h
 2015  df
 2016  sudo docker ps
 2017  sudo docker exec test echo HELLO WORLD
 2018  sudo docker exec test date
 2019  sudo docker exec test sleep 3
 2020  sudo docker exec test bash
 2021  sudo docker exec test find / 
 2022  sudo docker exec test which bash
 2023  which bash
 2025  sudo docker exec test ls -l /bin/bash
 2026  sudo docker exec test find / | grep bash
 2027  sudo docker exec test find / | grep sh$
 2028  sudo docker exec -i -t test sh 
 2029  sudo docker exec test free
 2030  sudo docker exec test df
 2031  sudo docker exec test ss
 2032  sudo docker exec test netstat
 2033  sudo docker exec test netstat -ltn
 2034  sudo docker exec test id
 2035  sudo docker exec test curl
 2036  sudo docker exec test apk add curl 
 2037  sudo docker exec test curl www.google.com -I
 2038  sudo docker exec test which tcpdump
 2039  sudo docker exec test apk add tcpdump
 2040  sudo docker exec test which tcpdump
```
```
docker run --detach --name NAME_OF_CONTAINER --rm NAME_OF_IMAGE:RELEASE_NAME
docker exec NAME_OF_CONTAINER COMMAND ARGUMENT_OF_COMMAND
