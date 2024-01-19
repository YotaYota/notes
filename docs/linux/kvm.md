# KVM

KVM (Kernel Virtual Machine) is a hypervisor - a software layer that sits between the hardware and the VMs, managing resource allocation, scheduling, and communication.

Virtualization is the abstraction of computing resources from the hardware layer, allowing multiple virtual environments to run simultaneously on a single physical machine (the host) while ensuring full resource separation. These virtual environments, known as virtual machines (VMs) or guests, act as self-contained entities with their own operating systems, kernels and applications.

## Install and Test

```bash
sudo apt -y install bridge-utils cpu-checker libvirt-clients libvirt-daemon qemu qemu-kvm virt-manager
```

```bash
kvm-ok
```

```bash
sudo virt-install --name ubuntu-guest --os-variant ubuntu20.04 --vcpus 2 --ram 2048 --location http://ftp.ubuntu.com/ubuntu/dists/focal/main/installer-amd64/ --network bridge=virbr0,model=virtio --graphics none --extra-args='console=ttyS0,115200n8 serial'
```
