---
layout: post
title:  "How to monitor disk activity"
categories: linux kernel disk
---

# How to monitor disk activity
{: style="text-align: center"}

Written by Nick Otter.

# Contents

- [**Introduction, disk activity and Linux**](#introduction-disk-activity-and-linux)<br>
     - [Requirements](#requirements)<br>
     - [Overview of disk activity monitoring tools](#overview-of-disk-activity-monitoring-tools)<br>
- [**I/O wait**](#i/o-wait)<br>
- [**See I/O used by devices**](#see-i%2Fo-per-device)<br>
- [**See I/O used by processes**](#see-i/o-per-process)<br>
- [**Test I/O performance**](#test-i/o-performance)<br>
- [**Improving performance**](#improving-performance)<br>

# Introduction, disk activity and linux

Linux disk activity is understood as  **`disk I/O`**.**`I/O`** stands for `input` and `output` instructions on the disk.

Latency always exists between the hard disk actuator arm read/write heads completing `input` and `output` instructions and the RAM's response to CPU queries - positioning the heads on specific disk sectors to perform read writes takes a lot longer. This can lead to the CPU idling, while it waits for a response from the disk.

If lots of disk read writes are required, this can lead to large performance issues due to I/O bottlenecks - monitoring is a must.

How disk I/O is measured:
* **`wait`** time that the CPU is doing nothing while waiting for an `I/O` operation to complete.
* **`read`** read requests issued to the disk.
* **`write`** write requests issued to the disk.
* **`busy`** time that the disk is busy handling requests.
* **`IOPS`** the amount (estimate) of I/O operations that can be carried out per second by the disk.

We'll look at all of these in this article.

# Requirements

| Updated | `05/2020`|
| Linux | `Kernel 5.4` `RHEL 8 4.18` |

# Overview of disk activity monitoring tools

| Action                                      | **`iostat`** | **`iotop`** | **`ioping`**  | **`nmon`** | 
|--                                           |--            |--           |--             |--             
| Display disk I/O in real time.              |  &#9745;     |             | &#9745;       |                 |              
| List processes performing I/O.              |              | &#9745;     |               |                 |
| Test disk I/O.                              |              |             |&#9745;        |                 |
| Analyse disk I/O over time from a dump.     |              |             |               | &#9745;         |

# I/O wait

Ok, let's imagine there's a slow system. Let's use `top` to see if I/O wait is causing the issues.
```
$ top
```
```
[root@rhel-8-1 ~]# top

top - 14:54:18 up  5:56,  1 user,  load average: 0.26, 0.23, 0.14
Tasks: 257 total,   3 running, 254 sleeping,   0 stopped,   0 zombie
%Cpu(s):  2.0 us, 17.3 sy,  0.0 ni,  0.5 id, 55.9 wa, 23.3 hi,  1.0 si,  0.0 st
MiB Mem :   9947.0 total,   2898.1 free,   2380.0 used,   4669.0 buff/cache
MiB Swap:      0.0 total,      0.0 free,      0.0 used.   7250.4 avail Mem 
```

**`55.9 wa`** - this is `I/O` `wait`. Percent of time the CPU(s) is waiting for disk completion over the last sampling period. 
So (this is a 1 core system and top increments percentage by cores - `number of cores` `×` `100%`), over the last period the CPU spent **`55.9`** % of it waiting on the disk (could be 111.8% if it was a 2 core system...).  

There's activity in **`sys`** (which is time spent running kernel space processes); could be expected as we've seen high wait - user space can't access the disk. Hardware interrupts (**`hi`**) also look high - this is an instruction from disk to CPU. 

How to investigate? From the top output we can also see that there are **Written by Nick Otter.Written by Nick Otter.`3`** **`running`** tasks. This really narrows stuff down; some, one or all of those tasks are causing the high wait. To keep investigating we can stay focussed on the disk. 

Let's see:
* which disk is causing high I/O,
* which process is causing high I/O.

# See I/O used by devices

Seeing how much I/O is happening by device can be done with `iostat` - this isn't built in. But, `iotop` (used to see I/O for processes) is, so alternatively you can work backwards from there.
```
$ iostat
```

Let's take a reading every 2 seconds for 1 interval (just for the sake of it). `-x` stands for **`extended report`**. N.B. the first report from iostat prints statistics based on the last time the system was booted - not curent state.
```
[root@rhel-8-1 ~]# iostat -x 2 1
Linux 4.18.0-147.3.1.el8_1.x86_64 (rhel-8-1) 	05/06/20 	_x86_64_	(1 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           3.01    0.01    3.31    0.50    0.00   93.17

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
sda             48.37   15.22   2372.24   3258.12     0.09     3.60   0.19  19.14    0.63    2.51   0.05    49.04   214.07   0.31   1.99
sdb              0.28    0.00      6.34      0.00     0.00     0.00   0.00   0.00    0.44    0.00   0.00    22.65     0.00   0.16   0.00
sdc              0.28    0.00      6.34      0.00     0.00     0.00   0.00   0.00    0.42    0.00   0.00    22.65     0.00   0.15   0.00
scd0             0.36    0.00     27.65      0.00     0.00     0.00   0.00   0.00    1.98    0.00   0.00    76.86     0.00   0.90   0.03
```

| `r/s`                              | `w/s`                               | `rkB/s` | `wkB/s` |
|--                                  |--                                   |--                                  |--      
| Read requests completed per second.| Write requests completed per second.| Number of sectors read from the device. | Number of sectors written to the device per second. |

It's worth noting that the **`%util`** column from the output above is the percentage over the time period that I/O requests were issued to the device. Close to **`100%`** for a while for serial devices is **bad**. However, if the output shows close to 100 for a RAID array setup or modern SSDs - it doesn't reflect a performance limit. (Sounds like an update might be needed..). 

# See I/O used by processes 

```
$ iotop -o
```
```
Total DISK READ :       0.00 B/s | Total DISK WRITE :     923.63 M/s
Actual DISK READ:       0.00 B/s | Actual DISK WRITE:     222.13 M/s
  TID  PRIO  USER     DISK READ  DISK WRITE  SWAPIN      IO    COMMAND
b'12931 be/4 root        0.00 B/s    0.00 B/s  0.00 % 46.53 % [kworker/u2:0+flush-8:0]'
b'15018 be/4 root        0.00 B/s  923.48 M/s  0.00 % 46.47 % dd if=/dev/zero of=/tmp/foo bs=1G count=5 oflag=dsync'
b'14683 be/4 root        0.00 B/s    0.00 B/s  0.00 %  0.04 % [kworker/0:3-xfs-data/sda2]'
b' 3028 idle root        0.00 B/s  147.90 K/s  0.00 %  0.00 % tracker-store [pool]'
b'14945 be/4 root        0.00 B/s    3.61 K/s  0.00 %  0.00 % platform-python -s /usr/sbin/iotop -bo'
```

To persist the data use `-b`, meaning **`batch`** and redirect to a file. **`-o`** stands for `only`; display I/O in use only.
```
$ iotop -bo
```

**`TID`** stands for `thread ID`. In `RHEL` `8` it's the same as the processes PID (`process ID`). So for the iotop trace above, the TID 15018 that's I/O intensive can also be investigated using `ps`.

```
[root@rhel-8-1 ~]# ps -aux | grep 15018
root      15018 33.8 10.3 1265676 1050572 pts/0 D    09:35   0:01 dd if=/dev/zero of=/tmp/foo bs=1G count=5 oflag=dsync
```
```
[root@rhel-8-1 ~]# ps -eLo pid= -o tid= | awk '$2 == 15018 {print $1}'
15018
```

# Test I/O performance

Ok, so how to test I/O performance for your machine? It's possible to get some benchmarks with **`dd`**.

For **server latency**, let's find out how much data per second can be written to disk by using `count`. 

It's important to note, that this will be affected by whatever else you are running on the machine. 

It's also important to note - we're using **`/dev/zero`** (similar to `/dev/random`) as the input file. This random data source will probably take longer than any application.

```
$ dd if=/dev/zero of=/tmp/foo bs=512 count=1000 oflag=dsync
```
```
[root@rhel-8-1 ~]# dd if=/dev/zero of=/tmp/foo bs=512 count=1000 oflag=dsync
1000+0 records in
1000+0 records out
512000 bytes (512 kB, 500 KiB) copied, 9.26722 s, 55.2 kB/s
```

For **throughput**, let's just use a count of 1. This should be a lot faster as this is only 1 write to disk - instead of 1000 write instructions to disk. 
```
$ dd if=/dev/zero of=/tmp/foo bs=1G count=1 oflag=dsync
```
```
[root@rhel-8-1 ~]# dd if=/dev/zero of=/tmp/foo bs=1G count=1 oflag=dsync
1+0 records in
1+0 records out
1073741824 bytes (1.1 GB, 1.0 GiB) copied, 1.59629 s, 673 MB/s
```

There is a calculation, for theoretical maximum IOPS that I've seen from [Scout](https://scoutapm.com/blog/understanding-disk-i-o-when-should-you-be-worried). If you're looking for a further benchmark to debug - this is it. As said in the article, you should aim for `sar` transfers per/second to be as close to your theoretical I/O operations per/second calculation as possible.

```
                              number of disks` * average I/O Operations on 1 disk per/second
I/O operations per/second = ------------------------------------------------------------------
                                 % of read workload + (raid factor *) % of write workload)
```                                     

# Improving performance

Ok, so now we know how to monitor I/O. What about how to improve it? Simple answer - take benchmarks and make sure I/O intensive applications cache more in RAM. Failing that, upgrading to a **`multi disk array`**, configuring a **`RAID`** or simply just having a drive with better rotational speed will improve I/O performance.

Thresholds for I/O are subjective to your setup and the applications running. Like in most monitoring cases, you should always be aware for thresholds staying high over time as opposed to in the moment. 

Not mentioned in this article - among those tools above, **`ioping`** is a very handy. A quick and simple tool to test I/O speed. For persistence, use **`nmon`**; you can export I/O usage over time to a CSV for analysis with **`nmonchart`**.

---

Thanks. [Understanding I/O wait](https://www.witekio.com/blog/understanding-io-wait-0-idle-can-ok/), [Understanding disk I/O when should you be worried](https://scoutapm.com/blog/understanding-disk-i-o-when-should-you-be-worried), [Troubleshooting high I/O wait](https://bencane.com/2012/08/06/troubleshooting-high-io-wait-in-linux/), [Test I/O performance](https://medium.com/@kenichishibata/test-i-o-performance-of-linux-using-dd-a5074f1de9ce), [Calculate I/O in a storage array](https://www.techrepublic.com/blog/the-enterprise-cloud/calculate-iops-in-a-storage-array/) were useful to write this. This was written by Nick Otter.
