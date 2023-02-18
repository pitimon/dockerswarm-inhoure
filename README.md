# dockerswarm-inhoure
 myCluster
- [นำทางโดย Docker Swarm Rock](https://dockerswarm.rocks)
## Prepare node Ubuntu 22.04 (3 nodes)
```shell
sudo -i
```
- replace change-me to your hostname
```shell
hostnamectl set-hostname “Change-me”
```
- change local time
```shell
timedatectl set-timezone Asia/Bangkok
```
- docker engine install for ubuntu
```
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
- replace apt to nala packet management 
- [nala](https://gitlab.com/volian/nala)
```shell
wget https://gitlab.com/volian/nala/uploads/605d833bdffd23cee4bb6670b2d6c27b/nala_0.12.1_all.deb
dpkg -i nala_0.12.1_all.deb 
apt-get -f install -y  
```
```
tee -a /etc/apt/sources.list.d/nala-sources.list <<EOF 
# Sources file built for nala
deb https://mirror1.ku.ac.th/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.nipa.cloud/ubuntu/ jammy main restricted universe multiverse
deb https://mirror.kku.ac.th/ubuntu/ jammy main restricted universe multiverse
deb http://mirror1.totbb.net/ubuntu/ jammy main restricted universe multiverse
EOF
```
```
nala update  
nala upgrade -y 
nala list —upgradable
nala install htop dnsutils mtr -y
```
```
reboot
```
### Option: clone to template
```shell
cp /dev/null /etc/machine-id
rm /var/lib/dbus/machine-id
ln -s /etc/machine-id /var/lib/dbus/machine-id
init 0
```
- grant user for docker authorited
- server normal user login
```shell
sudo usermod -aG docker $USER
docker ps
```
## Swarm init
```
docker swarm init
```
- copy command token for run on worker node
- after run at manager node
```
docker node ls
```
### deploy portainer for swarm (Manager Node)
```
curl -L https://downloads.portainer.io/ce2-17/portainer-agent-stack.yml -o portainer-agent-stack.yml
docker stack deploy -c portainer-agent-stack.yml portainer
```

- Revert Proxy
- edit hosts file (Lab compatible)
## Traefik Proxy
- [Treafik](https://traefik.io/)
- [Step](https://github.com/pitimon/dockerswarm-inhoure/tree/main/traefik)

## Swarmpit
- [Swarmpit](https://swarmpit.io)
- [Step](https://github.com/pitimon/dockerswarm-inhoure/tree/main/swarmpit)

## Sample deploy
- hello-world sample