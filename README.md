
# Docker + Microservices Training - Jan 2020

## Presenter
Gautham Pai
https://jnaapti.com/

## Course Outline
https://jnaapti.com/downloads/cisco-docker.pdf

## DAY 1 (20 JAN 2020)
### Section 1 - Overview

- Server-side components can be categorized into 2 kinds - _stateless_ and _stateful_

- Ideally, any stateful operation (e.g. PUT, POST requests) must **NOT** change the state of the application server. Only then, is it completely stateless. Even logging must not happen within the application.

- Statelessness allows 2 things - failure handling, scaling

- Scaling has 2 types:
    - Vertical scaling (aka scale up)
    - Horizontal scaling (aka scale out)
    
- Scale-out + load-balancing is always better; allows us to use multiple machines with commodity hardware. Much cheape than scaling up.

- Load balancers also help manage failures - by frequently checking for uptime of each server and 'blacklisting' a server if it is down, and not fwding requests until it comes back up.

- Can't load-balance load balancers, so we use multiple load balancers and do DNS resolutiion tricks (geo-DNS etc.) to ensure different clients hit different load balancers.

- With stateless components, failure recovery == simply replace/respin!

- Sticky sessions ==> not a good idea, because in-memory session state is _still state!_ So, just maintain sessions outside the stateless services, i.e. put it in some _stateful_ component (cache, DB etc.)

- Handling scale and failures with stateful components:
    - **Sharding** - to handle scalability
    - **Replication** - to handle failures
    
- Private Cloud --> Public Cloud --> Hybrid Cloud --> Multi Cloud

- AWS CloudFormation - PaaS - infrastructure as code

### Section 2 - Containers & Docker

- Container == 'jailed process'

- Linux containers runnning in MacOS are actually running inside a linux VM that is running on MacOS

- Windows also ships 'WSL' (Windows Subsystem for Linux) which runs a linux kern el inside the windows kernel

- Docker uses a layered file system. The internal container filesystem is maintained as a layer

### Section 3 + Section 4
- Exercise: Spring Boot App as a Docker Container: https://gist.github.com/umnagendra/eaf01070052e8c6c714ef8fc56d8749a

## DAY 2 (21 JAN 2020)
### Section 5 - Docker Networking

- container <-> node, container <-> internet, container <-> container (same node), container <-> container (different nodes)

- `docker0` acts as the bridge interface to connect containers to internet, also acts as a switch between containers

- 3 types of networking drivers - `bridge`, `host` and `none`. There is a default bridge network, we can create our own bridge network too. (`docker network create`)

- When using a custom network, containers in the same network can address each other using the container names

- Containers of one network (mynet2) cannot communicate with containers of another network (mynet1), _**unless**_ you establish a new network interface inside a container such that it is part of 2 (or more) networks (`docker network connect <other-network> <my-container>`)

- You can even mount device files (mounted USB, sound devices etc.) into a container, run emulators etc. - because inn linux, everything is a file

- There is a 4th type of networking - **OVERLAY** that enables multi-node docker communication. Once the docker cluster is setup (using overlay), it works the same way as a bridge network. The multi-node complexities are abstracted by overlay networking. (`docker network create --driver overlay`)

- You can create an overlay network in one node, and it will show up in other nodes of the docker cluster. This is powerful - you are sctually _scaling-out_, but thanks to the networking, it all seems like one single big machine (like _scaling-up_)

- Overlay networks , however, are a challenge beyond a point, because it takes control away from low-level networking implementations. Kubernetes is designed such that we can plug-in network drivers.
