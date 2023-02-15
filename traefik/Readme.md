# Traefik
## Lab limit public access
- edit hosts file for fix solution
- this domain demo is ".cpelab.local
- demo cann't get cetificate from Let's Encrypt 

## Step
- Create a network that will be shared with Traefik
```
docker network create --driver=overlay traefik-public
```
- Create a tag in this node store public certificate file

```
export NODE_ID=$(docker info -f '{{.Swarm.NodeID}}')
echo $NODE_ID
```
```
docker node update --label-add traefik-public.traefik-public-certificates=true $NODE_ID
```

- Create an environment variable with your email, to be used for the generation of Let's Encrypt certificates,
- change your information before export

```
export EMAIL=user@smtp.com
export DOMAIN=traefik.cpelab.local
export USERNAME=admin
export PASSWORD=changeMe
export HASHED_PASSWORD=$(openssl passwd -apr1 $PASSWORD)
echo $HASHED_PASSWORD
```
- deploy traefik stack
```
docker stack deploy -c traefik-host.yml traefik
```

- access the web UI at https://traefik.cpelab.local