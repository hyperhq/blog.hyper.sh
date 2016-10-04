

# How is Hyper.sh different from Docker Cloud?

We have received many questions about how our Hyper.sh container-native cloud compares with Docker Cloud, so we thought it would be good to point out the major differences and benefits. We have made a list of five points on how they compare. Some of the things to note are about pricing, scaling, and backups, which are each very difficult things and every platform does it differently. If you read them and afterward still have some questions, feel free to email us! 

Let's get started!

### How much do you pay and how easy is that?
#### _Docker Cloud:_
You have to create a cluster of machines before you even can start a single container. If a machine disappears (can happen https://murze.be/2016/02/today-digitalocean-lost-our-entire-server/), you have to control that event yourself. After you have started a cluster at a third party provider, you have to figure out how much capacity you will constantly need, so that you don’t run out of; CPU, disk space, memory. After all these difficulties, you will also get a bill from both Docker Cloud and your third party provider.

#### _Hyper.sh:_
You can start a container just after you signed up. You don’t have to maintain any servers. You only pay for the seconds you actually use. You can scale effortlessly and you get one bill.

### How easy is it to start containers?
#### _Docker Cloud:_
It is pretty easy to start a container through the interface. Docker Cloud has some nice presets that you can deploy in minutes. However, the CLI is not easy! The CLI introduces among other things new keywords like “stack”, “-r RUN_COMMAND” and “--deployment-strategy” and it is not possible to load any content from your own computer either.

#### _Hyper.sh:_
You use the Hyper.sh CLI like you would use docker and docker-compose. By default containers are not accessible from the public, you, therefore, assign a public floating IP to each container. For even easier deploys you can load content from your own computer directly to the container.

### How easy is it to take backups or snapshot of volumes?
#### _Docker Cloud:_
There is no way to do backups or snapshots of volumes.

#### _Hyper.sh:_
It is super easy with the hyper_ CLI. You just write “hyper snapshot create” and then the volume name.

### How is networking between containers?
#### _Docker Cloud:_
When you are using Docker Cloud, networking is managed by the weave.works daemon (which you have to add to your calculation when counting memory usage) and you don’t really have any influence in how that is working or if you are experiencing degrade network performance you are left to yourself.

#### _Hyper.sh:_
All your containers are joined in a private Layer-2 virtual private network managed by Hyper.sh, which ensure performance on a consistent basis and are not left for any outside product to manage.

### How do you stay up to date with security patches and new docker versions?
#### _Docker Cloud:_
It is your responsibility, any security patches are something you have to update yourself. If any new version of Docker comes up, you either have to upgrade all your nodes at the same time, which means downtime, or do a difficult migration.

#### _Hyper.sh:_
You don’t have to maintain any servers, Hyper.sh is responsible for patching and security. When a new version of Docker comes, the infrastructure can update without downtime to your sites.

## Conclusion
Hyper.sh and Docker Cloud each try to make it as easy as possible, but we think that we have made something better that will benefit you, as you don’t have to worry about a lot of infrastructures, but still get that performance you need while being flexible enough to cover nearly all use cases!

#### Sign up at Hyper.sh now and try it yourself!


