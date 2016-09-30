title: "Hyper Use Case Week - Part 4: Floating IP Failover"
date: 2016-05-12 18:00:00 +0800
author: hyper
preview: This is the last part of a 4 part series about the use cases that we've seen people using on Hyper since we opened our public beta on April 4th 2016.

In [part 3](https://blog.hyper.sh/hyper-use-case-week-part-3-volume-snapshot-and-restore.html) we looked at persisting state between redeploys by using a volume snapshot. In this part we will look at another common use case: failing over to a second instance using a floating IP.

---

This the last part of a 4 part series about the use cases that we've seen people using on Hyper since we opened our public beta on April 4th 2016.

In [part 3](https://blog.hyper.sh/hyper-use-case-week-part-3-volume-snapshot-and-restore.html) we looked at persisting state between redeploys by using a volume snapshot. In this part we will look at another common use case: failing over to a second instance using a floating IP.

As we said in the first post these _"...use cases are not groundbreaking, but what we love to see is the ease and speed with which people are getting up and running."_

So let's get started!

### Use Case 4 - Floating IP failover

So you'd like to gracefully fail over to another backend when something goes wrong? No problem.

![Attach a floating IP with one web server and failover to a second](images/hyper-use-case-week-part-4-floating-IP-failover/1.png)

#### How do I do it?

Users on the [Hyper public beta](https://console.hyper.sh/register) are achieving this in just a few lines. Here's how:

``` shell
hyper run -d --name web-1 hyperhq/webapp:host python app.py
FIP=$(hyper fip allocate 1)
hyper fip attach $FIP web-1
curl $FIP:5000
> Hello my host name is: 51924e0494f3
hyper rm -f web-1
hyper run -d --name web-2 hyperhq/webapp:host python app.py
hyper fip attach $FIP web-2
curl $FIP:5000
> Hello my host name is: 8568c6bbf2f793
```

Obviously this is a Python example, but if your application runs in a container, it will run on Hyper.sh.

#### What's next?

**That's it** for this series. Coming up next, running your own Cassandra cluster on Hyper. Keep an eye on our [twitter](https://twitter.com/hyper_sh).

Why not get started with Hyper.sh now? [https://console.hyper.sh/register](https://console.hyper.sh/register)

Questions? Drop us a mail at [contact@hyper.sh](mailto:contact@hyper.sh).
