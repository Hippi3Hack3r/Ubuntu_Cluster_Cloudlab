# Ubuntu Cluster Setup for Cloudlab
This program is designed to install the necessary packages and configurations
to create a simple HPC cluster on a cloudlab expirement.

What it does:
* updates all packages
* installs slurm, openmpi, mpich, libmpi, rdma-core, rdmacm-dev
* installs NFS and mounts the /users directory (you can change this)
* optionally installs the latest version of tcpdump and libpcap
* optionally installs and configures libfabric and Sandia OpenSHMEM
### How to use
1. Clone this repository onto your local machine.
1. If you do not have ansible installed run runme.sh first.
1. Replace the hostnames in inventory.yml with the names of the machines in your expirement, where "server" is the machine you want to be the head node.
1. run the command `ansible-playbook main.yml`

## Configuration options:
The cluster setup can be customized by changing the variables located in `vars.yml`

NFS:
* By default the `/users/` directory will be mounted. You can change this by changing the values of `nfs_exports` and `nfs_mounts`
* `nfs_rdma_enabled` will configure the nfs server to communicate over rdma. If you set this to false it will use TCP (default)

SLURM:
* good luck.

Some optional development tools are included:
* `new_tcpdump: true` will install the most up to date version of libpcap and tcpdump.
* `mellanox_debug_tools_enabled: true` will install the mellanox firmware tools. This will only work if the nodes contain a mellanox network card.

## TODO's
need to fix the nfs stuff for fs/nfsd/portlist to clobber the file and reupload instead of just adding entries.

Make it so the local interface doesn't need to be hardcoded in slurm.conf and nfs exports
## Aknowladgements
Development financially supported by University of Utah ANSR lab
