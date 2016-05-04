title: "Learn Hyper_ By Examples: Hello World In A Container"
date: 2016-05-04 00:00:00 +0800
author: hyper
draft: true
preview: This guide takes you through the fundamentals of using Hyper_ to containerize user applications and explain some technical facts behind them.

---

[Hyper_](https://www.hyper.sh/) is a secure container cloud service. It allows you to deploy your containers in seconds from your laptop to the cloud.

Running an application inside a container takes a single command: `hyper run`.

> Note: Before reading rest of this article, you do not have to be familiar with other container technology like [Docker](https://docker.io). But you need to have installed latest hyper command line tool by following [this guide](https://docs.hyper.sh/GettingStarted/install.html) on your own system.

### Run a Hello world

Like any beginner, we start from running a hello world container.

``` shell
[root@localhost ~]$ hyper run ubuntu /bin/echo 'Hello world'
Hello world
```

Great, you just launched your first container on the cloud!

#### This is a Container in the Cloud
This `hello world` HyperContainer is running on the [Hyper_](https://hyper.sh) cloud!  So don't expect to find the container or it's image in your local machine. Hyper_ is designed to let you manage containers from your local environment, since we believe `container cloud should act like a big laptop`!

In this example:

* `hyper run` runs your container.

* `ubuntu` is the image you run (currently we only support Docker image), Hyper_ will check if this image is already pulled on the target host machine. If not, this image will be pulled from [Docker hub](https://hub.docker.com/) t.

Unlike other container cloud providers who use IaaS and VMs to wrap containers, Hyper_ directly creates a [hypervisor based container](directly) on physical servers in our datacenter which is secure enough to serve user's workload.

This container uses `ubuntu` image, and executes the `/bin/echo` command inside it and then prints out:

```
Hello world
```

Like Docker, user containers in Hyper_ only run as long as the command you specify is active, so that Ubuntu container will run into  `Exited (0)` status if you use `hyper ps` to see it.

### Run an interactive container

This time, let’s run in a container and interact with it.

``` shell
[root@localhost ~]$ hyper run -t -i ubuntu /bin/bash
root@a307b0881f49:/#
```

In this example, we added some new flags.

* `-t` flag assigns a pseudo-tty or terminal inside the new container.
* `-i` flag allows you to make an interactive connection by grabbing the standard in (STDIN) of the container.
* `/bin/bash` launches a Bash shell inside our container.

Let’s try running some commands inside the container:
```
root@a307b0881f49:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
# top
```

* `ls` displays the directory listing of the root directory of a typical Linux file system.

```
root@a307b0881f49:/# top - 03:12:41 up 0 min,  0 users,  load average: 0.00, 0.00, 0.00
Tasks:   3 total,   1 running,   2 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.7 sy,  0.0 ni, 99.3 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem:    507124 total,    21640 used,   485484 free,      980 buffers
KiB Swap:        0 total,        0 used,        0 free.     9232 cached Mem

  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND      
    1 root      20   0    4376    116      0 S  0.0  0.0   0:00.00 init         
    2 root      20   0   18172   3016   2716 S  0.0  0.6   0:00.35 bash         
   12 root      20   0   19852   2404   2088 R  0.0  0.5   0:00.00 top
```

* `top`displays the cpu memory usage and processes information.

> NOTE: In Hyper_  your contaiiners are fully isolated by virtualisation and do not share kernel with host. So `top` and `/procs` are all expected to work well.

Now, you can play around inside this container just like inside a cloud VPS. When completed, run the `exit` command or enter `Ctrl-D` to exit the interactive shell.

### Start a daemonized container

Let’s create a container that runs as a daemon.

```shell
[root@localhost ~]$ hyper run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
9c68cb7512266c41b23f4e0e92ac8a4d9425e1ee7d9df7530fcfd0ec05312053
```

The only new flag we add here is:

* `-d` flag runs the container in the background (to daemonize it).

Finally, we specify a command to run:

```shell
/bin/sh -c "while true; do echo hello world; sleep 1; done"
```

In the output, we do not see hello world but a long string:

```shell
9c68cb7512266c41b23f4e0e92ac8a4d9425e1ee7d9df7530fcfd0ec05312053
```
This long string is called a container ID which is also listed in the hyper.sh console. It uniquely identifies a container so we can work with it.

We can use this container ID to see what’s happening with our `hello world` daemon.

First, let’s make sure our container is running. Run the `hyper ps` command. The `hyper ps` command queries the Hyper_ for information about all the containers it knows about.

```shell
[root@localhost ~]$ hyper ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                  PORTS                    NAMES               PUBLIC IP
9c68cb751226        ubuntu              "/bin/sh -c 'while tr"   12 minutes ago      Up 11 minutes                                    ecstatic_dubinsky   
```
In this example, we can see our  daemonized container. `hyper ps` returns some useful information:

* `9c68cb75122`6 is the shorter version of the container ID.
* `ubuntu` is the image of this container.
*  the command,  created time, current status, exposed ports, name, public ip.

We will explain `PORTS` and `PUBLIC IP` later in other articles.

Is this container doing we asked it to do? We can use `hyper logs` command to look inside the container.

```shell
[root@localhost ~]$ hyper logs ecstatic_dubinsky
hello world
hello world
hello world
hello world
...
```

Great! The daemon is working and you have just created your first containerised application!

#### Tips: Use The Hyper_ Console

Besides `hyper` command line tool, we also provided a simple but powerful console for you to get more information, especially when you need to get the overall status.

![](-/images/learn-hyper_-by-examples-hello-world-in-a-container/1.png)

* `Status` is the current status of this container, green for running, gray for stopped.
* `Image` is the image used by this container.
* `Name` is the name of this container, you can specify it by `hyper run --name`, otherwise a random name will be allocated.
* `ID`is the container ID we mentioned before.
* `Floating IP` is the public IP of this container, we'll talk this in following blogs.
* `Private IP` is the IP address of this container, it is reachable by other containers belong to the same user.
* `Command` is the command we used to start the container.
* `Created` is the time when this container is created.
* `Ports` is the ports claimed to be exposed in the Dockerfile (Docker image) of this container.


### Summary
So far, you launched your first container using the `hyper run` command. You ran an interactive container that ran in the foreground. You also ran a detached container that ran in the background. And then you use several hyper commands to see what happened.
* `hyper ps` - Lists containers.
* `hyper logs` - Shows us the standard output of a container.

In the next blog, we will show you how to use Hyper_ to deploy real world applications and work with these containers.
