---
published: true
date: '2024-10-03 00:20 +0200'
title: Use tcpdump on eXR for control plane troubleshooting - part 1
author: Dimitri Mikhailov
tags:
  - iosxr
  - cisco
  - Control-Plane
  - Packet-Capture
  - Tcpdump
excerpt: >-
  This article introduces how to use tcpdump on Cisco XR platforms running eXR,
  such as the ASR 9000 and NCS 5500.
position: hidden
---
{% include toc icon="table" title="Use tcpdump on eXR for control plane troubleshooting - part 1" %}
# Introduction
In today's fast-paced world, efficient and quick troubleshooting is a key skill for any network engineer. This article will shed some light on a few available tools used for investigating control plane protocol issues on IOS-XR devices. Each tool has its own strengths and limitations. Understanding them will help you select the best tool for a given task in order to get the most valuable data and save time while avoiding pitfalls.  

The most common tools are the process debugs, so let's start by looking at how they work.

# Process debugs
In a nutshell, debugs are simply a series of messages that a process may display as a response to certain events. Debugs are user-friendly but can consume CPU and despite being quite verbose, often it is required to run several debugs at once to get all the relevant data. Last but not least, to print debugs, a process would always require some kind of trigger, such as the reception of a control plane packet on process level.
<div class="notice--info">
  <b>Key takeaways</b>
  <ol>
    <li>Run by a CPU process at application level.</li>
    <li>Easy to use with a similar syntax across different platforms.</li>
    <li>Often very verbose, and sometimes not very granular.</li>
    <li>Display data, very specific to a given process.</li>
    <li>Require a valid trigger to display a debug message.</li>
  </ol>
</div>

## Interpreting process debugs
For example, the debug output from a Border Gateway Protocol process for an incoming BGP packet suggests two things: 
![BGP-1.png]({{site.baseurl}}/images/eXR-tcpdump/BGP-1.png){: width="800"}
- First, the packet has been successfully received by our network interface.
{: .text-left}
![BGP-2.png]({{site.baseurl}}/images/eXR-tcpdump/BGP-2.png){: width="800"}
- Second, the packet has reached the BGP process level.  
{: .text-left}
  
  
  
If debugs _fail_ to show traces of our packets, it would prompt several questions:  
![BGP-3.png]({{site.baseurl}}/images/eXR-tcpdump/BGP-3.png){: width="800"}
- Did the BGP packets reach our process level ?
{: .text-left}
![BGP-4.png]({{site.baseurl}}/images/eXR-tcpdump/BGP-4.png){: width="800"}
- If not, where and why were they dropped ?
{: .text-left}
![BGP-5.png]({{site.baseurl}}/images/eXR-tcpdump/BGP-5.png){: width="800"}
- Most importantly, did these drops occurred _inside_ the device, or _before_ reaching our network interface ?
{: .text-left}

# Interface packet capture
To answer that last question, we can try capturing traffic directly from the network interface, to get the evidence of what is happening on a wire. Unfortunately, this approach isn't always feasible due to a potentially high interface traffic volume or other factors such as the availability of a packet capturing equipment.

<div class="notice--info">
  <b>Key takeaways</b>
  <ol>
    <li>Collects packets from the device network interface.</li>
    <li>Acts as evidence of what is happening on a wire.</li>
    <li>May be challenging due to:</li>
    <ul>
      <li>High interface traffic.</li>
      <li>Availability or hardware capability of a packet capturing equipment.</li>
    </ul>
  </ol>
</div>


So, what other options do we have when both process debugs and interface capture tools can't provide us with the answers we're looking for ?

I'm glad you asked!

And in this first article, I'll show you a less known technique for collecting control plane packets with the built-in tcpdump tool. 

# Tcpdump
Tcpdump is a common Unix utility for capturing network traffic, which can also be used internally on IOS-XR devices. The tcpdump is available on the 64-bit IOS-XR Software or eXR, running on Cisco platforms such as the ASR9000 or NCS5500.  

Before diving in, let me first clarify a few keywords we'll be using going forward.

