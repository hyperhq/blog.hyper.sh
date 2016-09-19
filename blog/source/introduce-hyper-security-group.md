title: "Introduce Hyper_ Security Group (and more)"
date: 2016-09-19 14:24:00 +0800
author: hyper
tags:
    - Docker
    - Container
    - Container-native cloud
    - Hyper
preview: We are excited to introduce you the new Hyper_ Security Group, CLI Auto Upgrade, and more!

---
At Hyper_, our mission has always been **Make running containers in production effortless**. In the past few weeks, we keep collecting feature requests from our community to prioritize [our roadmap](https://trello.com/b/7fEwaPRd/roadmap). Today, we have some really exciting news for you.

> **Please [install the new CLI](https://docs.hyper.sh/GettingStarted/install.html)**.

### Security Group

Since [our GA](https://blog.hyper.sh/hyper-is-generally-available.html), one of the top feature requests is **Security Group**. By controlling the traffic for containers, it provides developers greater security and manageability:

> - `hyper sg create web_sg -f web_sg.yaml`: create a security group by adding rules that allow traffic to or from its associated containers
> - `hyper run -d --sg=web_sg --name web nginx`: specify one or more security groups with the container; when we decide whether to allow traffic to reach a container, we evaluate all the rules from all the security groups that are associated with the container
> - `hyper update nginx --sg_add=new_sg`: add/remove the security groups associated with containers
> - `hyper sg update web_sg -f web_sg2.yaml`: modify the rules for a security group on the fly; the new rules are automatically applied to all containers that are associated with the security group

More details can be found at [Roadmap](https://trello.com/c/ZbGtfwZt/37-security-group), [Documentation](https://docs.hyper.sh/Reference/sg_ref.html).

### CLI Auto Upgrade

Everyone loves command line tools! It is the enormously powerful to be able to control everything at fingertips. However, the trade-off is manual update. With this release, the built-in functionality will upgrade`hyper` CLI whenever there is a newer version. It helps you to automatically keep your local utility up to date. 

### Faster Container Deployment
If a Docker image is built with the `VOLUME` instruction, it creates a mount point with the specified name and marks it as holding externally mounted volumes, which will be initialized upon `run` or `create` commands. The initialization process could take tens of seconds or even minutes to complete, depending on the data size.

The new `--noauto-volume` option allows faster container deployment, by skipping `VOLUME` flag in the Docker image. 
``` bash
	$ hyper run --noauto-volume`.
```

### Multiple Mountpoints
You can mount a volume at multiple different paths of a container.
``` bash
	$ hyper run -v vol1:/opt/data -v vol1:/opt/log --name=mycontainer ubuntu
```

### Large Local Data Support
`hyper run -v /local:/container` allows you to load local data to remote containers upon launching. Previously the data size limit is a few hundreds MBs. The new release supports up to 10GB!

### Enhanced Container Resource Limits
The new performance patch improves the container resource limits:
- Max open files: 1000000
- Max processes: 30604
- Max pending signals: 30604:30604

---------------------

We hope that you will enjoy these new features in the new Hyper_. Please stay tuned on [our roadmap](https://trello.com/b/7fEwaPRd/roadmap) as we continue to add more exciting features and if you have any questions you can always contact us through the Hyper_ console, Hyper_ forum, or on support@hyper.sh.

Thanks,

The Hyper_ Crew






