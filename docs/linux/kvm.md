# KVM

KVM (Kernel Virtual Machine) is a type 1 hypervisor - a software layer that sits between the hardware and the VMs, managing resource allocation, scheduling, and communication.

Virtualization is the abstraction of computing resources from the hardware layer, allowing multiple virtual environments to run simultaneously on a single physical machine (the host) while ensuring full resource separation. These virtual environments, known as virtual machines (VMs) or guests, act as self-contained entities with their own operating systems, kernels and applications.

## QEMU

QEMU is an emulator that can also be used as a virtualizer with the help of KVM to provide a native speed by accessing Intel VT-x or AMD V technology of modern processors.

## Virtual Machine Manager

`virt-manager`

GUI for KVM.

## Install and Test

Install KVM, QEMU and Virtual Machine Manager

```bash
sudo apt install bridge-utils cpu-checker libvirt-clients libvirt-daemon qemu qemu-kvm virt-manager
```

*Note*: bridge-utils is to use a bridged network adapter which allows VMs to be seen as real machines on the network.

Check that KVM is functioning

```bash
kvm-ok
```

Download iso image, eg from [ubuntu](http://cdimage.ubuntu.com/)


```bash
systemctl status libvirtd.service
```

Add user to `kvm` and `libvirtd`

```bash
sudo usermod -aG kvm $USER
sudo usermod -aG libvirtd $USER
```

Libvirt is a wrapper around kvm and qemu. The qemu commands themselves are very complicated.

## Sources

- [kvm, qemu  & virt-manager](https://linux.how2shout.com/how-to-install-qemu-kvm-and-virt-manager-gui-on-ubuntu-20-04-lts/)
- [tutorial](https://medium.com/@DrewViles/using-libvirt-kvm-qemu-to-create-vms-792e49262304)
