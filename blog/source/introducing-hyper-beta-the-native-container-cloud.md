title: Introducing Hyper_ (Beta) - The Native Container Cloud
date: 2016-04-04 00:00:00
author: hyper
draft: true
preview: Today, we are very happy to announce Hyper\_ (beta) . Different from traditional container services, Hyper\_ is a native container cloud, in which multi-tenant containers can inherently be run safely side by side on bare metal, instead of being nested in VMs.

---

Today, we are very happy to announce Hyper\_ (beta) . Different from traditional container services, Hyper\_ is a native container cloud, in which **multi-tenant containers can inherently be run safely side by side on bare metal**, instead of being nested in VMs.

### Secure Container
Hyper\_ is built on [HyperContainer](https://github.com/hyperhq/hyper) , a virtualized container technology we open-sourced. HyperContainer addresses the security problem in Linux Containers by replacing:

- `Cgroup + Namespace` ---> `Hypervisor (KVM, Xen, etc)`
- `Host Kernel` ---> `Guest Kernel`

![](https://trello-attachments.s3.amazonaws.com/5694785e124f36d746f5c7be/1511x393/b8b5cd31b59af44c0c86349e150438fb/HyperContainer_vs_LinuxContainer.png)

As for performance, **a HyperContainer is able to boot in less than a second (~100ms)**, giving similar performance to a Linux Container.

### Docker Native Workflow

![](https://trello-attachments.s3.amazonaws.com/55545e127c7cbe0ec5b82f2b/879x320/5471e40d4a519c3d31f455bdccc978ca/upload_2_3_2016_at_3_50_31_PM.png)

Thanks to the built-in isolation, HyperContainer replaces VMs as the building block of Hyper\_.  More importantly, without the underlying VM hosts, the need for a cluster is gone, along with things like capacity planning, VM configuration, COE, and cluster utilization rates. Thus, **Hyper\_ works like one single remote Docker host with unlimited capacity**.

There is no need to learn a new set of commands. The Docker native workflow makes working with HYPER_ as simple as running containers on your own laptop:

- `hyper pull`: pull images from Docker hub  to your Hyper\_ account
- `hyper run`: launch a HyperContainer without VM or scheduling
- `hyper run --link`: link multiple containers as they run on one machine
- `hyper exec`: login your container or execute commands
- `hyper fip associate`: associate floating public IPs to enable Internet access and mask application failures

### Pay Per Second
You can spin up 100 containers in 2 seconds, crunch some data or run parallel builds on the lastest commit for 10s, destroy all containers in 1s, and Hyper\_ will only charge for 2+10+1=13s. This Per Second model not only promises cost saving, also advocates event-driven patterns such [AWS Lamda](https://aws.amazon.com/lambda), [Spark Streaming](http://spark.apache.org/streaming/), and [CD/CI](https://10second.build/).

![](https://trello-attachments.s3.amazonaws.com/56b19c6e5bb4a89f92d0e71f/903x472/2ccb5880a4286dd6d4c14eb19b3dab99/upload_2_3_2016_at_2_21_34_PM.png)

### Persistent Storage
We've built a distributed, replicated, reliable block storage system in Hyper\_ to offer you the persistent volume/snapshot features you need to run your stateful workloads. And it is powered by full high-performant SSD!

### 10Gbps Network for FREE!
We use the latest 10Gbps network to run your container. The best part is that all traffic (in, out, inter-container) is FREE!

### (Closing) Make the cloud run like your laptop
