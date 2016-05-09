title: Learn Docker By Examples: A Hello World Container in Hyper_
date: 2016-05-09 00:00:00
author: harryz
preview: This guide takes you through the fundamentals of using Hyper_ to containerize user applications and explain some technical facts behind them.

---

[Hyper_](https://www.hyper.sh/) is a secure container cloud service. It allows you to deploy your containers in seconds from your laptop to the cloud.

Running an application inside a container takes a single command: `hyper run`.

> **Prerequisites:** 
> Please make sure you complete the following prerequisites:
> - [Open a Hyper_ account](https://console.hyper.sh/register)
> - [Generate API credential](https://docs.hyper.sh/GettingStarted/generate_api_credential.html)
> - [Setup `hyper` CLI on your local computer](https://docs.hyper.sh/GettingStarted/install.html)

### Run a Hello world container
Let's start with a hello world container.

``` shell
[root@localhost ~]$ hyper run ubuntu /bin/echo 'Hello world'
Hello world
```

Great, you just launched your first container on the cloud! 

> **NOTE:**
> This `hello world` container runs on the [Hyper_](https://hyper.sh) cloud!  So don't expect to find the container or image on your local machine. 

In this example:

* `hyper run` launches your container.

* `ubuntu` is the name of the Docker image to launch, the command will check if this image is already pulled into your account at Hyper_. If not, this image will be automatically fetched from [Docker hub](https://hub.docker.com/).
* `/bin/echo` is executed in the container, and the output is redirected to your local terminal:

```
Hello world
```

> **NOTE:**
> Containers run as long as the command you specify is active. Once the command finished (either successfully or failed), the container will enter  `Exited (0)` status.

### Run an interactive container

Next, let’s see how to run a container and interact with it.

``` shell
[root@localhost ~]$ hyper run -t -i ubuntu /bin/bash
root@a307b0881f49:/# 
```

In this example, we added some new flags.

* `-t` flag assigns a pseudo-tty or terminal inside the new container.
* `-i` flag allows you to make an interactive connection by grabbing the standard in (STDIN) of the container.
* `/bin/bash` launches a Bash shell inside our container.

Let’s try some commands inside the container:
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

> **NOTE:**
> In Hyper_  your containers are fully isolated and do not share kernel with other containers. So > `top` and `/procs` are all expected to be accurate.

To quit, type `exit` or press `Ctrl-D` to exit the interactive shell. 

### Start a daemonized container

Let’s create a container that runs in background.

```shell
[root@localhost ~]$ hyper run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
9c68cb7512266c41b23f4e0e92ac8a4d9425e1ee7d9df7530fcfd0ec05312053
```

The new flag we add here is:

* `-d` flag runs the container in the background (to daemonize it).

Finally, we specify a command to run:

```shell
/bin/sh -c "while true; do echo hello world; sleep 1; done"
```

In the output, we do not see hello world but a long string:

```shell
9c68cb7512266c41b23f4e0e92ac8a4d9425e1ee7d9df7530fcfd0ec05312053
```
This long string is called a container ID, which is also listed in Hyper_'s console. It uniquely identifies a container so we can work with it.

We can use this container ID to see what’s happening there. First, let’s make sure our container is running. Run the `hyper ps` command, which returns the information of active containers in your account.

```shell
[root@localhost ~]$ hyper ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                  PORTS                    NAMES               PUBLIC IP
9c68cb751226        ubuntu              "/bin/sh -c 'while tr"   12 minutes ago      Up 11 minutes                                    ecstatic_dubinsky   
```
In this example, we can see our  daemonized container. `hyper ps` returns some useful information:

* `9c68cb751226` is the shorter version of the container ID.
* `ubuntu` is the image of this container.
*  the command, created time, current status, exposed ports, name, public ip. We will explain network related feature in another post.

Is this container doing we asked it to do? We can use `hyper logs` command to check the output.

```shell
[root@localhost ~]$ hyper logs ecstatic_dubinsky
hello world
hello world
hello world
hello world
...
```

Great! The daemon is working and you have just created your first containerised server! 

#### Tips: Use the Hyper_ console

Besides `hyper` command line tool, we also provided a simple web console for you to get more information, especially when you need to get the overall status.

![](-/images/learn-hyper_-by-examples-hello-world-in-a-container/1.png)

* `Status` is the current status of this container, green for running, gray for stopped.
* `Image` is the image used by this container.
* `Name` is the name of this container, you can specify it by `hyper run --name`, otherwise a random name will be allocated.
* `ID`is the container ID we mentioned before.
* `Floating IP` is the public IP of this container, we'll talk this in following blogs.
* `Private IP` is the private IP address of this container. It is reachable within your private network, but isolated from other users and the Internet.
* `Command` is the command we used to start the container.
* `Created` is the time when this container is created.
* `Ports` is the ports claimed to be exposed in the Dockerfile (Docker image) of this container.


### Summary
So far, you launched your first container using the `hyper run` command. You ran an interactive container that ran in the foreground. You also ran a detached container that ran in the background. And then you use several hyper commands to see what happened.

* `hyper ps` - Lists containers.
* `hyper logs` - Shows us the standard output of a container.

In the next blog, we will show you how to use Hyper_ to deploy real world applications and work with these containers.
