title: "Learn Hyper_ By Examples: Run Your application"
date: 2016-05-10 22:00:00 +0800
author: hyper
tags:
    - Container
    - Hyper
    - Network
preview: This article shows how to use Hyper_ to deploy a real-world application and play with it.

---

# Run Your application

In the previous ["*Run a Hello world*"](https://blog.hyper.sh/learn-hyper_-by-examples-hello-world-in-a-container.html) the first container in Hyper_ has been launched using the `hyper run` command. In this article, let's go further.

## More about the hyper command line tool

The `hyper` client is a simple command line also known as a command-line interface (CLI).  One helpful command is `hyper version` to return version and commit ID of the currently installed hyper client as well as the server information in the cloud side.
```shell
$ hyper version
Client:
 Version:      1.10.0-beta
 API version:  1.23
 Go version:   go1.6
 Git commit:   a549613
 Built:        Tue Apr 26 09:22:33 UTC 2016
 OS/Arch:      darwin/amd64

Server:
 Version:      library-import
 API version:  1.23
 Go version:   go1.6
 Git commit:   library-import
 Built:        library-import
 OS/Arch:      linux/amd64
```
## Get `hyper` command help

You can display the help for specific `hyper` commands. It returns details the options and their usage. To see a list of all the possible commands, use the following:
```shell
$ hyper --help
```
To see usage for a specific command, specify the command with the `--help` flag:
```shell
$ hyper logs --help

Usage:  hyper logs [OPTIONS] CONTAINER

Fetch the logs of a container

  -f, --follow        Follow log output
  --help              Print usage
  --since             Show logs since timestamp
  -t, --timestamps    Show timestamps
  --tail=all          Number of lines to show from the end of the logs
```

> **Note:**
> For full details and examples of each command, please checkout the
> [hyper command reference](https://docs.hyper.sh/Reference/CLI/index.html) on the official [hyper.sh](https://hyper.sh) site.

## Running a web application in Hyper_

So now you've learned a bit more about the `hyper` client you can move onto the important stuff: running real-world applications. We
start this by running an example web application in Hyper_.

This web application is a popular Python Flask application image from docker hub.

Again, we start with a `hyper run` command.
```shell
$ hyper run -d training/webapp python app.py
```
You've already seen the `-d` flag which tells Hyper_ to run the
container in the background.

You've specified an image: `training/webapp`. This image is a
pre-built image you've created that contains a simple Python Flask web
application.

Lastly, you've specified a command for your container to run: `python app.py`. This launches your web application.

> **Note:**
> You can see more detail on the `hyper run` command in the [hyper run command reference](https://docs.hyper.sh/Reference/CLI/run.html).

## Viewing your web application

Now you can see your running containers using the `hyper ps` command.
```
$ hyper ps -l
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES               PUBLIC IP
d54150919603        training/webapp     "python app.py"     4 minutes ago       Up 4 minutes        0.0.0.0:5000->5000/tcp   berserk_yalow       

```

You may notice you've specified a new flag, `-l`, for the `hyper ps`
command. This tells the `hyper ps` command to return the details of the *last* container started.

> **Note:**
> By default, the `hyper ps` command only shows information about running containers. If you want to see stopped containers, use the `-a` flag.

We can see the same details as the  ["*Run a Hello world*"](https://blog.hyper.sh/learn-hyper_-by-examples-hello-world-in-a-container.html) but with one important addition in the `PORTS` column.

    PORTS
    0.0.0.0:5000->5000/tcp

This port is exposed in [its Dockerfile](https://hub.docker.com/r/training/webapp/~/dockerfile/), and it is the the default port that Flask serves on.
```
...
WORKDIR /opt/webapp
EXPOSE 5000
CMD ["python", "app.py"]
```

> **Note:**
> Since Docker image is the only image specification Hyper_ support for now, you can learn more about how to expose ports in Docker images in [its official doc](https://docs.docker.com/engine/userguide/containers/dockerimages/).

Note that in Hyper_ , **there's no need to do port mapping**, because hypervisor based containers uses SDN to ensure network connectivity in the same tenant (and isolation between different tenants). So any ports exposed in your image can be reached directly, for example if we know the IP address of this container:
```
 $ hyper inspect --format '{{ .NetworkSettings.IPAddress }}' berserk_yalow
172.16.0.132
```
Then this web application can be reached by another container through `172.16.0.132:5000`.  

And even if your containerised application works on ports that are not exposed in the Dockerfile, they can also be reached directly like in our example. But we recommend explicitly specifying the ports, because it allows for automation, we'll show this in following articles.

## Reach your application from outside world

The previous section described that containers in Hyper_ can be reached through IP, but after all, those are still private IP addresses. How can we visit our application from outside world?

It's quite simple: using a floating IP.

Floating IP (or fip) is a real **public IP** allocated by SDN component in Hyper_ and can be associated with specific container. That's a standard workflow in many mature cloud platforms like AWS, GCE or OpenStack. We allow you to do this in a container cloud.

```
$ hyper fip allocate 1
162.221.195.205
```
The floating IP management relies on `hyper fip` command.
```
$ hyper fip allocate --help

Usage:  hyper fip allocate [OPTIONS] COUNT

Creates some new floating IPs by the user
```
* `COUNT` is the number of floating IPs you want to create. In this example, we created only one.

Then we can associate this IP to your application.
```
$ hyper fip associate 162.221.195.205 berserk_yalow
```

Try to visit it from laptop:


![](-/images/learn-hyper_-by-examples-run-your-application/1.png)


Our Python application is live!




## Check the web application's logs

You can also find out a bit more about what's happening with your application and use another of the commands you've learned, `hyper logs`.

```shell
$ hyper logs -f berserk_yalow
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
115.199.176.47 - - [04/May/2016 13:22:47] "GET / HTTP/1.1" 200 -
115.199.176.47 - - [04/May/2016 13:37:17] "GET / HTTP/1.1" 200 -
115.199.176.47 - - [04/May/2016 13:37:18] "GET /favicon.ico HTTP/1.1" 404 -
```

This time though you've added a new flag, `-f`. This causes the `hyper
logs` command to act like the `tail -f` command and watch the
container's standard out. We can see here the logs from Flask showing
the application running on port `5000` and the access log entries.


## Inspecting your web application container

You may have noticed in previous section, we got the IP address of container using `hyper inspect` command.

This command is a low-level request into your Hyper_ container and returns a JSON document containing useful configuration and status information for the specified container.
```shell
$ hyper inspect nostalgic_morse
```
You can see a sample of that JSON output.

```
[
    {
        "Id": "d541509196038664cea908425782ac6c834980bf3c361115b423c51274fa1409",
        "Created": "2016-05-04T08:35:28.786209278Z",
        "Path": "python",
        "Args": [
            "app.py"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 0,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2016-05-04T08:35:34.564Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:6fae60ef344644649a39240b94d73b8ba9c67f898ede85cf8e947a887b3e6557",
    . . .
```

You can also narrow down the information you want to return by requesting a specific element, for example to return the container's IP address, use:


```
$ hyper inspect --format '{{ .NetworkSettings.IPAddress }}' berserk_yalow
172.16.0.132
```

## Stopping your web application container

OK, you've seen the web application working. Now you can stop it, by using the `hyper stop` command to free some resource.

```shell
$ hyper stop berserk_yalow
berserk_yalow
```
We can now use the `hyper ps` command to ensure the container has stopped.
```
$ hyper ps -l
```

## Restarting your web application container

Oops! Just after you stopped the container the manager said that he needs the container back. From here you have two choices: you can create a new container or restart the old one. Now let's see how to restart your previous container.

```
$ hyper start berserk_yalow
berserk_yalow
```
Now quickly run `hyper ps -l` again to see if the container is
running again or browse to the container's URL to see if the application
responds.

> **Note:**
> Also available is the `hyper restart` command that runs a stop and then start on the container.

## Removing your web application container

Finally, if you know your web application will be end of life and won't be needed anymore. You can easily remove it using the `hyper rm` command.

```shell
$ hyper rm berserk_yalow
Error response from daemon: Conflict, You cannot remove a running container. Stop the container before attempting removal or use -f
```
What happened? We can't actually remove a running container. This protects you from accidentally removing a running container you might need. As the error message implies, you can remove the container by stopping it first.

```shell
$ hyper stop berserk_yalow
berserk_yalow
$ hyper rm berserk_yalow
berserk_yalow
```
And now your container is stopped and deleted.

Otherwise, you should use `hyper rm -f` to delete the container forcefully.

> **Note:**
> Always remember that removing a container is final! So don't do `hyper rm -f` unless you know what you are doing!


# Next steps

Congratulations, you have run through a full application life-cycle workflow! In next article, we'll dive into another exciting part of containers on Hyper_ cloud: Networking!
