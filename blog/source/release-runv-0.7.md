title: "Release runV 0.7"
date: 2016-10-29 00:00:00 +0800
author: hyper
tags:
    - Docker
    - Containers
    - runV
    - HyperContainer
preview: "Today, we are happy to announce the new v0.7 release of runV and HyperContainer, bringing them to more platforms: ARM64, Power and Mainframe (s390x)."

---

For those who have been following us since the early days, you may know that Hyper started as two open source projects: [HyperContainer](http://www.hypercontainer.io), which pioneered the territory of virtual containers, and [runV](https://github.com/hyperhq/runv), the hypervisor-based equivalent of [runC](https://github.com/opencontainers/runc). The combination of sub-second boot performance and VM-level isolation enables a new wave of container native cloud infrastructure, such as [Hyper.sh](https://hyper.sh).

Since day one, these projects have aimed to be **vendor neutral**, supporting multiple hypervisors (**KVM**, **Xen**, **VirtualBox**). Today, we are happy to announce the new v0.7 release of runV and HyperContainer, bringing them to more platforms:

- x86
- ARM64
- Power
- Mainframe (s390x)

## HyperContainer

- More platform supports: s390x, ppc64le, and arm64
- VM Template: faster boot performance (130ms) and less memory consumption (save 80MB per pod/VM, #415)
- Improve gRPC APIs.
- Improve streaming IO (attach & exec) for containers.
- Many other fixes and improvements 

## runV

- Support system arch s390x and ppc64le (#312)
- Support system arch ARM64 (#360)
- Enable VM template for runV and runV-containerd, which improves the boot performance to 130ms and reduces 80MB memory consumption per container. (#303, #304)
- Enable CNI, OVS and improve the networking configurations. (#286, #307)
- Add QoS Control for network interface. (#331)
- Allow one volume to be mounted to multiple mount points of one container. (#329)
- Improve streaming IO for containers
- Move dependencies from Godep to vendors
- Many other fixes and improvements 

## Furthure Information
- Code
    - https://github.com/hyperhq/runv
    - https://github.com/hyperhq/hyperd/
- Download
    - https://github.com/hyperhq/runv/releases
    - https://github.com/hyperhq/hyperd/releases
- Docs: http://docs.hypercontainer.io

--------------------

Thanks,

The Hyper Team
