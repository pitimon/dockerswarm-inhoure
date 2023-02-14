# dockerswarm-inhoure
 myCluster
- [Docker Swarm Rock](https://dockerswarm.rocks)
## Prepare node
```shell
sudo -i
```
```shell
hostnamectl set-hostname “Change-me”
```
```shell
timedatectl set-timezone Asia/Bangkok

apt update ; apt upgrade -y
apt-get install \
    ca-certificates \
    curl wget \
    gnupg \
    lsb-release -y

mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" |  tee /etc/apt/sources.list.d/docker.list > /dev/null

apt-get update
apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```
```shell
wget https://gitlab.com/volian/nala/uploads/605d833bdffd23cee4bb6670b2d6c27b/nala_0.12.1_all.deb
dpkg -i nala_0.12.1_all.deb 
apt-get -f install -y  
nala update  
nala upgrade -y 
nala list —upgradable
nala install htop dnsutils mtr -y
# nala install “other list”
reboot
```
### Option: clone to template
```shell
cp /dev/null /etc/machine-id
rm /var/lib/dbus/machine-id
ln -s /etc/machine-id /var/lib/dbus/machine-id
reboot
```
```shell
sudo usermod -aG docker $USER
docker ps
```
### Swarm init
```
docker swarm init
```
- copy command token for run on worker node
- after run at manager node
```
docker node ls
```
### deploy portainer for swarm
```shell
curl -L https://downloads.portainer.io/ce2-17/portainer-agent-stack.yml -o portainer-agent-stack.yml
docker stack deploy -c portainer-agent-stack.yml portainer
```