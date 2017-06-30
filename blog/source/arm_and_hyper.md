title: "ARM and Hyper Team to Bring Secure Container for IoT, Edge and 5G"
date: 2017-06-30 12:44:00 +0800
author: hyper
tags:
    - ARM
    - Hyper
    - Container
    - IoT
    - Edge
preview: ARM and Hyper announced a joint engineering effort in runV project, to bring secure container, for IoT, Edge and 5G.

---

> Working to demonstrate benefits of runV technologies for container applications

Shanghai, June 30, 2017 - Today, Hyper (HyperHQ Inc.) announced through the [runV](https://github.com/hyperhq/runv) project plans to bring to bring multi-tenancy to container, for IoT, edge compute, and 5G.

The momentum around containers is gaining traction as to package, distributed, and run applications increases. However, as containers share the host kernel, they are not considered as secure as virtual machines (VM) in a multi-tenant environment. To address this problem, a new open-source project, [runV](https://github.com/hyperhq/runv), has been created by Hyper and will feature hypervisor-based runtimes that are equivalent to the already available [runC](https://runc.io/). runV leverages typical hypervisor technology (KVM, Xen, etc.) to launch container images, instead of Linux Container (Cgroup, Namespace).

runV will be able to launch a Docker (or OCI) image into a micro VM in 100ms, and still keeps the hardware-enforced isolation in traditional virtualization. The unique combination of virtualization and containerization allows runV to bring the best of both worlds:

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

Currently, runV has preliminary support on ARM servers, however with container multi-tenancy developers can expect to see performance optimizations and density improvements. In working together on the runV project, Hyper and ARM aim to demonstrate the benefits of runV and how well suited it is to ARM-based platforms for the next generation of IoT, 5G, and edge computing.

## About Hyper

Hyper (HyperHQ Inc.) builds innovative, open source technology that makes it easy to deploy and manage secure containers in multi-tenant environment. We enable organizations to accelerate all aspects of their software pipeline with container-native public infrastructure.

