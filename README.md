This is a set of manifests to deploy the most recent version of itzg's vanilla minecraft docker image.

The way this works is that there are 2 main processes running for this. You have your minecraft servers and the mc-router. The router is responsible for receiving traffic on port `30000` and then routing that traffice based on the recieved hostname to the appropriate minecraft server. The minecraft server itself hosts the game that the player connects to. Using a PVC we allow for the world data to persist after pod deletion. Additionally the router and service annotation system allow us to host multiple minecraft servers while only exposing a single port on the node. 

- uses nodeport exposed on port: `30000`
- routes based on hostname which is specified on the loadbalancer service for the minecraft server pod's annotation.
- server files persist on pod deletion

- configurations you need to make if you use this:
  - `spec.local.path` on the `vol.yaml`
    - where you want the server files stored
  - the value for the node affinity in the `vol.yaml`
    - sets which node you want the volume and server deployed 
  - `hostname` in the `service.yaml`
    - sets what you want players to enter in order to be routed to that particular service

- optional configs
  - MOTD, server name, and OPs for the server
    - based on [this](https://docker-minecraft-server.readthedocs.io/en/latest/configuration/server-properties/)



[Itzg's mc-router](https://github.com/itzg/mc-router)
- has a great description of how the router functions at a deeper level
- needs access to services for this deployment and if you're using itzg's stateful set deployment for the minecraft server

[Itzg's minecraft server](https://github.com/itzg/docker-minecraft-server)
- his deployments here use a nodeport which also works but requires a 1 port:1 server ratio
- doesn't use a PVC which allow for more portability but less persistence on data files. 