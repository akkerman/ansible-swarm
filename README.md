Ansible Role for Docker Swarm
=========

Configures docker swarm on ubuntu bionic.

Requirements
------------

None

Role Variables
--------------

Role variables with their defaults:

```yml
docker_swarm_leader: "{{ groups['docker_swarm_managers'][0] }}"
docker_swarm_interface: "eth1"
```

The first entry in the inventory group `docker_swarm_managers` will be the one that's initialized first and all subsequent managers and workers will join. When changing this, make sure the node is also in the `docker_swarm_mangers` for consistency.

The ipv4 address will be gathered from the ethernet interface specified by `docker_swarm_interface`. This is the address the `docker_swarm_leader` will listen on and should be reachable by all other nodes that want to join the cluster. Check the docker swarm documentation, to know about [open protocols and ports between the hosts](https://docs.docker.com/engine/swarm/swarm-tutorial/#open-protocols-and-ports-between-the-hosts).


Dependencies
------------

None, just make sure that:

1. Docker is installed
2. The user running the playbook belongs to docker group (or specify become: yes in the playbook)

Example Playbook
----------------

```yaml
---
- hosts: all
    roles:
        - akkerman.swarm
```

Example inventory
-----------------

```ini
[docker_swarm_managers]
node-0

[docker_swarm_workers]
node-1
node-2
```

A minimum of one host should be in the `docker_swarm_managers` group. 
The role only changes the host if the swarm on that node is inactive, i.e. you cannot move a node to another group and expect the cluster to adapt.

License
-------

BSD

Author Information
------------------

[Marcel Akkerman](https://github.com/akkerman)
