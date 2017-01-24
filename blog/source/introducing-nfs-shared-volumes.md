title: "Introducing (NFS) Shared Volumes"
date: 2017-01-24 16:00:00 +0800
author: hyper
tags:
    - Docker
    - Container
    - Container-native cloud
    - Hyper
preview: At the beginning of 2017, nothing is more exciting than delivering one of the most requested features in Hyper's history, shared volumes!

---

# Introducing (NFS) Shared Volumes!

Going from open source technology to offering a public service in 2016 was a major shift for Hyper.sh, and at the beginning of 2017, nothing is more exciting than delivering one of the most requested features in Hyper's history: **Shared Volumes**!

## A quick review of volumes in Hyper
Since day one, Hyper.sh has supported [block volumes](https://docs.hyper.sh/Feature/storage/volume.html) that are comparable to [AWS EBS](https://aws.amazon.com/ebs/): inherently distributed, replicated, and persistent through failures. We also support [Snapshots](https://docs.hyper.sh/Feature/storage/snapshot.html), providing point-in-time snapshots for backup and recovery.

Previously in Hyper.sh, like AWS EBS, a block volume could only be attached to one container at a time.

However, because Docker has supported shared volumes across multiple containers, our users have consistently requested that Hyper.sh be able to do the same, and today we're very excited to announce that feature!

## Introducing (NFS) Shared Volumes
To support shared volumes, the first idea that popped up was to provide a scalable NFS service, in which you could create shares and mount them to any container.

Though the idea sounded cool, making an NFS service scalable, reliable and performant would require an enormous engineering effort. When we looked at the use cases, most users only required a small number of containers to to share volumes which gave us some more options.

We decided to look for a way that fit 90% of use cases while at the same time being quick and easy for everyone to understand and use.

This is what we came up with:

First you need to create a container using the `hyperhq/nfs-server` image which runs an optimized [nfs-ganesha](https://github.com/nfs-ganesha/nfs-ganesha) server and automatically exports all attached volumes via the NFS protocol. Once ready, you can mount the exposed volumes on the NFS container in other containers using the `--volumes-from` option.

For example,the following commands exposes two volumes (`/data1` and `/data2`) and mounts them in two `busybox` containers (`test1` and `test2`). `test1` and `test2` will each have these two volumes mounted at the path `/data` and `/data2`.

    $ hyper run --name mycontainer -d -v /data1 -v /data2 hyperhq/nfs-server
    $ hyper run -d --name test1 --volumes-from mycontainer busybox
    $ hyper run -d --name test2 --volumes-from mycontainer busybox

> NOTE: Recursive mounting is not allowed, e.g. you cannot re-mount the NFS volumes from containers `test1` and `test2` to a third one `test3`:

    $ hyper run -d --name test3 --volumes-from test1 busybox
    hyper: Error response from daemon: Cannot recursively import volumes from test1.
    See 'hyper run --help'.

> NOTE: `--volumes-from` source container must be created with the Hyper official image (`hyperhq/nfs-server`). Trying to import volumes from containers of other images will fail, e.g.,

    $ hyper run -d --name foo busybox
    $ hyper run -d --volumes-from foo busybox
    hyper: Error response from daemon: volumes-from source container is created from busybox rather than the official image hyperhq/nfs-server.
    See 'hyper run --help'.

## Your feedback Matters

Do you have feedback on this feature? Do you have ideas about what we should do next? Then please do share your thoughts with us in the [roadmap](https://trello.com/b/7fEwaPRd/roadmap)!!

--------------------------------------

Happy hacking,
The Hyper.sh Crew
