# Release

title: "Introducing Service"
date: 2016-10-21 16:44:00 +0800
author: hyper
tags:
    - Docker
    - Container
    - Container-native cloud
    - Hyper
preview: Service automatically do the load balancing across your containers, and provision new replicas to replace the bad ones.

---
At Hyper.sh, our mission has always been **Make running containers in production effortless**. In the past few weeks, we keep collecting feature requests from our community to prioritize [our roadmap](https://trello.com/b/7fEwaPRd/roadmap). Today, we have some really exciting news for you.

### Service = Load Balancer + Self Healing

![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/5809d9232d9f8cb9ef140303/f7bfaceab369bc1350dc82590ff54359/service.png)

Service is an abstraction which defines a logical set of containers in a single (private) network and a policy by which to access them. As an example, consider an image-processing backend which is running with 3 replicas. Those replicas are fungible - frontends do not care which backend they use. While the actual containers that compose the backend set may change, the frontend clients should not need to be aware of that or keep track of the list of backends themselves. 

Service also keeps monitoring the replicas. If one or more containers existed (crashed), the service will automatically provision new replicas to replace the failed ones. By doing this, service helps to ensure the desired number of the group.

Service supports both `tcp`, `http` and `https` protocol. You can easily offload the entire SSL termination process to the service. Your containers can benefit from encrypted communication with very little operational overhead or administrative complexity.

More details can be found at 
- [Roadmap](https://trello.com/c/7qb0MCCL/23-service)
- [Documentation](https://docs.hyper.sh/Reference/container/service.html)

### Container Event API (Websocket)

In many cases, developers want to keep watching container's events. They want to know when a (new) container is launched, when a container exits (stops), etc. The container event API meets these requirements.  

The new API is implemented in websocket, which provides easy, realtime notification to the clients. Currently it supports `start` and `stop`, with more in the roadmap.

More details can be found at:
- [Roadmap](https://trello.com/c/QkavyD7R/33-container-event-api-websocket)
- [Documentation](https://docs.hyper.sh/Reference/API/2016-04-04%20[Ver.%201.23]/Event/ws.html)
- [Example (in Go)](https://github.com/hyperhq/websocket-client/blob/master/go/wsclient.go)

### Container Termination Protection
For the long-running containers, a little extra care doesn't hurt. The new protection flag helps you to prevent incidental termination:

	$ hyper run --protection=true busybox
	$ hyper update --protection=false mycontainer
 
 More details can be found at:
- [Roadmap](https://trello.com/c/HcidVhFz/62-termination-protection-for-container)
- [Documentation](https://docs.hyper.sh/Reference/CLI/run.html)

### Compose Enhancement
The enhancement adds supports for:

-  `compose pull`: pull the Docker images specified in the compose file
-  `noauto_volume`:  ignore `VOLUME` flags in the Docker images
-  `security_groups`: specify security groups to containers

More details can be found at:
- [Roadmap](https://trello.com/c/TcNvHXMH/57-support-security-group-and-noautovol-options-in-compose)
- [Documentation](https://docs.hyper.sh/Reference/compose_file_ref.html)

### CLI Enhancement
The enhancments allows Hyper CLI to load API credentials from environmental variables: `HYPER_ACCESS` and `HYPER_SECRET`:

 More details can be found at:
- [Roadmap](https://trello.com/c/78OaPMiC/49-make-the-cli-read-access-and-secret-of-envs)

---------------------

We hope that you will enjoy these new features in the new Hyper.sh. Please stay tuned on [our roadmap](https://trello.com/b/7fEwaPRd/roadmap) as we continue to add more exciting features and if you have any questions you can always contact us through the Hyper.sh console, Hyper.sh forum, or on support@hyper.sh.

Thanks,

The Hyper Team