## Control plane vocabulary
To start with, any packet handled by the device local CPU is click called _"for us"_. Next, let’s take a look at how these _"for us"_ packets reach our process level. The incoming network interface traffic is always a combination of transit packets and some locally destined control plane packets.
![Punt-diagram.png]({{site.baseurl}}/images/eXR-tcpdump/Punt-diagram.png){: width="385" .align-left padding: 5px;}
1. Incoming packets on the linecard or LC network interface will reach a _Network Processor Unit_ or NPU, where _"for us"_ packets are redirected towards the local CPU, an action called _PUNT_.

2. To reach the linecard CPU, punted packets will follow a green pathway via a _Control Ethernet Switch_, acting as a bridge between CPU and NPU.

3. The transit traffic will use the orange path towards the fabric.

4. Control Ethernet Switches from each linecard are interconnected as an internal ethernet control plane network to which the Route Processor or RP have access.  

## Using tcpdump on RP punt interfaces
So with all those definitions out of the way, the main idea now is to use the tcpdump on punt interfaces available from the RP or the LC shell.  

While access to the RP shell is straightforward, connecting to the LC shell is more platform-dependent. As such, this topic will require a separate article in the future to cover all aspects, so stay tuned.  

The most common use case for this technique is to capture routing protocol packets punted to the RP CPU. However, it can also be used for any other packets the CPU would receive, such as SNMP, NTP, LDP, etc.  

### Demo
Let me show you the next steps by using our lab NCS5500 router. 

<div class="highlighter-rouge">
<pre class="highlight">
<code>RP/0/RP0/CPU0:FRETA3#<b>show platform</b>
Sun Sep 15 20:56:49.809 UTC
Node              Type                       State             Config state
--------------------------------------------------------------------------------
0/0/CPU0          NC57-18DD-SE               IOS XR RUN        NSHUT
0/0/NPU0          Slice                      UP                
0/0/NPU1          Slice                      UP                
0/1/CPU0          NC55-32T16Q4H-AT           IOS XR RUN        NSHUT
0/1/NPU0          Slice                      UP                
0/RP0/CPU0        NC55-RP2-E(Active)         IOS XR RUN        NSHUT
0/FC1             NC55-5508-FC2              OPERATIONAL       NSHUT
0/FC3             NC55-5508-FC2              OPERATIONAL       NSHUT
0/FC5             NC55-5508-FC2              OPERATIONAL       NSHUT
0/FT0             NC55-5508-FAN2             OPERATIONAL       NSHUT
0/FT1             NC55-5508-FAN2             OPERATIONAL       NSHUT
0/FT2             NC55-5508-FAN2             OPERATIONAL       NSHUT
0/PM0             N9K-PAC-3000W-B            OPERATIONAL       NSHUT
0/PM1             N9K-PAC-3000W-B            OPERATIONAL       NSHUT
0/PM2             N9K-PAC-3000W-B            OPERATIONAL       NSHUT
0/SC0             NC55-SC                    OPERATIONAL       NSHUT
0/SC1             NC55-SC                    OPERATIONAL       NSHUT
 
RP/0/RP0/CPU0:FRETA3#<b>show install active summary</b>
Sun Sep 15 20:56:52.036 UTC
Label : 7.10.2
    Active Packages: 4
        ncs5500-xr-7.10.2 version=7.10.2 [Boot image]
        ncs5500-isis-1.0.0.0-r7102
        ncs5500-mpls-1.0.0.0-r7102
        ncs5500-mcast-1.0.0.0-r7102
</code></pre></div>

For this simple test, we'll use locally generated traffic, such as ICMP requests sourced from the RP, towards our own interface IP address `192.168.99.1`. By using the `loopback internal` configuration on that interface, we can loop packets in such a way that they will appear to the NP as incoming from the physical interface itself.

<div class="highlighter-rouge">
<pre class="highlight">
<code>RP/0/RP0/CPU0:FRETA3#<b>sh run int Te0/1/0/31</b>
Wed Oct  2 20:35:47.213 UTC
interface TenGigE0/1/0/31
 ipv4 address 192.168.99.1 255.255.255.0
 loopback internal
!</code></pre></div>

First, I’ll connect to the RP shell, using a `run` command from the XR command line.

