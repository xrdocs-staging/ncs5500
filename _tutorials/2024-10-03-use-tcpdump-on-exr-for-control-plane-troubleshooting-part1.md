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
![BGP-1.png]({{site.baseurl}}/images/eXR-tcpdump/BGP-1.png)
- First, the packet has been successfully received by our network interface.
{: .text-left}
![BGP-2.png]({{site.baseurl}}/images/eXR-tcpdump/BGP-2.png)  
- Second, the packet has reached the BGP process level.
{: .text-left}
  
  
If debugs fail to show traces of our packets, it would prompt several questions:  
  
  
![BGP-3.png]({{site.baseurl}}/images/eXR-tcpdump/BGP-3.png)  
- Did the BGP packets reach our process level ?
{: .text-left}
![BGP-4.png]({{site.baseurl}}/images/eXR-tcpdump/BGP-4.png)  
- If not, where and why were they dropped ?
{: .text-left}
![BGP-5.png]({{site.baseurl}}/images/eXR-tcpdump/BGP-5.png)  
- Most importantly, did these drops occurred inside the device, or before reaching our network interface ?
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

To start with, any packet handled by the device local CPU is click called **"for us"**. Next, let’s take a look at how these "for us" packets reach our process level. The incoming network interface traffic is always a combination of transit packets and some locally destined control plane packets.