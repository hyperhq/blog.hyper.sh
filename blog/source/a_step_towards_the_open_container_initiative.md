title: runV - A step towards the Open Containers Initiative
date: 2015-08-11 5:30:00
author: tibo
tags:
    - runV
    - OCI
    - Open Containers
preview: When the development of Hyper started several months ago, we define its mission to be "Make VM run like Container", because we think that the most profound and impactful idea behind Docker is to be "App Centric".

---

When the development of [Hyper](www.hyper.sh) started several months ago, we define its mission to be "***Make VM run like Container***", because we think that the most profound and impactful idea behind [Docker](www.docker.com) is to be "***App Centric***".

We believe that this philosophy is not only limited within containers. By expanding the idea to the hypervisor, we could provide more options to developers and community, and thus enable [tools of mass innovation](https://www.youtube.com/watch?v=apOEYhmskvQ).

This is why we have been really excited by the newly released [Open Containers Initiative](http://www.opencontainers.org/) (or OCI), with [runC](https://github.com/opencontainers/runc) as a first Container runtime implementation of the Open Containers Format.

Now, we are excited to propose [runV](https://github.com/hyperhq/runv), as the hypervisor-based runtime of OCI. We encourage everyone to try, test, and contribute to runV, so we can build the future of the world's infrastructure together!

Note: You can find the list of OCI implementation on the [official OCI repository](https://github.com/opencontainers/specs/blob/master/implementations.md).