<div class="highlighter-rouge">
<pre class="highlight">
<code>RP/0/RP0/CPU0:FRETA3#<b>run</b>
Sun Sep 15 20:56:57.093 UTC
[xr-vm_node0_RP0_CPU0:~]$
</code></pre></div>

Second, I’ll confirm the punt interface for our capture, that interface is platform specific as you can see in this table.

| Platform | Punt interface                             |
| -------- | ------------------------------------------ |
| asr9000  | eth-punt.1282                              |
| ncs5500  | ps-inb.1538                                |
| NCS55A2  | eth-vf2                                    |
| NCS540   | eth-vf2                                    |
| NCS560   | spp_br, but we'll use ps-inb.1538, instead |

Instead of trying to remember it, the punt interface can be found by reading the bootstrap config file named `calvados_bootstrap.cfg` and searching for the `GUEST1_PUNT_ETH` string using Unix `cat` and `grep` commands.

<div class="highlighter-rouge">
<pre class="highlight">
<code>[xr-vm_node0_RP0_CPU0:~]$<b>cat /etc/init.d/calvados_bootstrap.cfg | grep GUEST1_PUNT_ETH</b>
<mark style="background: #E3E6E8; margin: 0 -0.15em; padding: 0.1em 0.15em; border-radius: 0.2em; -webkit-box-decoration-break: clone; box-decoration-break: clone;">GUEST1_PUNT_ETH=<mark style="background: #A9E0FF; margin: 0 -0.15em; padding: 0.1em 0.15em; border-radius: 0.2em; -webkit-box-decoration-break: clone; box-decoration-break: clone;">ps-inb.1538</mark></mark></code></pre></div>

The output shows the punt interface as `ps-inb.1538` which we will use with `-i` flag to set the tcpdump capturing source interface, as our final step:

