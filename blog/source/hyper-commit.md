title: "Hyper commit/push, Hyper Func Sync mode, and more!"
date: 2017-05-16 21:40:00 +0800
author: hyper
tags:
    - Container
    - Image
    - Hyper.sh
    - Commit
preview: Here comes our latest upgrade on Hyper.sh!

---
Here comes our latest upgrade on Hyper.sh!

### Hyper Commit / Push
In many cases, our users told us that they want to `commit` a container to create a new image (version) and `push` it back to the registry, so here it is!

```
$ hyper commit --change "ENV DEBUG true" 50d6ab76a13a  user/testimage:v1
sha256:8a0cb365e21b94328e0fe8727ff347051e8bc0292fa8c5d1450a9b1d272bbaa4
$ hyper inspect -f "{{ .Config.Env }}" 8a0cb365e21b
[DEBUG=true PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin]
$ hyper images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
user/testimage      v1                  8a0cb365e21b        30 seconds ago      1.126 MB
$ hyper push user/testimage:v1
```
Please check out the documentation here for more information: 
- [hyper commit](https://docs.hyper.sh/Reference/CLI/commit.html)
- [hyper push](https://docs.hyper.sh/Reference/CLI/push.html)

If you have any feedback, no matter how small, we'd love to hear from you!

### Hyper Func Sync Call
We also make an enhancement [***Hyper Func***](https://docs.hyper.sh/Feature/container/func.html) by adding the `--sync` flag to block the API until the function call completes (or failed).

```
$ hyper func create --name helloworld ubuntu echo HelloWorld
helloworld is created with the address of https://us-west-1.hyperfunc.io/call/helloworld/e62c014e-386c-42ea-8d07-41d44e98cc3d
$ hyper func call --sync helloworld
Hello World
```

Please check out the documentation here for more information: 
- [hyper func call](https://docs.hyper.sh/Reference/CLI/Func/call.html)


### API Credential Name
Now, you can name your API crendentials in the web console.

![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/591a8497b174282efb28b815/409ce1b5d8ab7e90620cb73882408d9e/upload_5_16_2017_at_12_48_20_PM.png)

----------------------------------------------
That's it for now, and as always, happy hacking!
The Hyper.sh Crew

[Twitter](https://twitter.com/hyper_sh), [Slack](https://slack.hyper.sh/), [Forum](https://forum.hyper.sh/), [Mail](mailto:talk@hyper.sh)
