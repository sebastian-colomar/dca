## INSTALL MIRANTIS CONTAINER RUNTIME
- https://docs.mirantis.com/mcr/29/install/mcr-linux/ubuntu.html
```
#sudo apt-get --yes remove docker docker-engine docker-ce docker-ce-cli docker.io
sudo apt-get --yes update
```
```
#sudo apt-get --yes install apt-transport-https ca-certificates curl software-properties-common
sudo apt-get --yes install curl gnupg
```
```
DOCKER_EE_URL="http://repos.mirantis.com"
DOCKER_EE_VERSION=29.2.1
#curl -fsSL "${DOCKER_EE_URL}/ubuntu/gpg" | sudo apt-key add -
sudo gpg --batch --yes --output /usr/share/keyrings/mirantis-archive-keyring.gpg --dearmor <<< $(curl -fsSL "${DOCKER_EE_URL}/ubuntu/gpg")
```
```
gpg --show-keys --with-fingerprint --keyid-format=short /usr/share/keyrings/mirantis-archive-keyring.gpg
echo 'Expected value = DD91 1E99 5A64 A202 E859  07D6 BC14 F10B 6D08 5F96'
#sudo apt-key fingerprint 6D085F96
#sudo add-apt-repository "deb [arch=$(dpkg --print-architecture)] $DOCKER_EE_URL/ubuntu $(lsb_release -cs) stable-$DOCKER_EE_VERSION"
SUITE=$( lsb_release -cs 2>/dev/null )
COMPONENT=stable-$DOCKER_EE_VERSION
sudo tee /etc/apt/sources.list.d/mirantis.sources 0<<EOF
Types: deb
URIs: https://repos.mirantis.com/ubuntu
Suites: $SUITE
Architectures: amd64
Components: $COMPONENT
Signed-by: /usr/share/keyrings/mirantis-archive-keyring.gpg
EOF
```
```
sudo apt-get --yes update
```
```
#sudo apt-get --yes install docker-ee docker-ee-cli containerd.io
sudo apt-get --yes install docker-ee
```
```
sudo apt-get --yes update && sudo apt-get --yes upgrade
```
```
sudo docker run hello-world
```
## ADD DOCKER GROUP
https://docs.docker.com/engine/install/linux-postinstall/
```
sudo groupadd docker
```
```
sudo usermod -aG docker $USER
```
```
newgrp docker
docker run hello-world
```
## INSTALL MIRANTIS KUBERNETES ENGINE
- https://docs.mirantis.com/mke/3.9/install/install-mke-image.html
```
UCP_VERSION=3.9.7
POD_CIDR=10.244.0.0/16
docker container run --interactive --name ucp --pod-cidr $POD_CIDR --rm --tty --volume /var/run/docker.sock:/var/run/docker.sock mirantis/ucp:$UCP_VERSION install --host-address $( ip route | grep dev.eth0.proto.kernel | awk '{ print $9 }' ) --interactive --force-minimums
```
## UNINSTALL MIRANTIS KUBERNETES ENGINE
```
UCP_VERSION=3.9.7
docker container run --rm --interactive --tty --name ucp --volume /var/run/docker.sock:/var/run/docker.sock mirantis/ucp:$UCP_VERSION uninstall-ucp --interactive
```