<div class="highlighter-rouge">
<pre class="highlight">
<code>[xr-vm_node0_RP0_CPU0:~]$<b>tcpdump <mark style="background: #E1D1FF; margin: 0 -0.15em; padding: 0.1em 0.15em; border-radius: 0.2em; -webkit-box-decoration-break: clone; box-decoration-break: clone;">-i</mark> <mark style="background: #A9E0FF; margin: 0 -0.15em; padding: 0.1em 0.15em; border-radius: 0.2em; -webkit-box-decoration-break: clone; box-decoration-break: clone;">ps-inb.1538</mark></b>
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ps-inb.1538, link-type EN10MB (Ethernet), capture size 262144 bytes
20:58:06.243219 4e:41:50:00:10:01 (oui Unknown) > 4e:41:50:00:01:01 (oui Unknown), ethertype Unknown (0x876e), length 342: 
	0x0000:  ffff ffff 4350 5548 5800 0000 0000 0000  ....CPUHX.......
	0x0010:  0000 0000 0200 0060 006f 0000 0000 0000  .......`.o......
	0x0020:  0000 0000 0000 0000 e000 0000 0000 0000  ................
	0x0030:  0000 0000 0000 0000 0000 0000 0000 0000  ................
	0x0040:  0000 0070 001a 0001 0000 0000 <mark style="background: #E3E6E8; margin: 0 -0.15em; padding: 0.1em 0.15em; border-radius: 0.2em; -webkit-box-decoration-break: clone; box-decoration-break: clone;">4500 0064</mark>  ...p........E..d
	0x0050:  <mark style="background: #E3E6E8; margin: 0 -0.15em; padding: 0.1em 0.15em; border-radius: 0.2em; -webkit-box-decoration-break: clone; box-decoration-break: clone;">009b 0000 ff01 73aa c0a8 6301 c0a8 6301</mark>  ......s...c...c.
	0x0060:  0800 07c0 c6b8 009b abcd abcd abcd abcd  ................
	0x0070:  abcd abcd abcd abcd abcd abcd abcd abcd  ................
	0x0080:  abcd abcd abcd abcd abcd abcd abcd abcd  ................
	0x0090:  abcd abcd abcd abcd abcd abcd abcd abcd  ................
	0x00a0:  abcd abcd abcd abcd abcd abcd abcd abcd  ................
	0x00b0:  fd01 10df 0000 0038 0000 0098 0000 006c  .......8.......l
	0x00c0:  0000 000c 0000 4000 e0f9 020a 647f 0000  ......@.....d...
	0x00d0:  0000 0001 0000 0000 0000 0000 0000 0000  ................
	0x00e0:  0000 0000 0000 0000 0000 0000 006b 0000  .............k..
	0x00f0:  0000 0000 0200 0060 6000 0000 e000 0000  .......``.......
	0x0100:  0000 0000 0000 0008 0000 001c 0000 001c  ................
	0x0110:  c0a8 6301 0000 0010 0000 0000 00ff 0100  ..c.............
	0x0120:  0000 0000 0000 0000 0000 0000 0000 0000  ................
	0x0130:  0000 0000 0000 0000 0000 0000 0000 0000  ................
	0x0140:  0000 0000 0000 0000                      ........
20:58:06.244265 4e:41:50:00:01:01 (oui Unknown) > 4e:41:50:00:10:01 (oui Unknown), ethertype Unknown (0x876e), length 178: 
	0x0000:  ffff ffff 4350 5548 3000 0000 0000 0000  ....CPUH0.......
	0x0010:  0001 0000 0000 0000 006f 0000 0200 0060  .........o.....`
	0x0020:  0000 0000 0000 0000 e000 0000 0000 0000  ................
	0x0030:  0000 0000 0000 0000 0000 0000 0000 0000  ................
	0x0040:  <mark style="background: #E3E6E8; margin: 0 -0.15em; padding: 0.1em 0.15em; border-radius: 0.2em; -webkit-box-decoration-break: clone; box-decoration-break: clone;">4500 0064 009b 0000 ff01 73aa c0a8 6301</mark>  E..d......s...c.
	0x0050:  <mark style="background: #E3E6E8; margin: 0 -0.15em; padding: 0.1em 0.15em; border-radius: 0.2em; -webkit-box-decoration-break: clone; box-decoration-break: clone;">c0a8 6301</mark> 0000 0fc0 c6b8 009b abcd abcd  ..c.............
	0x0060:  abcd abcd abcd abcd abcd abcd abcd abcd  ................
	0x0070:  abcd abcd abcd abcd abcd abcd abcd abcd  ................
	0x0080:  abcd abcd abcd abcd abcd abcd abcd abcd  ................
	0x0090:  abcd abcd abcd abcd abcd abcd abcd abcd  ................
	0x00a0:  abcd abcd                                ....</code></pre></div>

Unfortunately tcpdump couldn't display those punted packets in an easy-to-read format, due to the presence of additional IOS-XR system headers.

To overcome this, we will dump full packet, including all headers, in hexadecimal format, by using `-xx` flag, and then use hex patterns to search through the result. The full packet dump will include Ethernet headers, and therefore the `0x0000` address will represent the beginning of the frame.

