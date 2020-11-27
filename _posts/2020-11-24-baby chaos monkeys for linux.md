---
layout: post
title:  "Baby chaos monkeys for Linux"
categories: linux general
---

# Baby chaos monkeys for Linux
{: style="text-align: center"}

# Contents

- [**Introduction**](#introduction)<br>
   - [Requirements](#requirements)<br>
- [**Kernel**](#kernel)<br>
   - [How to cause a kernel panic](#how-to-cause-a-kernel-panic)<br>
- [**Memory**](#memory)<br>
   - [How to cause a segfault](#how-to-cause-a-segfault)<br>
- [**CPU**](#cpu)<br>
   - [How to max out CPU as quickly as possible](#how-to-max-out-cpu-as-quickly-as-possible)<br>
- [**Network**](#network)<br>
   - [How to send an incorrect TCP checksum](#how-to-send-an-incorrect-TCP-checksum)<br>
- [**File System**](#filesystem)<br>
   - [How to corrupt a filesystem](#how-to-corrupt-a-filesystem)<br>
   - [How to orphan an inode](#how-to-orphan-some-inodes)<br>
<br><br><br>

# Introduction

I'm not sure if these are really [**chaos monkeys**](https://en.wikipedia.org/wiki/Chaos_engineering) but I've found these commands useful to test stuff. Here's the (growing) list. I add to it when I remember. Collaborators, complaints and pull requests welcome. Most of these are just terminal commands, but that might change in future.

# Requirements

| Linux | `RHEL 8 4.18` |

# Kernel

**How to cause a kernel panic**

```
$ echo 1 > /proc/sys/kernel/sysrq
$ echo c > /proc/sysrq-trigger
```
![](https://media1.tenor.com/images/0d19c4b59db501d0b5c0dd897a234055/tenor.gif?itemid=5799525)

# Memory

**How to cause a segfault**

Taken from Joey Adams on [this thread](https://codegolf.stackexchange.com/questions/4399/shortest-code-that-raises-a-sigsegv). Bye bye!

```
$ kill -11 $$
```

# CPU

**How to max out CPU as quickly as possible**

Do you want to write repeatedly write a string to STDOUT without any output constraints to give your CPU an eternal workout?<br><br>
![](https://media.giphy.com/media/svcVqVsSvzm0g/giphy.gif)

```
$ yes > /dev/null
```

# Network

**How to send an incorrect TCP checksum**

Hello Netcat. Start a netcat listener on the receiver machine on your desired port. Then let's send something over TCP.
```
$ (i=0; while true; do echo aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa $i; i=$((i+1)); sleep 1; done) | netcat IP PORT
```
In my case this was happening over the `/dev/eth0` network interface. But switch where appropriate. This should do it!
```
$ sudo tc qdisc add dev eth0 root netem corrupt 100%; sleep 10; sudo tc qdisc del dev eth0 root netem
```

# Filesystem

**How to corrupt a filesystem**

Let's do some random block writes to a partition, bypassing the volume's filesystem, this will mess it up even with journaling enabled.
```
$ sudo dd if=/dev/zero of=/partition bs=1k seek=10 count=4k
```

**How to orphan some inodes**

```
$ touch home && tail -f home &
```
Then...
```
$ bash -c `rm home; bash`
```
:'(

---

Thanks.
