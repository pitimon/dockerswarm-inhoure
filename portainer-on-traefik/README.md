- Create an environment variable with the domain where you want to access your Portainer 
```
export DOMAIN=portainer.cpedemo.local
```
- Get the Swarm node ID of this (manager) node and store it in an environment variable
```
export NODE_ID=$(docker info -f '{{.Swarm.NodeID}}')
docker node update --label-add portainer.portainer-data=true $NODE_ID
```

- Deploy the stack with
```
docker stack deploy -c portainer.yml portainer
```
- Check if the stack was deployed with
```
docker stack ps portainer
```
- You can check the Portainer logs with
```
docker service logs portainer_portainer
```