<div class="highlighter-rouge">
<pre class="highlight">
<code>[xr-vm_node0_RP0_CPU0:~]$<b>tcpdump <mark style="background: #E1D1FF; margin: 0 -0.15em; padding: 0.1em 0.15em; border-radius: 0.2em; -webkit-box-decoration-break: clone; box-decoration-break: clone;">-xxi</mark> <mark style="background: #A9E0FF; margin: 0 -0.15em; padding: 0.1em 0.15em; border-radius: 0.2em; -webkit-box-decoration-break: clone; box-decoration-break: clone;">ps-inb.1538</mark></b>
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ps-inb.1538, link-type EN10MB (Ethernet), capture size 262144 bytes
20:58:32.248474 4e:41:50:00:10:01 (oui Unknown) > 4e:41:50:00:01:01 (oui Unknown), ethertype Unknown (0x876e), length 342: 
	0x0000:  4e41 5000 0101 4e41 5000 1001 876e ffff
	0x0010:  ffff 4350 5548 5800 0000 0000 0000 0000
	0x0020:  0000 0200 0060 006f 0000 0000 0000 0000
	0x0030:  0000 0000 0000 e000 0000 0000 0000 0000
	0x0040:  0000 0000 0000 0000 0000 0000 0000 0000
	0x0050:  0070 001a 0001 0000 0000 <mark style="background: #E3E6E8; margin: 0 -0.15em; padding: 0.1em 0.15em; border-radius: 0.2em; -webkit-box-decoration-break: clone; box-decoration-break: clone;">4500 0064 00a8</mark>
	0x0060:  <mark style="background: #E3E6E8; margin: 0 -0.15em; padding: 0.1em 0.15em; border-radius: 0.2em; -webkit-box-decoration-break: clone; box-decoration-break: clone;">0000 ff01 739d c0a8 6301 c0a8 6301</mark> 0800
	0x0070:  07b3 c6b8 00a8 abcd abcd abcd abcd abcd
	0x0080:  abcd abcd abcd abcd abcd abcd abcd abcd
	0x0090:  abcd abcd abcd abcd abcd abcd abcd abcd
	0x00a0:  abcd abcd abcd abcd abcd abcd abcd abcd
	0x00b0:  abcd abcd abcd abcd abcd abcd abcd fd01
	0x00c0:  10df 0000 0038 0000 0098 0000 006c 0000
	0x00d0:  000c 0000 4000 e0f9 020a 647f 0000 0000
	0x00e0:  0001 0000 0000 0000 0000 0000 0000 0000
	0x00f0:  0000 0000 0000 0000 0000 006b 0000 0000
	0x0100:  0000 0200 0060 6000 0000 e000 0000 0000
	0x0110:  0000 0000 0008 0000 001c 0000 001c c0a8
	0x0120:  6301 0000 0010 0000 0000 00ff 0100 0000
	0x0130:  0000 0000 0000 0000 0000 0000 0000 0000
	0x0140:  0000 0000 0000 0000 0000 0000 0000 0000
	0x0150:  0000 0000 0000
20:58:32.249461 4e:41:50:00:01:01 (oui Unknown) > 4e:41:50:00:10:01 (oui Unknown), ethertype Unknown (0x876e), length 178: 
	0x0000:  4e41 5000 1001 4e41 5000 0101 876e ffff
	0x0010:  ffff 4350 5548 3000 0000 0000 0000 0001
	0x0020:  0000 0000 0000 006f 0000 0200 0060 0000
	0x0030:  0000 0000 0000 e000 0000 0000 0000 0000
	0x0040:  0000 0000 0000 0000 0000 0000 0000 <mark style="background: #E3E6E8; margin: 0 -0.15em; padding: 0.1em 0.15em; border-radius: 0.2em; -webkit-box-decoration-break: clone; box-decoration-break: clone;">4500</mark>
	0x0050:  <mark style="background: #E3E6E8; margin: 0 -0.15em; padding: 0.1em 0.15em; border-radius: 0.2em; -webkit-box-decoration-break: clone; box-decoration-break: clone;">0064 00a8 0000 ff01 739d c0a8 6301 c0a8</mark>
	0x0060:  <mark style="background: #E3E6E8; margin: 0 -0.15em; padding: 0.1em 0.15em; border-radius: 0.2em; -webkit-box-decoration-break: clone; box-decoration-break: clone;">6301</mark> 0000 0fb3 c6b8 00a8 abcd abcd abcd
	0x0070:  abcd abcd abcd abcd abcd abcd abcd abcd
	0x0080:  abcd abcd abcd abcd abcd abcd abcd abcd
	0x0090:  abcd abcd abcd abcd abcd abcd abcd abcd
	0x00a0:  abcd abcd abcd abcd abcd abcd abcd abcd
	0x00b0:  abcd</code></pre></div>

## Video
This entire presentation is also available as part of my video series published in the Cisco TAC YouTube playlist.

<a href="http://www.youtube.com/watch?feature=player_embedded&v=n-JwMVsage4
" target="_blank"><img src="http://img.youtube.com/vi/n-JwMVsage4/0.jpg"
alt="IMAGE ALT TEXT HERE" width="480" height="360" border="3" /></a>

## Conclusion
This wraps up part 1. Its goal was to provide an introduction to control plane troubleshooting techniques. In the part 2, I'll show you a few simple and advanced packet filtering techniques and how to export packets in pcap format for later analysis.
