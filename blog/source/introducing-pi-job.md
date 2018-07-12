title: Introduce Pi Job and Large Pod Size
date: 2018-07-04 16:48:12
author: Peng Zhao
preview: Today, we are excited to announce to support Kuberentes Job feature in Pi. Also, we add more large pod size for bigger workload requirements.
---

# Job - Run to Completion

Today we are happy to announce the release of [Kubernetes Job](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) support in [Pi](https://hyper.sh/pi). 

As you would expect, a job creates one or more pods and ensures that a specified number of them successfully terminate. As pods successfully complete, the job tracks the successful completions. When a specified number of successful completions is reached, the job itself is complete. 

With Pi Job however, you can run any command in a pod without the need of Kubernetes cluster. Also you only pay (per second!) when the job is running, just as with any other resource on __Pi__.

### Create a job which computes π to 2000 places and prints it out:

```
$ pi create job pi --image=perl --restart=Never --backoff-limit=4 -- perl -Mbignum=bpi -wle "print bpi(2000)"
job "pi" created
```

### Check on the status of the job using this command:

```shell
$ pi describe jobs/pi
Name:             pi
Namespace:        default
Selector:         controller-uid=b1db589a-2c8d-11e6-b324-0209dc45a495
Labels:           controller-uid=b1db589a-2c8d-11e6-b324-0209dc45a495
                  job-name=pi
Annotations:      <none>
Parallelism:      1
Completions:      1
Start Time:       Tue, 03 Jul 2018 15:02:53 +0800
Pods Statuses:    0 Running / 1 Succeeded / 0 Failed
Pod Template:
  Labels:       controller-uid=b1db589a-2c8d-11e6-b324-0209dc45a495
                job-name=pi
  Containers:
   pi:
    Image:      perl
    Port:
    Command:
      perl
      -Mbignum=bpi
      -wle
      print bpi(2000)
    Environment:        <none>
    Mounts:             <none>
  Volumes:              <none>
Events:
  FirstSeen    LastSeen    Count    From            SubobjectPath    Type        Reason            Message
  ---------    --------    -----    ----            -------------    --------    ------            -------
  1m           1m          1        {job-controller }                Normal      SuccessfulCreate  Created pod: pi-dtn4q
```

To view completed pods of a job, use `pi get pods`.

### List all the pods that belong to a job in a machine readable form:

```shell
$ pods=$(pi get pods --selector=job-name=pi --output=jsonpath={.items..metadata.name})
$ echo $pods
pi-aiw0a
```

Here, the selector is the same as the selector for the job. The `--output=jsonpath` option specifies an expression that just gets the name from each pod in the returned list.

### View the standard output of one of the pods:

```shell
$ pi logs $pods
3.1415926535897932384626433832795028841971693993751058209749445923078164062862089986280348253421170679821480865132823066470938446095505822317253594081284811174502841027019385211055596446229489549303819644288109756659334461284756482337867831652712019091456485669234603486104543266482133936072602491412737245870066063155881748815209209628292540917153643678925903600113305305488204665213841469519415116094330572703657595919530921861173819326117931051185480744623799627495673518857527248912279381830119491298336733624406566430860213949463952247371907021798609437027705392171762931767523846748184676694051320005681271452635608277857713427577896091736371787214684409012249534301465495853710507922796892589235420199561121290219608640344181598136297747713099605187072113499999983729780499510597317328160963185950244594553469083026425223082533446850352619311881710100031378387528865875332083814206171776691473035982534904287554687311595628638823537875937519577818577805321712268066130019278766111959092164201989380952572010654858632788659361533818279682303019520353018529689957736225994138912497217752834791315155748572424541506959508295331168617278558890750983817546374649393192550604009277016711390098488240128583616035637076601047101819429555961989467678374494482553797747268471040475346462080466842590694912933136770289891521047521620569660240580381501935112533824300355876402474964732639141992726042699227967823547816360093417216412199245863150302861829745557067498385054945885869269956909272107975093029553211653449872027559602364806654991198818347977535663698074265425278625518184175746728909777727938000816470600161452491921732172147723501414419735685481613611573525521334757418494684385233239073941433345477624168625189835694855620992192221842725502542568876717904946016534668049886272327917860857843838279679766814541009538837863609506800642251252051173929848960841284886269456042419652850222106611863067442786220391949450471237137869609563643719172874677646575739624138908658326459958133904780275901
```

# More Large Pod Size

There has been a lot of interest from our customers to use large pod size to both CPU and memory intenive workload. Now, we add the support of the following new pod sizes:

| Size  | vCPU  | RAM (GB) | 
|---|---|---|
| L3  |  8  |  30  |
| L4  | 16  |  50  |
| L5  | 32  | 110  |
| L6  | 64  | 230  |

If you need even larger spec,  please feel free to [let us know](contact@hyper.sh)!

Want to try _Pi_ now? Enjoy your access here: https://hyper.sh
