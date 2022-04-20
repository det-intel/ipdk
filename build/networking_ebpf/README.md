# IPDK Networking With eBPF

This directory contains the recipe for running IPDK networking with eBPF.

A high-level overview of the architecture is as follows:

![IPDK eBPF Architecture](architecture/demo-Step0.png)

## Running

### Running Natively

If you want to run natively (on bare metal or in a VM), do the following:

```
REPO=<root to your ipdk repository>
$ cd $REPO/build
$ sudo ./ipdk install
$ export PATH=`pwd`:$PATH
$ sudo ./ipdk install ebpf-ubuntu2004
$ sudo ./networking_ebpf/vagrant/provision.sh
$ sudo ./networking_ebpf/scripts/host_install.sh
```

This should put p4c and other executables into your path. You should then be
able to compile the example program simple_l3.p4 as follows. Assuming you want
the results placed in the directory RESULTS,

```
RESULTS=<where you want compiler products placed>
cd $REPO/build/networking_ebpf/ipdk-ebpf/p4c/backend/ebpf
make -f ./runtime/kernel.mk BPFOBJ=$RESULTS/out.o P4FILE=$REPO/build/networking_ebpf/examples/simple_l3.p4 ARGS="-DPSA_PORT_RECIRCULATE=2" P4ARGS="--Wdisable=unused -I/usr/local/share/p4c/p4include/dpdk" psa
```

### Vagrant

The recipe has a built-in Vagrant virtual machine. To run this, you can
simply do the following:

```
$ cd vagrant
$ vagrant up
```

Once the machine is booted and provisioned, you can then login to the virtual
machine and finish running the recipe setup.

```
$ cd vagrant
$ vagrant ssh
$ /git/ipdk/build/networking_ebpf/scripts/host_install.sh
```

This will install the following components of the IPDK eBPF recipe:

* protobuf
* P4 compiler with eBPF PSA support
* psabpf CLI
* psa-ebpf-demp
* An old version of golang
* ipdk-plugin Docker CNM

## Docker Instructions

See the [README_DOCKER.md](README_DOCKER.md) for Docker specific instructions.
