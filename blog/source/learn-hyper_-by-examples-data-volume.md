title: "Learn Hyper_ By Examples: Manage Data Volume"
date: 2016-05-12 20:00:00 +0800
author: hyper
tags:
    - Container
    - Hyper
    - Volume
preview: This guide teaches you how to manage persistent data in the Hyper_ cloud.

---

[Hyper_](https://www.hyper.sh/) is a secure container cloud service. It allows you to deploy your containers in seconds from your laptop to the cloud.

This is the last article of "Learn Hyper_ by Examples" series. So far you’ve been introduced to basic Hyper_ concepts, seen how to work with real applications as well as learned about networking and links between containers. In this section you’re going to learn how to manage the persistent data volumes in Hyper_ cloud.

> **Prerequisites:**
> Please make sure you complete the following prerequisites:

> - [Open a Hyper_ account](https://console.hyper.sh/register)
> - [Generate API credential](https://docs.hyper.sh/GettingStarted/generate_api_credential.html)
> - [Setup `hyper` CLI on your local computer](https://docs.hyper.sh/GettingStarted/install.html)

## Data volume
A data volume in Hyper_ is a persistent storage system for container. It is a specially-designated directory within one container that does not belong to the container image file system. Data volumes provide several useful features for persistent or shared data:


- Volumes are initialized when a container is created. If the container's base image contains data at the specified mount point, that data is hidden.
- Persistent Data volumes can't be shared among containers.
- Changes to a data volume are made directly.
- Changes to a data volume will not be included in the image.
- Data volumes is persist even if the container itself is deleted.
- Data volumes is stored in high available distributed system.
- Snapshot can be taken from data volume.

Data volumes are designed to persist data, independent of the container's life cycle. Hyper_ therefore *never* automatically deletes volumes when you remove a container, nor will it "garbage collect" volumes that are no longer
referenced by a container.


## Adding a anonymous volume

You can add a data volume to a container using the `-v` flag with the
`hyper run` command. You can use the `-v` multiple times
to mount multiple data volumes. Now, mount a single volume in your web
application container.

```shell
$ hyper run -d --name web -v /webapp training/webapp python app.py
77e5095b0871f24f4160578980d61a787fb555e4f26d8c7a32eb64b5e155ad66
```

This will create a new volume inside a container at `/webapp`. And you can see that this volume has no specified name.

```shell
$ hyper volume ls
DRIVER              VOLUME NAME                                                        SIZE                CONTAINER
hyper               51c6a102427d6f57cb8b0563b74ac76c35fb5bc580f33156e5f16b436cd7305    10 GB               77e5095b0871
```

You can describe the volume details by `hyper volume inspect` command:
```shell
$ hyper volume inspect 51c6a102427d6f57cb8b0563b74ac76c35fb5bc580f33156e5f16b436cd7305
[
    {
        "Name": "51c6a102427d6f57cb8b0563b74ac76c35fb5bc580f33156e5f16b436cd7305",
        "Driver": "hyper",
        "Mountpoint": "",
        "Labels": {
            "container": "77e5095b0871f24f4160578980d61a787fb555e4f26d8c7a32eb64b5e155ad66",
            "size": "10",
            "snapshot": ""
        }
    }
]
```

## Manage volumes

Besides creating volumes during `hyper run`, you can also create and manage volumes using the `hyper volume` command.

```shell
$ hyper volume --help

Usage:  hyper volume [OPTIONS] [COMMAND]

Manage Hyper_ volumes

Commands:
  create                   Create a volume
  inspect                  Return low-level information on a volume
  ls                       List volumes
  rm                       Remove a volume

Run 'hyper volume COMMAND --help' for more information on a command

  --help             Print usage
```
So this time, we'll create a volume with name `vol1` first.

```shell
$ hyper volume create --name vol1
```

Now we run a database container which claims `vol1` as volume, and then writes "hello" to this directory.
```shell
$ hyper run -it --name db-1 -v vol1:/tmp hyperhq/postgres /bin/shell
root@ba047872509b:/#  echo hello >>  /tmp/test.txt
```
* `-v <volume-name>:<target-dir>` creates a volume using given name and mounts it into the target directory inside the container. Hyper_ cloud uses industry proven distribution file system to persist data in the target directory until the volume is destroyed.

Then we stop the database container and create a snapshot of this volume.
```shell
$ hyper stop db-1
$ hyper snapshot create --name mybackup --volume vol1
```

The main benefit of a snapshot is that you can create a volume from it. This makes it possible for you to migrate volumes from one container to another, it's essential for data high availability.

Now let's create a volume from a previous snapshot.

```shell
$ hyper volume create --name vol2 --snapshot mybackup
```

And we can see the data written by the previous container is preserved.
```shell
$ hyper run -it --name db-2 -v vol2:/tmp hyperhq/postgres cat /tmp/test.txt
hello
```

> **NOTE:**
> For full usage of `hyper volume create` command, please check the [Hyper_ official cli guide](https://docs.hyper.sh/Reference/CLI/volume_create.html).


## Conclusion

Persistent volume storage is important for container clouds since traditional OS level container volumes tend to be bound to the local directory of their hosts. But in Hyper_ cloud, volumes are implemented with a mature distributed file system directly since there are no shared hosts. We strongly recommend that you preserve valuable data in a Hyper_ volume and maintain them carefully with the help of Hyper_ cloud.

Thanks for following along in this "Learn Hyper By Examples" blog series.  You should now be familiar with some of the most important features of Hyper_ cloud,  including: "How to use hyper cli", "How to deploy applications to Hyper_ cloud", "How to link containers",  and "How to manage container's persistent data".
We hope you have enjoyed this blog series and please continue to explore more of the Hyper_  cloud features on  [Hyper_](https://console.hyper.sh/register)!
