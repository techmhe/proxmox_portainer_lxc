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

## Known Issues

### ZFS-backed datastores
Docker appears to have a bug when running in an LXC on a ZFS-backed datastore as it will not enable the native ZFS driver for /var/lib/docker, instead trying to use the AUFS driver (and failing). To work-around the issue, after installation using this script, perform the following:

1. set the docker services to disabled in the LXC container (e.g. systemctl stop docker; systemctl disable docker)
2. rename your existing /var/lib/docker dir to something else like docker_old
3. stop the LXC container
4. in Proxmox gui create a new storage mount point at /var/lib/docker for the container from your ZFS thin zpool
5. start the container and move all contents within your renamed /var/lib/docker_old dir to the new mount point at /var/lib/docker
6. re-enable and restart docker services (e.g. systemctl enable docker; systemctl start docker)

Enjoy !

-=dave
