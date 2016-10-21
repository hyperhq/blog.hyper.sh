title: "Introducing Service"
date: 2016-10-21 16:44:00 +0800
author: hyper
tags:
    - Docker
    - Container
    - Container-native cloud
    - Hyper
    - Docker Hosting
preview: Service automatically handles the load balancing across your containers, and provisions new replicas to replace failed containers.

---
At Hyper.sh, our mission has always been to **make running containers in production effortless**. In the past few weeks, we've been collecting feature requests from our community to help prioritize [our roadmap](https://trello.com/b/7fEwaPRd/roadmap). Today, we can announce some exciting new features.

### Hyper Service = Load Balancer + Self Healing

![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/5809d9232d9f8cb9ef140303/9a1617d9613314f654def7cad0eddc55/service_small.png)

Hyper Service is an abstraction which defines a logical set of containers in a single (private) network and a policy by which to access them. As an example, consider an image-processing backend which is running with 3 instances. Those instances are fungible - the frontend does not care which backend it uses. While the actual containers that compose the backend set may change, the frontend clients should not need to be aware of that or keep track of the list of backends themselves. 

Hyper Service also monitors the replicas. If a container exits  (crashes), the service will automatically provision new a new instance to replace the failed one. By doing this, Hyper Service helps to ensure the that the desired number of instances are present at all times.

Hyper Service supports both `tcp`, `http` and `https` protocols, allowing you to easily offload the entire SSL termination process. Your containers can benefit from encrypted communication with very little operational overhead or administrative complexity.

More details can be found at 
- [Roadmap](https://trello.com/c/7qb0MCCL/23-service)
- [Documentation](https://docs.hyper.sh/Reference/container/service.html)

### Container Event API (Websocket)

In many cases, developers want to track container events. They want to know when a (new) container is launched, when a container exits (stops), etc. The container event API provides this functionality.  

The new API is implemented using websockets, which provides easy, realtime notifications to the clients. `start` and `stop` events are currently supprted, with more in the roadmap to follow.

More details can be found at:
- [Roadmap](https://trello.com/c/QkavyD7R/33-container-event-api-websocket)
- [Documentation](https://docs.hyper.sh/Reference/API/2016-04-04%20[Ver.%201.23]/Event/ws.html)
- [Example (in Go)](https://github.com/hyperhq/websocket-client/blob/master/go/wsclient.go)

### Container Termination Protection

For long-running containers, a little extra care doesn't hurt. The new protection flag helps you to prevent accidental termination:

	$ hyper run --protection=true busybox
	$ hyper update --protection=false mycontainer
 
 More details can be found at:
- [Roadmap](https://trello.com/c/HcidVhFz/62-termination-protection-for-container)
- [Documentation](https://docs.hyper.sh/Reference/CLI/run.html)

### Enhancements to Hyper Compose

This release supports the following extra commands:

-  `compose pull`: pull the Docker images specified in the compose file
-  `noauto_volume`:  ignore `VOLUME` flags in the Docker images
-  `security_groups`: specify security groups to containers

More details can be found at:
- [Roadmap](https://trello.com/c/TcNvHXMH/57-support-security-group-and-noautovol-options-in-compose)
- [Documentation](https://docs.hyper.sh/Reference/compose_file_ref.html)

### CLI Enhancements

This release allows the Hyper CLI to load API credentials from environmental variables: `HYPER_ACCESS` and `HYPER_SECRET`:

 More details can be found at:
- [Roadmap](https://trello.com/c/78OaPMiC/49-make-the-cli-read-access-and-secret-of-envs)

---------------------

We hope that you will enjoy these new features in the new Hyper.sh. Please stay tuned on [our roadmap](https://trello.com/b/7fEwaPRd/roadmap) as we continue to add more exciting features and if you have any questions you can always contact us through the Hyper.sh console, Hyper.sh forum, or on [support@hyper.sh.](mailto:support@hyper.sh)

Thanks,

The Hyper Team
