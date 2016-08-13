title: "Run Desktop Applications in Hyper_"
date: 2016-07-26 14:00:00 +0800
author: hyper
preview: Hyper_ cloud is designed to work well with many kinds of applications. That's why most people use it to deploy their applications in a cloud environment. This is great, and saves people huge headaches, but this article will show you a not-so-typical way.

---

In this article, we will show how to use Hyper_ to help run desktop applications on the cloud. The basic idea is that we will launch a Linux desktop version OS on Hyper_ cloud and then we can launch Chrome, Firefox etc in that container by using VNC.

> More about VNC: VNC is a great client/server tool to access your computers remotely. Please check [tigervnc](http://tigervnc.org/) for example if you are not familiar with it.

Wait, why would we even want to run all these applications in Hyper_?
The most common case is that users may not want to install all the things on their PCs.
They may also want the ability to delete an application and know it is gone without some random file hanging around.
Or, they may just want a machine lives in the cloud!

Obviously, running those applications in Hyper_ cloud will help a lot. What's more, now users can control how much CPU and Memory the application uses, and visit this application anywhere as long as they have Internet.

Let's see how to do!


### Run desktop CentOS in Hyper_ cloud

The first use case will show how to run a desktop version CentOS 6 container in Hyper_.

#### Launch a CentOS 6 container in cloud

Let's create and start the container:
```shell
$ containerId=`./hyper run -d centos:6`
```

#### Setup desktop
Then let's exec into this container:
```shell
$ hyper exec -it ${containerId} /bin/bash
```

Now we have a CentOS 6 server in the cloud, let's upgrade it into desktop version:
```shell
$ yum groupinstall "Desktop" -y
```

#### Setup VNC
Note that if you want to visit this desktop applications running in cloud from local, you need remote access tools like VNC:
```shell
$ yum install tigervnc tigervnc-server vim -y
```
And edit `/etc/sysconfig/vncservers` to make it work：
```
VNCSERVERS="2:root"
VNCSERVERARGS[2]="-geometry 1024x768  -nolisten tcp -localhost"
```
Start the VNC server:
```shell
$ vncserver
```
This command will ask you for a remote access password, feel free to choose your favorite one here, let's say `PASSWORD`.

Exit your Hyper_ container, a desktop CentOS 6 is ready in the cloud.


#### Use `fip` to connect your desktop container

The floating IP or `fip` feature in Hyper_ cloud makes it so easy to connect to your cloud application from anywhere:
```shell
$ fip=`hyper fip allocate 1`
$ hyper fip attach ${fip} ${containerId}
```

Done! Now you can use VNC client to connect your CentOS container at  `fip:1`.

### Try to experience more

The most exciting part of "container revolution" is finally we can ship applications to cloud in an elegant way. Thanks to container images! In Hyper_, you can experience more desktop applications by deploying them in one shell, and connect them using `fip` from VPN client.

For example, `kaixhin/vnc` is a image which provide you with `Ubuntu Core 14.04 + LXDE desktop + Firefox browser + TightVNC server`. You can deploy it to Hyper_ easily:
```shell
$ hyper run -d  kaixhin/vnc
$ hyper fip attach 162.221.195.27  0649e517b002
```
Then you can use this Ubuntu workstation on Hyper_ cloud by connecting to `162.221.195.27:1` with password "password", and browser Internet with FireFox.

Another similar example is for LXDE desktop users:
```shell
$ hyper run -d  dorowu/ubuntu-desktop-lxde-vnc
$ hyper fip attach 162.221.195.28 c8a584dcbb9b
```
Visit your Ubuntu through `http://162.221.195.28:6080/vnc.html`, and use `ubuntu` as password.

Have fun!
