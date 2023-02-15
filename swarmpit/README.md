# Swarmpit

- Create an environment variable with the domain 
```
export DOMAIN=swarmpit.cpelab.local
```
- Create a label in this node, so that the CouchDB database
```
export NODE_ID=$(docker info -f '{{.Swarm.NodeID}}')
docker node update --label-add swarmpit.db-data=true $NODE_ID
```
- Create another label in this node, so that the Influx database
```
export NODE_ID=$(docker info -f '{{.Swarm.NodeID}}')
docker node update --label-add swarmpit.influx-data=true $NODE_ID
```
- Deploy the stack with:
```
docker stack deploy -c swarmpit.yml swarmpit
```
- Check if the stack was deployed with:
```
docker stack ps swarmpit

docker service logs swarmpit_app
```
- http://swarmpit.cpedemo.local