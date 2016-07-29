title: "Hyper_ vs DigitalOcean Smackdown - Round 1: WordPress with MySQL and PHPMyAdmin"
date: 2016-07-29 10:00:00
author: hyper
preview: Hyper is all about making things easier. Easier to use, easier to maintain and easier to understand. We see a lot of people wasting a lot of time installing and patching Docker, maintaining clusters, figuring out whether to use Mesos or Kubernetes.

---

Managing hosted servers is a thing of the past.

----

Hyper is all about making things easier. Easier to use, easier to maintain and easier to understand. We see a lot of people wasting a lot of time installing and patching Docker, maintaining clusters, figuring out whether to use Mesos or Kubernetes.

With Hyper, none of this is necessary, you can simply concentrate on what you care about: getting your application running so you can start delivering value to your customers.

Now _of course_, some workloads require custom stacks. Stacks with hand picked tools and thousands of man hours of tweaking but many workloads simply do not.

In this blog post we’ll show you just how many lines of effort you can save vs Digital Ocean when installing one of the most common stacks there is: Wordpress with MySQL and PHPMyAdmin.

> TL;DR: DigitalOcean takes 60+ commands, Hyper takes 11 and removes the need for server maintenance

The DigitalOcean version actually requires 2 entire tutorials [here](https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-lamp-on-ubuntu-16-04) and [here](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-16-04).

The Hyper commands to achieve the same effect can be found below.

	//run mysql container
	​$ hyper run --name mysqldb -e MYSQL_ROOT_PASSWORD=12345678 -d mysql
	dedd5369d311ea2de163a04c71961104569e953d90b2424df3e3debaf1ca3d0e

	//run phpmyadmin container
	$ hyper run --name myadmin -d --link mysqldb:db -p 8080:80 phpmyadmin/phpmyadmin
	475e215b02fef05da0eca243b708c578e19372d5efcf7551c2c4a32350633e34

	//add public ip to phpmyadmin container
	$ hyper fip allocate 1
	23.236.114.79
	$ hyper fip associate 23.236.114.79 myadmin

	//view all containers
	$ hyper ps
	CONTAINER ID   IMAGE                     COMMAND                  	CREATED              STATUS           PORTS                  	NAMES       PUBLIC IP
	475e215b02fe   phpmyadmin/phpmyadmin     "/run.sh"                	About a minute ago   Up 55 seconds    0.0.0.0:8888->80/tcp  myadmin     23.236.114.79
	dedd5369d311   mysql                     "docker-entrypoint.sh"   3 minutes ago        Up 3 minutes                            mysqldb

	//Open myphpadmin Web UI
	open http://23.236.114.79:8888 in Web Browser, login with account root/12345678

	//create wordpress container
	$ hyper run --name mywordpress --link mysqldb:mysql -p 8080:80 -d wordpress
	fb8d18bcf109731dea7e492545e0602d11f31b6e88ad155f9f1fa1158dbe027c

	//add public ip to wordpress container
	$ hyper fip allocate 1
	162.221.195.188
	$ hyper fip associate 162.221.195.188 mywordpress

	//view all containers
	$ hyper ps
	CONTAINER ID   IMAGE                     COMMAND                  CREATED              STATUS           PORTS                  NAMES        PUBLIC IP
	68f31cd09926   phpmyadmin/phpmyadmin     "/run.sh"                About a minute ago   Up 55 seconds    0.0.0.0:8888->80/tcp   myadmin      23.236.114.79
	12481a148fdc   mysql                     "docker-entrypoint.sh"   3 minutes ago        Up 3 minutes                           mysqldb
	fb8d18bcf109   wordpress                 "/entrypoint.sh apach"   5 minutes ago        Up 5 minutes     0.0.0.0:8080->80/tcp   mywordpress  162.221.195.188

	//Open wordpress Web UI
	open http://162.221.195.188:8080 in Web Browser, Start wordpress install wizard.

Summary
----
By using Hyper to re-use Docker images that are already available online, you can be up and running with 20% of the effort and don’t forget, with Hyper you don’t need to manage a server or patch a an OS anymore. You also get cool stuff like volume based persistence for Wordpress upgrades for free. That’s so much better!

Sign up for Hyper now: [https://hyper.sh/](https://hyper.sh/)
Follow us on Twitter for all the latest information: [https://twitter.com/hyper_sh](https://twitter.com/hyper_sh)
