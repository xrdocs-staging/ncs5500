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
![full](/images/eXR-tcpdump/BGP-1.png){: .full}
1. First, the packet has been successfully received by our network interface.

