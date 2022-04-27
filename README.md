# Ubuntu Cluster Setup for Cloudlab
This program is designed to install the necessary packages and configurations
to create a simple HPC cluster on a cloudlab expirement.

What it does:
* updates all system packages
* installs NFS and mounts the `/users` directory. mountpoint and protocol can be changed, see configuration variables below.
* installs Lmod
* installs openMPI and Sandia openSHMEM, and configures them to be used as modules through lmod
* installs slurm, rdma-core, rdmacm-dev
* optionally installs the latest version of tcpdump and libpcap (optional, see config variables)
* optionally installs several other research/sysadmin tools  (optional, see config variables)
### Usage:
1. Clone this repository onto your local machine.
1. Replace the hostnames in inventory.yml with the names of the machines in your expirement, where "hdeadnode" is the machine you want to be the head/login node.
1. Make the necessary changes in vars.yml
1. run the script runme.sh (this will install ansible if not already installed)

## Configuration options:
The cluster setup can be customized by changing the variables located in `vars.yml`

NFS:
* By default the `/users/` directory will be mounted. You can change this by changing the values of `nfs_exports` and `nfs_mounts`
* `nfs_rdma_enabled` will configure the nfs server to communicate over rdma. If you set this to false it will use TCP (default)

SLURM:
* The defult partition will be named "debug" you can name it whatever you want though.
`slurm_cpus_per_node` defines how many processes can run on a single node. Set this to your desired value (must be less than or equal to the machine's core count)

Some optional development tools are included:
* `new_tcpdump: true` will install the most up to date version of libpcap and tcpdump.
* `mellanox_debug_tools_enabled: true` will install the mellanox firmware tools. This will only work if the nodes contain a mellanox network card.
* `Sandia_OpenSHMEM: true` will install SOS-1.5.0

## TODO's
* need to fix the nfs stuff for fs/nfsd/portlist to clobber the file and reupload instead of just adding entries.

* Make it so the local interface doesn't need to be hardcoded in slurm.conf and nfs exports

* Check the hashes on downloaded tarballs

* add option to compile openMPI with libfabric instead of ibverbs

* currently, lmod is configured to work with bash and csh. Should probably add other shells. (This means your default shell must be one of those for lmod to work correctly)
## Acknowledgements
Development financially supported by University of Utah ANSR lab
