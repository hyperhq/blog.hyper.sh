title: Introduce Pi (Free Beta) - Expanding Serverless Container Platform to Google Cloud
date: 2018-04-23 17:25:00
author: Peng Zhao
preview: Today, we are excited to announce Pi, the new serverless container platform on Google Cloud. Pi provides the Kubernetes interface (pod), which allows allow direct integration with GKE and other GCP services.

---

Today, we are very excited to announce [_Pi_](https://hyper.sh/pi), our new serverless container platform on Google Cloud. We believe that it is a new evolution of [_Serverless_](https://en.wikipedia.org/wiki/Serverless_computing), which enables more applicartion patterns and use cases.

Let me explain.

## FaaS, Container and Serverless

_Serverless_, as a new pattern of application development and computing service, is pioneered by [AWS Lambda](https://aws.amazon.com/lambda/). Technically, _Lambda_ is defined as Function-as-a-Service, in which developers deploys individual functions (as source code) in AWS, who is responsible to handle function invocation, function input/output, scaling (in terms of concurrent requests), etc. 

Comparing with traditional long-running "Servers", _FaaS_ 



Different from the current [_Hyper_](https://hyper.sh/hyper) service,

- _Pi_ runs on top of Google Cloud
- It provides the Kubernetes sematics (pod), which allows direct and easy integration with GKE and other GCP services



You can sign up get your free access by following this link: https://hyper.sh/ . We are bringing new users on to the platform as quickly as possible and you’ll be updated by email as we proceed.

### Secure Container
Hyper.sh is built on [HyperContainer](https://github.com/hyperhq/hyper) , a virtualized container technology we open-sourced. HyperContainer addresses the security problem in Linux Containers by replacing:

- `Cgroup + Namespace` ---> `Hypervisor (KVM, Xen, etc)`
- `Host Kernel` ---> `Guest Kernel`

![](https://trello-attachments.s3.amazonaws.com/5694785e124f36d746f5c7be/1511x393/b8b5cd31b59af44c0c86349e150438fb/HyperContainer_vs_LinuxContainer.png)

As for performance, **a HyperContainer is able to boot in less than a second (~100ms)**, giving similar performance to a Linux Container.

### Docker Native Workflow

![](https://trello-attachments.s3.amazonaws.com/55545e127c7cbe0ec5b82f2b/879x320/5471e40d4a519c3d31f455bdccc978ca/upload_2_3_2016_at_3_50_31_PM.png)

Thanks to the built-in isolation, HyperContainer replaces VMs as the building block of Hyper.sh.  More importantly, without the underlying VM hosts, the need for a cluster is gone, along with things like capacity planning, VM configuration, COE, and cluster utilization rates. Thus, **Hyper\_ works like one single remote Docker host with unlimited capacity**.

There is no need to learn a new set of commands. The Docker native workflow makes working with Hyper.sh as simple as running containers on your own laptop:

- `hyper pull`: pull images from Docker hub  to your Hyper.sh account
- `hyper run`: launch a HyperContainer without VM or scheduling
- `hyper run --link`: link multiple containers as they run on one machine
- `hyper exec`: login your container or execute commands
- `hyper fip attach`: attach floating public IPs to enable Internet access and mask application failures

### Pay Per Second

We charge by the Container and We charge by the Second.

We charge you for the individual container, NOT a long running VM typically used to run your containers. Unlike AWS and Google, Hyper.sh is capable of directly launching a secure container without a VM, providing true agility and cost optimization.

We charge you by the second. You can spin up a container in 3 seconds, crunch some data or run parallel builds on the latest commit for 10s, destroy the container in 1s, and Hyper.sh will only charge for 3+10+1=14s.

This Per Second model not only promises cost saving, also advocates event-driven patterns such [AWS Lambda](https://aws.amazon.com/lambda), [Spark Streaming](http://spark.apache.org/streaming/), and [CD/CI](https://10second.build/).

![](https://trello-attachments.s3.amazonaws.com/56b19c6e5bb4a89f92d0e71f/903x472/2ccb5880a4286dd6d4c14eb19b3dab99/upload_2_3_2016_at_2_21_34_PM.png)

### Persistent Storage
We've built a distributed, replicated, reliable block storage system in Hyper.sh to offer you the persistent volume/snapshot features you need to run your stateful workloads. And it is powered by full high-performance SSD!

### 10Gbps Network for FREE!
We use the latest 10Gbps network to run your container. The best part is that all traffic (in, out, inter-container) is FREE!

### Make the cloud run like your laptop

We’re extremely excited to make Hyper.sh available to the world via this public beta and we’re looking forward to learning how people use the platform and where we can improve.

By repurposing the hypervisor from traditional full-blown VMs to the application-centric container, we think we’ve brought much-needed isolation to multi-tenancy, while keeping the signature tune of a Linux container: sub-second boot, with upgraded immutability and portability.

We believe that this is the nirvana for Cloud Native Applications. Developers can finally concentrate on the code, not being distracted by the server, because there is simply no such thing.

Want to try _Pi_ now? Get your access here: https://hyper.sh
