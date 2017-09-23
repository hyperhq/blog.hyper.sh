title: Performance test of running virtualized container on Google GCE
date: 2017-09-23 21:00:00 +0800
update: 2017-09-23 21:00:00 +0800
author: hyper
tags:
  - runV
  - Google GCE
  - Google Compute Engine
  - Google Cloud
  - Container
  - Docker
 
preview: Performance test of running virtualized container on Google GCE

---

Different from Linux Container, [runV](github.com/hyperhq/runv) is a virtualized container technology that uses hypervisor (KVM/QEMU, Xen) as the underlying runtime. It provides the hardware-enforced secure isolation to the existing container framework. Last week, we tagged 1.00 version of runV. The new release includes several major upgrades, including a Xen PV driver, which allows runV to be deployed on public IaaS, i.e. Google GCE.

Following the new release, we did some performance test this week. This post includes the result and preliminary analysis.

## Case 1: single container per instance

First, we launched a `g1-small` instance on GCE, and setup runV in the instance using Xen PV mode (which involves a Dom0 as the host), and launch a single virtual container to run the test.

#### Setup
- GCE instance:
	- Instance Size: g1-small
		- vCPU: 1
		- Memory: 1.7GB
		- Disk: 40GB SSD persistent disk
			- GCE limits: 1200 IOPS (READ, WRITE), 19.20MB/s throughput (READ, WRITE)
- runV:
	- Xen Dom0 Memory: 1GB
	- Container (DomU) vCPU: 1
	- Container Memory: 512MB

#### Result
Raw test results, fio config files and test scripts can be found at:
- [Single Container Disk IO](https://gist.github.com/bergwolf/a405f40f23d15f198bafd263bf0fe4dd)
- [Single Container CPU/Memory/Network](https://gist.github.com/bergwolf/49a248e019780168109d7861c3350a0d)

#### CPU
![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/59c48e61ab0ab52efb8cc781/5218358ef15e1016772e6de85ebcb66a/Single_Container_-_CPU.png)

||||||					avg|
|-|-|-|-|-|-|
|runV XenPV|	21s 221ms|	21s 205ms|	21s 151ms|	21s 100ms|	21s 169ms|
|native GCE|	20s 940ms|	20s 981ms|	21s 90ms|	20s 917ms|	20s 982ms|


#### Memory
![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/59c48e61ab0ab52efb8cc781/9f773b26fe5e0e1586cacf247b6f39ff/Single_Container_-_Memory.png)

|| runV XenPV|  GCE native|
|-|-|-|
|avg seq write (MB/s)| 7237| 7297|
|avg rand write (MB/s)| 3263| 3240|

#### Disk I/O
![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/59c48e61ab0ab52efb8cc781/fc483fcb78906ea266dd4f89d811a657/Single_Container_-_Disk_IO.png)

| IO type (KB/s) |runV XenPV (raw device)	|runV XenPV (ext4)|Instnace (raw device)	|Instance (ext4)|	
|----|----------|-------|---------------------|-----------------|
|128k randread|	24669.75|	24621.75|	24716.25	|24612.25|
|128k randwrite|	24675.75	|24621.5|	24700.5|	24616.75|
|4k randread|	6040.75|	6010.75|	6029.75|	6011.75|
|4k randwrite|	6017.25|	6010.5|	6016|	6011.25|

#### Network
![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/59c48e61ab0ab52efb8cc781/9f6f0e494511b156518999e7cba71ad7/Single_Container_-_Network.png)

||||||					Gbits/sec|
|-|-|-|-|-|-|
|runV XenPV|	8.40|	8.44|	8.56|	8.34|	8.435|
|Docker on GCE|	6.24|	6.25|	6.11|	6.21|	6.2025|
|native GCE|	25.7	|26.1	|24.8|	26.2|	25.7|

## Case 2: 100 containers per instance

Another case we did is to launch a customized instance with bigger CPU/Mem size, and launch 100 virtual containers simultaneously in the instance.

#### Setup
- GCE instance:
	- Instance Size: customized
		- vCPU: 24
		- Memory: 128GB
		- Disk: 4 * 100GB SSD persistent disk
			- GCE limits: 3000 IOPS (READ, WRITE), 48MB/s throughput (READ, WRITE)
			- We create 25 partitions (4GB each) on every disk. In total, there are 25 * 4 = 100 paritions. And we assign 100 partitions to 100 container with passthrough mode 

- runV:
	- Xen Dom0 Memory: 10GB
	- Container (DomU) vCPU: 1
	- Container Memory: 1GB


#### Result
Raw test results, fio config files and test scripts can be found at:
- [runV 100 containers disk IO](https://gist.github.com/bergwolf/4e38649193394367ef19dece73837374)
- [host 100 fs IO test](https://gist.github.com/bergwolf/6a91a22c2cf594eeba8cb2e9ec4244ba)

#### CPU

![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/59c48e61ab0ab52efb8cc781/cffba97cef9a7e49cc28cd1345daaa94/100_Containers_-_CPU.png)

|| sysbench runtime (seconds)|
|-|-|
|runV(XenPV)|51.460241|
|docker|49.809762|

#### Memory
![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/59c48e61ab0ab52efb8cc781/1dcfad4c19862d1a05082755a9239dd7/100_Containers_-_Memory.png)

||seq read(MB/s)|	random read(MB/s)|	seq write(MB/s)|	random write(MB/s)|
|-|-|-|-|-|
|runV(XenPV)|276960|	184310|	176670|	95210|
|docker|273570|	183570|	166460|	91180|

#### Disk I/O
![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/59c48e61ab0ab52efb8cc781/76809231f60cce4bffef7fd2f54facf8/100_Containers_-_Disk_IO.png)

|KB/s|	XenPV|	XenPV|	XenPV|	XepPV|	XenPV avg|	GCE native avg|
|-|-|-|-|-|-|-|
|128k randread|	216601|	216629|	216614|	216604|	216612|	224867|
|128k randwrite|	216417|	216382|	216340|	216414|	216388.25|	224653|
|4k randread|	52829|	52840|	52836|	52845|	52837.5|	55266|
|4k randwrite|	52680|	52668|	52685|	52684|	52679.25|	53442|

#### Network
![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/59c48e61ab0ab52efb8cc781/8960d2ccd6599e8a6786a1a9ae5ee399/100_Containers_-_Network.png)


||Gbits/s|
|-|-|
|runV(XenPV)|3.89|
|docker|1.03|

## Result Analysis
- The performance of runV (with Xen PV mode) on GCE is almost identical with native GCE instance in terms of CPU, memory and Disk I/O.
- GCE block device has read/write cache, and its IO throttling method is slow to start. This leads to very high performance at the very beginning of each test run, which we chose to ignore.
- Network performance is faster than docker container on GCE.
