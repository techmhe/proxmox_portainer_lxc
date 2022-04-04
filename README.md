# Install Portainer into a Proxmox LXC container

Benefits of running Portainer docker management framework nested within an LXC container on Proxmox are:
* Less memory and cpu resource overhead
* Modifying the virtual hardware resouces assigned to the LXC container can be done without having to reboot, as would be necessary when using a traditional VM

These benefits allow one to provision more services given the same hardware resources — especially in ram-limited systems — and maintain higher uptime for their docker-based microservices.

## Usage

***Note:*** _Before using this repo, make sure your Proxmox host is up to date._

To setup a new Portainer installation within a new LXC container on your Proxmox host, download the install script to your Proxmox host's console:

```
wget -qL https://github.com/fiveangle/proxmox_portainer_lxc/raw/master/create_container.sh
```

Edit `create_container.sh` to set your desired container disk size, hostname, portainer version, etc, then start the installation with:

```
bash create_container.sh
```

Enjoy !

-=dave
