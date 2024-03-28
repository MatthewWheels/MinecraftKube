This is a set of manifests to deploy the most recent version of itzg's vanilla minecraft docker image.

- uses nodeport exposed on host port: `30000`
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