title: "runV - bring isolation to Docker"
date: 2016-06-20 23:00:00 +0800
author: hyper
tags:
    - Container
    - Hyper
    - runV
preview: runV is a hypervisor-based implementation for OCI runtime, which functions similarly to runC. The difference is that runV does not use cgroups and namespaces, but a hypervisor to run the (Docker) image.

---

Hi,

We are thrilled to announce the release of Docker and [runV](http://github.com/hyperhq/runv) integration.

# What is runV?

runV is a hypervisor-based implementation for [OCI runtime](https://github.com/opencontainers/runtime-spec/blob/master/implementations.md), which functions similarly to runC. The difference is that runV does not use cgroups and namespaces, but a hypervisor to run the (Docker) image.

Traditional VMs are slow to boot and heavy on resources. However, they excel at strong isolation through having an independent guest kernel. The goal of runV is to "**combine the best of both worlds**", e.g. the security of a VM and the speed of container. Currently runV (v0.6) is able to launch a Docker image in 100-300 milliseconds, and has comparable density to containers.

# What is this integration about?

Simply put, it gives you a seamless experience to manage isolated "containers" with the standard `docker` CLI (1.11) .
Though these are essentially ultra-light VMs, you will be able to access them like containers using `docker attach` and `docker exec` commands.

# How to play with it?
Dependencies:
	- Kernel 4.0 or later
	- QEMU 2.1 or later

Build form source:

``` bash
[root@localhost ~] cd $GOPATH/src/github.com/hyperhq
[root@localhost ~] git clone https://github.com/hyperhq/runv/
[root@localhost ~] cd runv
[root@localhost ~] ./autogen.sh
[root@localhost ~] ./configure --without-xen
[root@localhost ~] make
[root@localhost ~] sudo make install
```

Try Busybox:
``` bash
# in terminal #1
[root@localhost ~] runv-containerd --debug --driver libvirt --kernel /opt/hyperstart/build/kernel --initrd /opt/hyperstart/build/hyper-initrd.img
# in terminal #2
[root@localhost ~] docker daemon -D -l debug --containerd=/run/runv-containerd/containerd.sock
# in terminal #3 for trying it
[root@localhost ~] docker run -ti busybox
/ # ls   
bin   dev   etc   home  lib   proc  root  sys   tmp   usr   var
/ # exit
[root@localhost ~]
```

# Finally

See you guys in our Booth (S20) at DockerCon Seattle.

- [1] https://github.com/opencontainers/runtime-spec/blob/master/implementations.md
- [2] https://blog.docker.com/2016/04/docker-engine-1-11-runc/
- [3] https://github.com/hyperhq/runv/blob/master/containerd/README.md
