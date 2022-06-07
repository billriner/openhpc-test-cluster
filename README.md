# OpenHPC Test Cluster

This repository provides the utilities necessary for quickly building an OpenHPC
test cluster using VirtualBox VMs and Vagrant.

## Prerequisites

Install the following software.

- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [VirtualBox Oracle VM VirtualBox Extension Pack](https://www.virtualbox.org/wiki/Downloads)
- [Vagrant](https://www.vagrantup.com/)

## Building and Running

To build a cluster of `$N` nodes, where `1 <= $N <= 10`, run:

    make NCOMPUTES=$N

A directory named `cluster` will be built. Change into the `cluster` directory
before running subsequent commands.

To start the SMS, run:

    vagrant up

Provisioning the first time will take approximately 24 minutes. Once the SMS is
running, you can SSH into it:

    vagrant ssh

If you are doing Slurm development, you can run the following script to start up
Slurm and establish some default accounting settings.

    /vagrant/slurm-setup.sh

Compute nodes are named `c1`, `c2`, ..., `c$N`. For any given compute node, say
`c$i`, you can start it with:

    vagrant up c$i

When booting compute nodes, Vagrant will be unable to communicate with them as
they only have an internal network. Ignore the error message Vagrant spits out
after 10 seconds of booting. The SMS will provision the compute nodes, which
will take approximately two minutes.

The ordinary Vagrant `halt`, `destroy`, and `status` commands may be useful as
well.

## Modifications for Vanderbilt CSB requirements

1. Add an interface with a public IP address to the cluster nodes.
2. Configure all nodes as NIS clients and the head node as a NIS slave server.
3. Use CSB slurm configuration files.
4. Add required additional CSB packages.
5. How to handle compute nodes with differing hardware?  e.g. GPUs and Infiniband (different images?)
7. Change to statefull provisioning so that the OS runs from disk instead of memory.
8. Change hostnames.
9. Update packages on the head node before creating compute node image.
10. We are not using IPMI since the hsradware does not support it (need to verify this).
11. Allow users to log into nodes where they do not have jobs running.

## Networking

The DHCP Server will automatically supply this information to the nodes.  IP addresses are DHCP-assigned static.

- VLAN:             1293 (pvta-hill-107)
- Subnet Mask:      255.255.255.0
- Default Gateway:  10.2.188.1
- DNS Servers:      10.2.189.78, 10.4.162.82

- sms
  IP Address:   10.2.188.44
  MAC Address:  52-54-00-26-10-60
  
- c1
  IP Address:   10.2.188.48
  MAC Address:  22-1a-2b-00-00-44

- c2
  IP Address:   10.2.188.47
  MAC Address:  22-1a-2b-00-00-55

- c3
  IP Address:   10.2.188.46
  MAC Address:  22-1a-2b-00-00-66

- c4
  IP Address:   10.2.188.45
  MAC Address:  22-1a-2b-00-00-77



