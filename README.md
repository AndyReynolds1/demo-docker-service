http://play-with-docker.com

## Single container

`docker run -d --name web -p 8080:8080 areynolds762/demo-node-website:1.0`

`docker stop web; docker rm web`


## Create swarm

`docker swarm init --advertise-addr $(hostname -i)`

### Create visualizer service
```
docker service create \
  --name=viz \
  --publish=8000:8080/tcp \
  --constraint=node.role==manager \
  --mount=type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock \
  dockersamples/visualizer
```

`docker swarm join-token manager`
  
Add additional nodes to swarm

## Deploy web app in swarm with 2 nodes
```
docker service create \
  --name web \
  --publish 8080:8080 \
  --replicas 2 \
  --update-delay 10s \
  areynolds762/demo-node-website:1.0
```

Notice container ID


## Drain node (self healing)
`docker node update --availability drain node2`

`docker node update --availability active node2`

## Scaling

`docker service scale web=4`

`docker service scale web=20`

`docker service scale web=4`

## Update image
`docker service update --image areynolds762/demo-node-website:2.0 web`

## Management tools
```
docker service create \
--name portainer \
--publish 9000:9000 \
--replicas=1 \
--constraint 'node.role == manager' \
--mount type=bind,src=//var/run/docker.sock,dst=/var/run/docker.sock \
portainer/portainer \
-H unix:///var/run/docker.sock
```

# Multi container service example
`git clone https://TheStally@bitbucket.org/TheStally/demo-docker-service.git`

`cd demo-docker-service`

`docker-compose up`

