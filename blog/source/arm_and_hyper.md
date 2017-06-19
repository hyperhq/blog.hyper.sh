title: "ARM and Hyper Team to Bring Multi-tenancy to Container for IoT, Edge and 5G"
date: 2017-06-19 12:44:00 +0800
author: hyper
tags:
    - ARM
    - Hyper
    - Container
    - IoT
    - Edge
preview: ARM and Hyper announced a joint engineering effort in runV project, to bring multi-tenancy to container, for IoT, Edge and 5G.

---

June 19, 2017

Today, ARM (ARM Holding plc) and Hyper (HyperHQ Inc.) announced a joint engineering effort in [runV](github.com/hyperhq/runv) project, to bring multi-tenancy to container, for IoT, Edge and 5G.

Container is getting momentum to package, distribute and run applications. However, as containers share the host kernel, they are not considered as secure as VMs in multi-tenant environment. To address this problem, a new open-source project, [runV](github.com/hyperhq/runv), is created by Hyper. For those who are familar with [runC](https://github.com/opencontainers/runc), runV is the hypervisor-based equivalent (runtime). It leverages typical hypervisor technology (KVM, Xen, etc.) to launch container images, instead of Linux Container (Cgroup, Namespace).

![](https://trello-attachments.s3.amazonaws.com/5694785e124f36d746f5c7be/1264x555/e027c03c35b4da0682e918959fa81bea/LinuxContainer_vs_HyperContainer.png)

runV is able to launch a Docker (or OCI) image into a micro VM in 100ms, and still keeps the hardware-enforced isolation in traditional virtualization. The unique combination of virtualization and containerization allows runV to bring the best of both worlds:

![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/592270791d50da8d8a38e52c/1942b19ee1327be619a66366c11e66b8/combine_the_best_of_both_worlds.png)

| -  | Container| VM | Hyper |
|---|---|---|---|
| Isolation | Weak, shared kernel | Strong, HW-enforced  | Strong, HW-enforced  |
| Portable  | Yes | No, hypervisor dependent | Yes, hypervisor agnostic and portable image |
| Boot  | Fast, sub-second  | Slow, tens of seconds  | Fast, sub-second  |
| Performance  | Great | OK| Good, minimal resource footprint and overhead |
| Density (instance) | 10k+ | ~500 | ~5k |
| Immutable | Yes, cattle  | No, pet | Yes, cattle  |
| Image Size| Small, MBs  | Big, GBs  | Small, MBs  |
| Heterogeneous Workload | No, kernel depency | Yes | Yes, Linux/Windows, BYOK  |
| Device Emulationb | No  | Yes | Yes |

runV is a stack-agnostic technology. It supports multiple hypervisors (KVM, Xen, VirtualBox), as well as multiple processors (x86, ARM, OpenPower, s390x). By joining efforts with Hyper, ARM aims to invest engineering resources into runV project, leading the technology for the future IoT and Edge Computing.

"The powerful combination of virtualization and containerization in runV will offer developers high levels of security, manageability and scale," said Tony Chen, Engineering Director,  ARM. "It enables a new form of public infrastructure as we see today in AWS, Azure, Google Cloud, which is container-native, application-centric".

### About ARM
ARM® technology is at the heart of a computing and connectivity revolution that is transforming the way people live and businesses operate. Every day more than 45 million ARM-based chips are shipped by our partners into products that enhance the human experience; connecting people, improving lives and making the impossible possible. From the sensor to the cloud and all points in between, ARM is shaping the smart connected world. 

### About Hyper
Hyper (HyperHQ Inc.)  builds innovative, open source technology that makes it easy to deploy and manage secure containers in multi-tenant environment. We enable organizations to accelerate all aspects of their software pipeline with container-native public infrastructure.

