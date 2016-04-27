title: How to run stateful container in Hyper_ container cloud?
date: 2016-04-27 00:00:00
author: hyper
preview: Hyper_ is a secure container cloud service. It allows you to deploy your containers in seconds, directly without hosting VMs to an infinite cloud, straight from your laptop.

---

[Hyper_](https://www.hyper.sh/) is a secure container cloud service. It allows you to deploy your containers in seconds, directly without hosting VMs to an infinite cloud, straight from your laptop.


Hyper_ also provides persistent volume service, which can been seen as "**The container-version of AWS EBS**". You can:

- hyper run -v to launch a container with additional volumes
- failover the volumes to a new container
- create snapshots from volumes for data backup
- restore snapshots to new volume

The following is an example to use Hyper_'s volume feature with a Rocket.chat app.

### Pull the images

``` shell
[root@localhost ~]# hyper pull rocket.chat
Using default tag: latest
latest: Pulling from library/rocket.chat
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
d1784d73276e: Pull complete
72e581645fc3: Pull complete
9709ddcc4d24: Pull complete
9df198b56120: Pull complete
ebd80fcd8c3b: Pull complete
26d5bf0f5698: Pull complete
c0f7e3e2e1e0: Pull complete
cab71a106a31: Pull complete
Digest: sha256:2b6774272c42d3789a795ef1711c81a9b320240d2fce9ca6e25d6ee3d3bdeecd
Status: Downloaded newer image for rocket.chat:latest
[root@localhost ~]#
[root@localhost ~]# hyper pull mongo
Using default tag: latest
latest: Pulling from library/mongo
8dddc0afbe0a: Pull complete
a3ed95caeb02: Pull complete
32eed1053be0: Pull complete
da7450003e70: Pull complete
da146c968d58: Pull complete
8d84cf004260: Pull complete
ab4010c6c1dc: Pull complete
cacc51e0bab5: Pull complete
5c9bb8d802e9: Pull complete
03913f2c5b05: Pull complete
Digest: sha256:be6bbba5cf0f206632fb0e782def85214d45b896f700be370cbdac421f53441a
Status: Downloaded newer image for mongo:latest
```

### Create a volume

``` shell
[root@localhost ~]# hyper volume create --name dbvol --size 50
dbvol
[root@localhost ~]# hyper volume ls
DRIVER              VOLUME NAME
hyper               dbvol
```

### Launch the mongo container, with the volume

``` shell
[root@localhost ~]# hyper run -d --name db -v dbvol:/data/db mongo
a03bddd9770e6964e69ffb0230648048e12a2a6424cce80738678c075450417d
[root@localhost ~]# hyper ps -a
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                      NAMES               PUBLIC IP
a03bddd9770e        mongo                     "/entrypoint.sh mongo"   25 seconds ago      Up 5 seconds        0.0.0.0:27017->27017/tcp   db
```

### Launch Rocket.chat, linking with mongo

``` shell
[root@localhost ~]# hyper run -d --name app --link db rocket.chat
38501806937e665ee79a5353851232191587d46149a67e4420d799e1927f2361
[root@localhost ~]# hyper ps -a
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                      NAMES               PUBLIC IP
38501806937e        rocket.chat               "node main.js"           18 seconds ago      Up 9 seconds        0.0.0.0:3000->3000/tcp     app
a03bddd9770e        mongo                     "/entrypoint.sh mongo"   Up 2 minutes      Up 5 seconds        0.0.0.0:27017->27017/tcp   db
```

### Associate Floating IP to Rocket.chat container

``` shell
[root@localhost ~]# hyper fip allocate 1
162.221.195.91
[root@localhost ~]# hyper fip associate 162.221.195.91 app
[root@localhost ~]#
```

### Generate some data

- Login Rocket.chat: [http://162.221.195.91:3000](http://162.221.195.91:3000)
- Register an account
- Say something

![](-/images/how-to-run-stateful-container-in-hyper-container-cloud/1.png)

### Remove the mongo container

``` shell
[root@localhost ~]# hyper rm -f db
db
[root@localhost ~]# hyper ps -a
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                    NAMES               PUBLIC IP
38501806937e        rocket.chat               "node main.js"           14 minutes ago      Up 9 minutes        0.0.0.0:3000->3000/tcp   app                 162.221.195.91
```

The volume is still there:

``` shell
[root@localhost ~]# hyper volume ls
DRIVER              VOLUME NAME
hyper               dbvol
```

### Launch a new mongo container, with the same db and the volume

``` shell
[root@localhost ~]# hyper run -d --name db -v dbvol:/data/db/ mongo
d0ae729247d7822468c79a997295e933cf8a17fd34300274cd70668cbcd5fb6a
[root@localhost ~]# hyper ps -a
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                      NAMES               PUBLIC IP
d0ae729247d7        mongo                     "/entrypoint.sh mongo"   29 seconds ago      Up 8 seconds        0.0.0.0:27017->27017/tcp   db
38501806937e        rocket.chat               "node main.js"           22 minutes ago      Up 16 minutes       0.0.0.0:3000->3000/tcp     app                 162.221.195.91
```

### Restart Rocket.chat container

``` shell
[root@localhost ~]# hyper restart app
app
[root@localhost ~]# hyper ps -a
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                      NAMES               PUBLIC IP
9602302e61c7        mongo                     "/entrypoint.sh mongo"   5 minutes ago       Up 5 minutes        0.0.0.0:27017->27017/tcp   db
38501806937e        rocket.chat               "node main.js"           34 minutes ago      Up 2 minutes        0.0.0.0:3000->3000/tcp     app                 162.221.195.91
```

### Refresh the browser

![](-/images/how-to-run-stateful-container-in-hyper-container-cloud/2.png)

### Hurrah~~~
