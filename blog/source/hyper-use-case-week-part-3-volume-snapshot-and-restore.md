title: "Hyper Use Case Week - Part 3: Volume snapshot and restore"
date: 2016-05-11 22:00:00 +0800
author: hyper
preview: This is part of a 4 part series about the use cases that we've seen people using on Hyper since we opened our public beta on April 4th 2016.

In [part 2](https://blog.hyper.sh/hyper-use-case-week-part-2-load-balancer-with-two-web-servers.html) we looked at load balancing across two web servers. In this part we will look at another common use case: taking a snapshot of a volume and restoring a process against that snapshot.

---

This is part of a 4 part series about the use cases that we've noticed people using on Hyper since we opened our [public beta](https://console.hyper.sh/register) on April 4th 2016.

In [part 2](https://blog.hyper.sh/hyper-use-case-week-part-2-load-balancer-with-two-web-servers.html) we looked load balancing across two web servers In this part we will look at another classic use case: taking a snapshot of a volume and restoring a process against that snapshot.

As we said in the first post these _"...use cases are not groundbreaking, but what we love to see is the ease and speed with which people are getting up and running."_

So let's get started!

### Use Case 3 - Volume snapshot and restore

So you'd like to presist data between redeploys of your service, something like this:

![Volume snapshot and restore](images/hyper-use-case-week-part-3-volume-snapshot-and-restore/1.png)

#### How do I do it?

Users on the [Hyper public beta](https://console.hyper.sh/register) are achieving this in just a few lines. Here's how:

``` shell
hyper volume create --name vol1
hyper run -it --name db-1 -v vol1:/tmp hyperhq/postgres /bin/bash
echo hello >>  /tmp/test.txt
hyper rm -f db-1
hyper snapshot create --name mybackup --volume vol1
hyper volume create --name vol2 --snapshot mybackup
hyper run -it --name db-2 -v vol2:/tmp hyperhq/postgres cat /tmp/test.txt
```

Obviously this is a Postgres example, but if your application runs in a container, it will run on Hyper_.

#### What's next?

**Next** we'll be looking at how to failover using a floating IP. Questions? Drop us a mail at [contact@hyper.sh](mailto:contact@hyper.sh).

Why not get started now? [https://console.hyper.sh/register](https://console.hyper.sh/register)
