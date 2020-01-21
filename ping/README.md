# ping
Illustrates docker networking concepts

```shell
# start 2 containers
docker run -itd --name c1 ping
docker run -itd --name c2 ping
```

```shell
# inspect the containers, get the IP of each container
docker inspect c1   # 172.17.0.3
docker inspect c2   # 172.17.0.4
```

```shell
# check if c1 and c2 can ping each other
docker exec -it c1 ping 172.17.0.4
docker exec -it c2 ping 172.17.0.3
```

```shell
# create our own bridge network
docker network create --driver bridge --subnet 172.18.0.0/16 mynet1

# inspect the network
docker network inspect mynet1

# create containers with this network
docker run -itd --name c3 --net mynet1 ping:0.0.1
docker run -itd --name c4 --net mynet1 ping:0.0.1

# check if c3 and c4 can ping each other
docker exec -it c3 ping 172.18.0.3
docker exec -it c4 ping 172.18.0.2

# create another bridge network
docker network create --driver bridge --subnet 172.19.0.0/16 mynet2

# inspect the network
docker network inspect mynet2

# create containers with this network
docker run -itd --name c5 --net mynet1 ping:0.0.1
docker run -itd --name c6 --net mynet1 ping:0.0.1

# check if c3 and c4 can ping each other
docker exec -it c5 ping 172.19.0.3
docker exec -it c6 ping 172.19.0.2

# containers of one network (mynet2) cannot communicate with containers of another network (mynet1)
docker exec -it c6 ping 172.18.0.3      # fails!

# ... unless, you establish a new network interface inside a container such that it is part of 2 (or more) networks
docker network connect mynet1 c6
docker exec -it c6 ping 172.18.0.3      # works!
```

