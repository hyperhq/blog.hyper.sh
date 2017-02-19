title: "Introducing Minio: Run you own S3 compatible cloud storage on Hyper.sh"
date: 2017-02-17 22:00:00 +0100
author: hyper
tags:
    - Docker
    - Container
    - Minio
    - S3
    - Storage
preview: "We've been working with the lovely people over at Minio to bring you S3 compatible storage on Hyper.sh!"

---

# Introducing Minio: Run your own S3 compatible cloud storage on Hyper.sh!

Storage has been long thought of as complex and difficult. The ephemeral nature of containers has further complicated the situation as it often seems counter-intuitive to store mission critical data on something that itself is supposed to be disposable.

[Minio](https://www.minio.io/) is an open source, S3 compatible, cloud-native object storage server that makes storage as easy as launching a Docker container. On Hyper.sh, Minio servers are backed by Hyper.sh volumes that make sure, even if a container running Minio server goes down, the data is safe in the volume. As a true cloud-native application, Minio scales very well in a multi-tenant cloud environment.

--

Although some of our users have been using Minio for a while, we've had other users asking for something similar, unaware that Minio existed.

To address this problem we joined forces with the Minio team to officialize the Minio deployment template.

## Why use Minio?

There are three main reasons why people have been using Minio on Hyper.sh:

1. Total control over object store configuration.
2. True cloud-native scalability.
3. Easy to setup and deploy.

## How to use Minio?

The Minio team helped us create these three reference Hyper Compose files for small, medium and large configurations.

Small - [link](https://gist.github.com/harshavardhana/14b2a472d661446fe5b0f602bc61ac82#file-s4-compose-yml)

Medium - [link](https://gist.github.com/harshavardhana/14b2a472d661446fe5b0f602bc61ac82#file-m3-compose-yml)

Large - [link](https://gist.github.com/harshavardhana/14b2a472d661446fe5b0f602bc61ac82#file-l3-compose-yml)

Note that, you'll first you need to create the storage volumes that Minio will use as backend storage.

For example,

```
hyper volume create --name minio-disk-1 --size=50
hyper volume create --name minio-disk-2 --size=50
hyper volume create --name minio-disk-3 --size=50
hyper volume create --name minio-disk-4 --size=50
```

This will create 4 volumes which in this case are each 50GB in size and will be treated as a storage pool for Minio to use. After that it is as simple as saving one of the above three compose files locally and running:

```
hyper compose up -f <YOUR COMPOSE FILE>
```

## That's it!

With Minio it really is that simple, and we're very pleased to be working with them to bring this functionality to the Hyper.sh platform.

We are always looking for feedback.

- Does this solve a problem for you?
- Are there any tools that you think we should be working more closely with?
- Are there any features that you think are currently missing from Hyper.sh?

To let us know you can always contact us on the in-console support chat, or on [the forum](https://forum.hyper.sh/) and for Minio specific questions please feel free to reach out to them or check their docs on [the Minio website.](https://www.minio.io/)
