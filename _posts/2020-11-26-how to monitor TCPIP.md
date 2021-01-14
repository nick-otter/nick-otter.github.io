---
layout: post
title:  "How to monitor TCPIP"
categories: linux kernel network 
---

# How to monitor TCP/IP
{: style="text-align: center"}

Written by Nick Otter.

# Contents

- [**Introduction**](#introduction)<br>
     - [TCP/IP overview](#tcp/ip-overview)<br>
     - [Requirements](#requirements)<br>
     - [Overview of TCP tools](#overview-of-TCP-tools)<br>
- [**Sniffing packets with tcpdump**](#sniffing-packets-with-tcpdump)<br>
- [**Understanding dopped packets**](#understanding-dropped-packets)<br>
- [**Analysing TCP/IP packets**](#analysing-tcp/ip-packets)<br>
     - [TCP flags](#tcp-flags)<br>
     - [Checksum](#checksum)<br>
     - [Checksum errors and TCP](#checksum-errors-and-tcp)<br>
     - [Window size](#window-size)<br>
     - [Retransmissions](#retransmissions)<br>
- [**IPv4 header cheatsheet**](#ipv4-header-cheatsheet)<br>
- [**TCP header cheatsheet**](#tcp-header-cheatsheet)
<br><br><br><br>

# Introduction

If you manage a server that receives or transmits over TCP/IP, how can this be monitored? 

Using tcpdump and maybe some wireshark, let's work it out. Jump to the end of the article for some cheatsheets on IPv4 and TCP headers. 

# TCP/IP overview

IP is a protocol that obtains the address to which data is sent. TCP is responsible for the delivery of that data after establishing a connection to the IP address of where data is to be sent. 

When data is sent, it's known as a packet - and contains a header with values to ensure secure delivery and correct processing.

Here's how a packet is sent from host to client. Neat right? IP is a `network/internet` layer protocol and TCP is a `transport` layer protocol. (We're not going to unpick `segment` `datagram` `packet` differences in this article. Datagram and packet can be interchangeable. It is helpful though to consider a TCP packet as a **segment of data** that can be **fragmented** across multiple packets). <br><br>

![](https://docs.oracle.com/cd/E19455-01/806-0916/images/ipov.fig88.epsi.gif)

# Requirements

| Updated | `05/2020` | 
| Linux | `Kernel 5.4` `RHEL 8 4.18` |

# Overview of TCP tools

| Action | `firewall-cmd` | `netcat` | `netstat` | `nmap` | `tcpdump` | `wireshark` |
|-- |-- |-- |-- |-- |-- |--
| Open, close ports. | &#9745; |||| | |
| Send `TCP` packets. | | &#9745; | | | | |
| List all `TCP` listening ports, port connections, listening programs. | | | &#9745;| &#9745;|| |
| Sniff packets in console by port, host, hostname/IP. | | | ||&#9745;|&#9745;|
| Analyse packet capture file dump. | | | ||| &#9745; |

<br><br><br>

# Sniffing packets with tcpdump

Now that's out the way, let's look at some packets. We're going to use `tcpdump` to capture all TCP traffic on a port on an interface. Verbose mode (`-v`) is switched on to see some additional fields like `checksum` and `ttl`. 

```
$ tcpdump -i lo port 105 -v
```
```
[root@rhel-8-1 ~]# tcpdump -i lo port 105 -v
tcpdump: listening on lo, link-type EN10MB (Ethernet), capture size 262144 bytes
14:56:15.265414 IP (tos 0x0, ttl 64, id 11574, offset 0, flags [DF], proto TCP (6), length 60)
    localhost.57840 > localhost.csnet-ns: Flags [S], cksum 0xfe30 (incorrect -> 0xf2ce), seq 3607771753, win 43690, options [mss 65495,sackOK,TS val 2565023151 ecr 0,nop,wscale 7], length 0
14:56:15.265436 IP (tos 0x0, ttl 64, id 0, offset 0, flags [DF], proto TCP (6), length 60)
    localhost.csnet-ns > localhost.57840: Flags [S.], cksum 0xfe30 (incorrect -> 0x3ffa), seq 1808500837, ack 3607771754, win 43690, options [mss 65495,sackOK,TS val 2565023151 ecr 2565023151,nop,wscale 7], length 0
14:56:15.265448 IP (tos 0x0, ttl 64, id 11575, offset 0, flags [DF], proto TCP (6), length 52)
    localhost.57840 > localhost.csnet-ns: Flags [.], cksum 0xfe28 (incorrect -> 0x123f), ack 1, win 342, options [nop,nop,TS val 2565023151 ecr 2565023151], length 0
14:56:16.346896 IP (tos 0x0, ttl 64, id 11576, offset 0, flags [DF], proto TCP (6), length 56)
    localhost.57840 > localhost.csnet-ns: Flags [P.], cksum 0xfe2c (incorrect -> 0x3880), seq 1:5, ack 1, win 342, options [nop,nop,TS val 2565024232 ecr 2565023151], length 4
14:56:16.346958 IP (tos 0x0, ttl 64, id 40761, offset 0, flags [DF], proto TCP (6), length 52)
    localhost.csnet-ns > localhost.57840: Flags [.], cksum 0xfe28 (incorrect -> 0x09c9), ack 5, win 342, options [nop,nop,TS val 2565024232 ecr 2565024232], length 0
^C
5 packets captured
10 packets received by filter
0 packets dropped by kernel
``` 

What can we see? Let's identify an IP and TCP packet from that output. IP encapsulates TCP. So we'll look at them as a pair. We'll move into investigating the header values later on. But first, let's look at the first pair.

Here's a breakdown of the separate packets shown in that output:

| `14:56:15.265414 IP (tos 0x0, ttl 64, id 11574, offset 0, flags [DF], proto TCP (6), length 60)`|
|--
| IP packet.

| `localhost.57840 > localhost.csnet-ns: Flags [S], cksum 0xfe30 (incorrect -> 0xf2ce), seq 3607771753, win 43690, options [mss 65495,sackOK,TS val 2565023151 ecr 0,nop,wscale 7], length 0` |
|--
| TCP packet.

<br><br>

# Understanding dropped packets

In the tcpdump output we can see `packets` `captured` `received by filter` and `packets dropped by kernel`. 

Packets get dropped due to no having enough `buffer` space in the kernel to capture them - why not increase buffer size (`4069` is KB...):
```
$ tcpdump -B 4069
```

Or, pass the `-c` flag. This sets the number of packets captured. A low number here will now stop the buffer size from maxing out (`20` means stream will stop after 20 packets have been captured...):
```
$ tcpdump -i lo -c 20 port 105
```

Or, you can even save `buffer` space by stopping `tcdump` doing unneccessary DNS queries to resolve IPs by using the `-n` (no lookups) flag:
```
$ tcpdump -n port 105
```

Or, why not all three:
```
$ tcpdump -c 20 -n -B 4069 port 105
```

Nice.

<br><br>

# Analysing TCP/IP packets 

Now that's out the way, let's look at the output again and check for errors. Check out the TCP and IPv4 header cheatsheets (our case is `IPv4`) down below if you want to make sure I'm going in the right direction. 

```
14:28:13.932874 IP (tos 0x0, ttl 64, id 31492, offset 0, flags [DF], proto TCP (6), length 56)
```
```
    localhost.53584 > localhost.csnet-ns: Flags [P.], cksum 0xfe2c (incorrect -> 0xa42e), seq 907910293:907910297, ack 438653541, win 342, options [nop,nop,TS val 1396465569 ecr 1396369057], length 4
```

Okay, right off the bat - the IP header all looks okay. `tos` is set to default (see [type of service hex table](https://www.tucny.com/Home/dscp-tos)), `ttl` is just default too (see [time to live wiki](https://en.wikipedia.org/wiki/Time_to_live)). `offset` being `0` means the size of data being sent isn't large enough to need to be fragmented and `Flags` goes in turn with offset - `DF` standing for 'Don't Fragment'. `id` could be useful if we have a problem.  

The TCP header...

I can't see an `ACK` flag, I'd expect to see an acknowledgment flag that confirms the receipt of data. But, there's no reset or `RST` flag which shows a terminated connection due to error. So maybe all's okay... we'll look into TCP flags. There's a `checksum` error so we'll investigate that. Understanding retransmissions and `win` (`window size`) is very important too. 

So from that trace we'll look at:
* TCP flags,
* checksum, 
* window size, 
* retransmissions / timeouts.

<br><br>

# TCP flags

Let's breakdown the TCP flags from that trace. 

In order, here are the Flags seen starting from the sender in the `tcpdump` output:
```
Flags [S]
Flags [S.]
Flags [.]
Flags [P.]
Flags [.]
```
Each TCP packet sent between host and client, contains a flag to indicate a particular connection state or provide additional information about how the connection should be managed.

`tcpdump` abbreviates these flags. In that order above they read like a conversation (as expected). TCP establishes a connection before data can be sent. This is that 'handshake'.

1. `Flags [S]` Sending host: `SYN` (initialise connection),<br>
2. `Flags [S.]` receiving host response: `SYN` `.` (initialise connection, acknowledged) (`.` stands for `ACK`!),<br>
3. `Flags [.]` sending host response: `.` (acknowledged).<br>
4. `Flags [P.]` Sending host: `PSH` (process data immediately without using the buffer),<br>
5. `Flags [.]` receiving host response: `.` (acknowledged).<br>

Here's a diagram just to hammer these flags home as well as show the stages of TCP establishing a connection.<br>
![](http://216.92.67.219/free/diagrams/tcpopen3way.png)

Remember, it would be important to look out for `Flags [R]`, which stands for `RESET`, as that means the connection has been terminated ungracefully (`FIN` flag is graceful termination).

With tcpdump we can filter for flags during a session. To filter for `SYN` and `RST` flags on network interface `loopback`:

```
$ tcpdump -i lo 'tcp[tcpflags] == tcp-syn or tcp[tcpflags] == tcp-rst'
```

<br><br>

# Checksum

The next highlight from that output was a `checksum` reading. 
```
cksum 0xfe30 (incorrect -> 0xf2ce)
```
A checksum error no less! Let's look at the structure of a TCP header to find the checksum field. Remember, the range across the top is in Bits.

![](https://user-images.githubusercontent.com/26765027/82868775-1dc0c380-9f25-11ea-9c4e-9079a081608e.jpg)

The TCP checksum is used to detect corruption of data over a connection. It is calculated like so:

* `'Pseudo' IP header values` `+` `TCP header values` `=` `checksum value`

The Destination IP also carries out this calculation and compares it to the Source IP that sent the packet. If the checksum values differ this means (in theory) that values in the packet received are different to header values in the packet sent.

`Pseudo IP header values` that are added together are:
```
Protocol
Source Address
Destination Address
TCP Length
```
(Source and Destination Adress are taken from the `IP` header).

`TCP header` values that are added together are:
```
Source Port
Destination Port
Sequence Number
Acknowledgement Number
Data Offset
Reserved
Flags
WIndow Size
Checksum (set to 0x0000) 
Urgent Pointer
```

In the `tcpdump` session above, the checksum showed an error - an incorrect match between Source and Destination IP calculations (`cksum 0xfe30 (incorrect -> 0xf2ce)`) let's see if we can work out why.

Looking at the actual TCP session in the terminal, all looks okay? We can see that data we want to be transmitted has been received.
```
[root@rhel-8-1 ~]# nc localhost 105
foo
```
```
[root@rhel-8-1 ~]# nc -l 105
foo
```
<br><br>

# Checksum errors and TCP

To save suspense I'm going to jump forward a couple of steps. Often a Checksum error isn't actually a good indicator that something's gone wrong with TCP segments. The receiver **will not** discard or drop packets if a checksum error is detected.

With TCP segments, checksum errors can mostly be attributed to `TCP checksum offloading`. The CPU doesn't calculate the TCP checksum, that calculation is left to the network interface. The network interface then calculates it incorrectly.  

Other cause of checksum errors could be:
* a faulty layer 3 (`network`) device, 
* man-in-the-middle packet manipulation - very unlikely, as man-in-the-middle would be able to calculate a proper checksum, it wouldn't necessarily cause a checksum error.

<br><br>

# Window size 

Here's that trace again. 
```
[root@rhel-8-1 ~]# tcpdump -i lo port 105 -v
tcpdump: listening on lo, link-type EN10MB (Ethernet), capture size 262144 bytes
14:56:15.265414 IP (tos 0x0, ttl 64, id 11574, offset 0, flags [DF], proto TCP (6), length 60)
    localhost.57840 > localhost.csnet-ns: Flags [S], cksum 0xfe30 (incorrect -> 0xf2ce), seq 3607771753, win 43690, options [mss 65495,sackOK,TS val 2565023151 ecr 0,nop,wscale 7], length 0
14:56:15.265436 IP (tos 0x0, ttl 64, id 0, offset 0, flags [DF], proto TCP (6), length 60)
    localhost.csnet-ns > localhost.57840: Flags [S.], cksum 0xfe30 (incorrect -> 0x3ffa), seq 1808500837, ack 3607771754, win 43690, options [mss 65495,sackOK,TS val 2565023151 ecr 2565023151,nop,wscale 7], length 0
14:56:15.265448 IP (tos 0x0, ttl 64, id 11575, offset 0, flags [DF], proto TCP (6), length 52)
    localhost.57840 > localhost.csnet-ns: Flags [.], cksum 0xfe28 (incorrect -> 0x123f), ack 1, win 342, options [nop,nop,TS val 2565023151 ecr 2565023151], length 0
14:56:16.346896 IP (tos 0x0, ttl 64, id 11576, offset 0, flags [DF], proto TCP (6), length 56)
    localhost.57840 > localhost.csnet-ns: Flags [P.], cksum 0xfe2c (incorrect -> 0x3880), seq 1:5, ack 1, win 342, options [nop,nop,TS val 2565024232 ecr 2565023151], length 4
14:56:16.346958 IP (tos 0x0, ttl 64, id 40761, offset 0, flags [DF], proto TCP (6), length 52)
    localhost.csnet-ns > localhost.57840: Flags [.], cksum 0xfe28 (incorrect -> 0x09c9), ack 5, win 342, options [nop,nop,TS val 2565024232 ecr 2565024232], length 0
^C
5 packets captured
10 packets received by filter
0 packets dropped by kernel
```
Let's look at the `window size` (`win`) parameter, starting with the host.

```
win 43690
win 43690
win 342
win 342
win 342
```

Welcome to TCP `flow control`. Window size is used to manage the flow of data between client and host - it denotes the number of Bytes a machine is able to accept. The numbers in the trace above are from the `receive window` (`rwnd`) - the spare room in the `receive buffer` for data. If the receive buffer is full (window size `0`), packets will get dropped due to not having enough space to handle the data.

There are several factors which affect TCP window size (for `Linux`, `RHEL` `8`; unsure for other OS.):

* the size of socket's receive buffer,
* TCP maximum segment size (MSS),
* the window scale extension,
* TCP Timestamps,
* tcp_workaround_signed_windows parameter.

How do I know what my machine's window size should be set to? How can I change the window size? 

To understand the appropriate TCP window size for your machine, work out what your desired data throughput is. Use this calculation to calculate throughput:
```
TCP window size (bits) / Latency (ms) = Throughput (bits per second)
```
There's a helpful (calculator)[https://www.speedguide.net/bdp.php] for throughput online too :)

By default, window size can't exceed ~`65` `KB` due to limitations with the TCP header. However... `window scaling` was introduced in [RFC1323](https://www.ietf.org/rfc/rfc1323.txt) to adjust window size and improve performance. 

TCP window size elements are set using kernel parameters so can be configured via `sysctl`. The TCP options are listed in `man 7 tcp`, and they are located in the `net.ipv4` namespace. To change the TCP window scale size, update `tcp_rmem`. And for more info about the `scale factor` for these values, head to [Redhat](https://access.redhat.com/solutions/29455).

```
[root@rhel-8-1 ~]# sysctl net.ipv4.tcp_rmem
net.ipv4.tcp_rmem = 4096	87380	6291456
```
| `4096` | `87380` | `6291456` |
|--|--|--
| Minimum window. | Default window. | Maximium window. |

Or, to ensure maximum window size is always used. Turn off TCP window scaling. This will force all TCP connections to use a 64KB window.
```
[root@rhel-8-1 ~]# sysctl net.ipv4.tcp_window_scaling=0
net.ipv4.tcp_window_scaling = 0
```

All TCP window size kernel parameters listed:
```
net.ipv4.tcp_timestamps
net.core.rmem_max
net.ipv4.tcp_rmem
net.ipv4.tcp_adv_win_scale
net.ipv4.tcp_window_scaling
net.ipv4.tcp_workaround_signed_windows
```

<br><br> 

# Retransmissions

Retransmission occurs when a sender doesn't receives an acknowledgement for the data that's been sent before a timeout. If no acknowledgement is received before the allotted time, the segment is `retransmitted`. Missing further acknowledgements, an `RTO` (`retransmission timeout`) state is entered before the retransmission of the packet is attempted again.

Updating the retransmission timeout setting is done at the `network` layer when adding the route. This is an example of how to reduce the RTO setting when adding a connection with the parameter `rto_min` (taken from [Redhat](https://access.redhat.com/solutions/2340811)). To confirm, just show the route again.
```
$ ip route change default via 10.0.2.2 dev eth0 rto_min 5ms
```
```
$ ip route
10.0.2.0/24 dev eth0  proto kernel  scope link  src 10.0.2.15 
169.254.0.0/16 dev eth0  scope link  metric 1002 
default via 10.0.2.2 dev eth0  rto_min lock 5ms
```

To see current retransmission settings use socket statistics command `ss`. Look for `rtt` (`retransmission time`) in the trace below.
```
$ ss -i
State       Recv-Q Send-Q                                               Local Address:Port                                                   Peer Address:Port   
ESTAB       0      0                                                        10.0.2.15:ssh                                                        10.0.2.2:50714   
     cubic rto:201 rtt:1.5/0.75 ato:40 cwnd:10 send 77.9Mbps rcv_space:14600
ESTAB       0      0                                                        10.0.2.15:39042                                                 216.58.216.14:http    
     cubic rto:15 rtt:5/2.5 cwnd:10 send 23.4Mbps rcv_space:14600
```


It's tricky to see retransmissions with tcpdump, so let's use wireshark filters. You can write the output of tcpdump to file with the `-w` flag. This file can then be read in `wireshark`.

```
tcp.analysis.retransmission
```

It's also worth looking at congestion control and `FRTO` if you have time, [here](https://whitequark.org/blog/2011/09/12/tweaking-linux-tcp-stack-for-lossy-wireless-networks/).
<br><br>

And here are the long requested cheatsheets..

# IPv4 header cheatsheet

Here's a diagram of the `IPv4` packet header, with tcpdump abbreviations in blue (`IP` in the trace shows it's an `IPv4` and not `IPv6` packet). The range at the top and right hand side is number of Bytes.

See also [RFC791](https://tools.ietf.org/html/rfc791).<br>
![](https://packetpushers.net/wp-content/uploads/2013/09/IPv4-Headers-Standard-tcpdump-names-1024x431.png)

`IPv4` trace description.

|||
|--|--
| **`14:28:13.932874`** | Timestamp header was captured by `tcpdump`. | 
| **`IP`**                | `IP` shows it's an `IPv4` packet, not `IPv6`.|
| **`tos 0x0`**         | Type of service (`TOS`) value. Specifies the parameters for the type of service requested. So networks can handle the transport of the packet/datagram correctly. Based on this value a packet would be placed in a prioritized outgoing queue, or take a route with appropriate latency, throughput, or reliability. See `TOS` hex table [here](https://www.tucny.com/Home/dscp-tos). |
| **`ttl 64`**           | Hops a packet can travel before it gets dropped to make sure the network doesn't get congested. `TTL` (Time to Live) can be set up to `225` by the sender of the packet. When the TTL field is decremented down to zero, the datagram is discarded. |
| **`id 31492`**         | Identifier for parts of a `fragmented` header ([read more](https://packetpushers.net/ip-fragmentation-in-detail/)).|
| **`offset 0`**         | The fragment offset - used for fragmented packets ([read more](https://packetpushers.net/ip-fragmentation-in-detail/)).|
| **`flags [DF]`**       | Any flags set. Used to control or identify fragments. `DF` stands for Don't Fragment. If the DF flag is set & fragmentation is required to route the packet, then the packet is dropped. (`MF` stands for More Fragments, which indicates the datagram/packet contains additional fragments). |
| **`proto TCP (6)`**     | `protocol`. Specifies the next encapsulated protocol. In this case, the encapsulated packet in the IP header is `TCP`.|
| **`length 56`**        | The packet length, in Bytes. |

<br><br>

# TCP header cheatsheet

`TCP` datagram header (see also [RFC793](https://tools.ietf.org/html/rfc793)).<br>
![](http://intronetworks.cs.luc.edu/current/html/_images/tcp_header.svg)

`TCP` trace description.

|||
|--|--
| **`localhost.53584 >`**                                   | Source IP address and port. |
| **`> localhost.csnet-ns`**                                | Destination IP adddress and port. |
| **`Flags [P.]`**                                          | TCP flags. `P` stands for `PSH` or "push". The purpose of the `PSH` flag is to tell the receiver to process packets as they are received instead of buffering them. `.` stands for `ACK` or "acknowledged". `ACK` means data from the Source to the Destination IP has been received successfully and a receipt has been sent  back from the Destination IP ([read more](https://www.keycdn.com/support/tcp-flags)).|
| **`cksum 0xfe2c (incorrect -> 0xa42e)`**                  | `Checksum` is a calculation of the 'Pseudo header' (parts of the IP header) and the TCP header executed by the Source IP address. The Destination IP address makes the same calculation and if the values differ, this is a `Checksum` error as supposedly something has corrupted or changed the data packet/datagram header values since it was sent.|
| **`seq 907910293:907910297`**                             | The TCP packet's start and end Byte sequence numbers. This packet has a length of `4` Bytes (`907910297` - `907910293`) and it starts at `907910293`.  |
| **`ack 438653541`**                                       | Acknowledgement number. This is the sequence number of the next Byte the Destination IP expects to receive. |
| **`win 342`**                                             | The Source IP's TCP `window`. This is how much data can be received by the Source IP before it goes into buffer. |
| **`options [nop,nop,TS val 1396465569 ecr 1396369057]`**  | TCP header `options`. See full list of options [here](http://www.networksorcery.com/enp/protocol/tcp.htm#Options). |
| **`length 4`**                                            | The length in Bytes of the TCP header. |

---

Thanks. [TCP checksum error case study](https://www.networkdatapedia.com/post/2017/09/13/tcp-checksum-error-case-study), [Manually create and send raw TCP/IP packets](https://inc0x0.com/tcp-ip-packets-introduction/tcp-ip-packets-3-manually-create-and-send-raw-tcp-ip-packets/), [Checksums](https://www.wireshark.org/docs/wsug_html_chunked/ChAdvChecksums.html),  [Tweaking Linux TCP stack for lossy networks](https://whitequark.org/blog/2011/09/12/tweaking-linux-tcp-stack-for-lossy-wireless-networks/), [TCP initial window size](https://access.redhat.com/solutions/29455) and other articles were useful in writing this. This was written by Nick Otter.
