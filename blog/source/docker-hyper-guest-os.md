title: Docker, Hyper and the end of Guest OS
date: 2015-06-29 17:00:00
author: me
tags:
    - Container
    - Docker
    - Guest
preview: Caas (Container-as-a-Service) is believed by many to be the next big thing in the cloud. It follows the philosophy of PaaS (shift the focus from infrastructure to app), but leverages the portability of Docker to avoid various technical limitations in a PaaS service

---

Caas (Container-as-a-Service) is believed by many to be the next big thing in the cloud. It follows the philosophy of PaaS (shift the focus from infrastructure to app), but leverages the portability of Docker to avoid various technical limitations in a PaaS service. As CaaS rapidly takes mind share among developers, it is not surprising that not only the startup world is fanatic about it, but also big boys like AWS and Google released their CaaS offerings (AWS ECS and Google GKE).

Among all these new shining sexy CaaS services, most of them are built with a common architecture of **IaaS+Container**. The logic behind is straightforward: as Docker is built on container technology, the **“Shared Kernel”** nature of containers makes Docker lacking of the mandatory secure isolation for a multi-tenant public platform. The hardware-enforced isolation in VMs turns out to be a perfect answer.

![](https://d262ilb51hltx0.cloudfront.net/max/800/0*8QTU9ek4QhOBoWPJ.)

Given above, CaaS is a form of Managed VM service, which heavily relies on the GuestOS. However, it introduces several downsides:

- **Not 100% Managed:** In this approach, developers still have the access to the underlying infrastructure, and hence need to take care of the unmanaged parts, e.g. capacity planning, VM instance flavor (size, distro), VPC network configuration, storage setup (LVM, RAID, filesystem, mountpoints), etc. And all of these parameters change frequently in today’s DevOps era, what means that a new endless management hell is waiting.

- **Over Capacity:** This architecture requires you to pre-build a VM pool and schedules your Docker containers in that pool. Though it might be able to autoscale out/in upon bursting, I’d say that there are always some VM resources in the pool sitting idle at any time. After all, capacity planning would never make you lean. Cloud ought to be **Just Fit**.

- **Doubled Complexity:** So, I build my VM pool on top of some IaaS platform for this CaaS service. Now, what about other nice and advanced features, like VPC, EBS or RDS? Should I check my CaaS to see if they have the container version of these, or just import from the underlying IaaS?
*** **CaaS:** OK, you are telling me to create another container-based VPC and subnets inside of my VM pool, which is already built with an IaaS VPC and various public/private subnets. I’m sorry, that sounds crazy!
*** **IaaS:** Right, Let me just use the IaaS VPC feature to setup the entire network. But who is going to take care of the convergence of my network, in case of autoscaling and failover, the CaaS provider or me? And, if I create subnets in the pool, does that mean there are several sub-pools, instead of a single shared one, for resource scheduling? Well, I’m concerned by the overall utilization rate.

# What is Hyper

Simply put, Hyper is a hypervisor-agnostic Docker runtime. It allows you to run Docker images with any hypervisor (KVM, Xen, etc.).

``` shell
[root@user ~:]# docker pull nginx:latest
[root@user ~:]# hyper run nginx:latest
[root@user ~:]# docker ps
[root@user ~:]#
[root@user ~:]# hyper list
......
Done
```

Different from the VM+Container approach, Hyper does not employ GuestOS in the VM instance; instead a **HyperKernel** is loaded to host the Docker images. This minimalist approach not only contributes to the immutable HyperVM, but also results in some encouraging performance:

- Takes milliseconds to launch a new HyperVM with a pod of Docker images
- As tiny as 20MB, for the memory footprint of a HyperVM (compared with 512MB on AWS EC2)

![](https://d262ilb51hltx0.cloudfront.net/max/1030/0*WDOCJCofJrd9QVcm.)

Moreover, thanks to the hypervisor, Hyper is immune from the shared-kernel problem in container. Combined with its lighting performance and immutability, now we can think over the way to build a secure, public, multi-tenant CaaS.

# CaaS = Hyper + Metal

![](https://d262ilb51hltx0.cloudfront.net/max/800/0*ACEMTU-ARMKtTCXi.)

In a Hyper-enabled CaaS, developers simply create the app spec (Docker Compose, CoreOS Fleet, etc.), and submit it to the platform, which takes care of all heavy liftings, like fetching the Docker images and launching HyperVM to run the app. No configuration management is required in HyperVM, and all app changes/upgrade is managed in the *“Infrastructure-as-Code”* way, e.g. the app-spec file (so the virtue of version-control, collaboration, etc.).

For the things like VPC network, persistent storage, Hyper completely avoids the doubled complexity mentioned above, by leveraging the mature solutions in the hypervisor world.

And, this approach delivers the true **Just-Fit** cloud. The prebuilt VM pool is gone, as well as the need of capacity planning, hence no more wasted cost.

# Finally,

With all said, this doesn’t conclude the end of OS. On the contrary, it is the end of the beginning for the new minimalist distros to rule the world by running on metal to power the next generation of CaaS!
