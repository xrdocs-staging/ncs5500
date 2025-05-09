---
published: true
date: '2023-01-09 10:16 -0800'
title: Introducing NCS-57D2-18DD-SYS
author: Tejas Lad
excerpt: Introduction to NCS-57D2
tags:
  - iosxr
  - cisco
  - NCS5500
  - NCS5700
  - metro
  - NCS57-D2-18DD
position: top
---
{% include toc icon="table" title="Introducing NCS-57D2-18DD-SYS" %}

## Introduction

Latest trends in the [metro architecture](https://xrdocs.io/ncs5500/tutorials/simple-scalable-programmable-sustainable-metro-with-cisco-ncs5500-nc5700/) have driven the evolution of our [product portfolio](https://www.cisco.com/c/en/us/products/routers/network-convergence-system-5500-series/index.html) with enhanced software capabilities and compact form-factors. The Cisco NCS 5500/5700 Series offers industry-leading 100 GbE and 400 GbE port density to handle massive traffic growth. It is designed for operational simplicity and to efficiently meet the scaling needs of large enterprises, web, and service providers. With [IOS-XR 7.8.1](https://www.cisco.com/c/en/us/td/docs/iosxr/ncs5500/general/78x/release/notes/b-release-notes-ncs5500-r781.html#Cisco_Concept.dita_f911b3f4-9c37-46b7-b0b4-c151641ff919), we have introduced the latest addition to our portfolio: [NCS-57D2-18DD-SYS](https://www.cisco.com/c/en/us/products/collateral/routers/network-convergence-system-5500-series/ncs-57d2-18dd-fixed-chassis-ds.html). In this article, we will deepdive into the features and understand its use cases.

## NCS5500/5700 Fixed Portfolio

Before deep-diving into the NCS-57D2, let us have a quick refresher of the NCS5500/5700 fixed portfolio. 

| Product ID          | Form Factor | Description                                                                                                                                                                                     |
|---------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| NCS-5501(SE)        | 1           | Base: 48x 1/10G SFP + 6x 40/100G QSFP <br><br>Scale : 40x 1/10G SFP + 4x 40/100G QSFP                                                                                                           |
| NCS-55A1-36H(-SE)-S | 1           | 36x 40/100G QSFP                                                                                                                                                                                |
| NCS-55A1-24H        | 1           | 24x 40/100G QSFP                                                                                                                                                                                |
| NCS-55A1-24Q6H-S    | 1           | 24x 1/10G SFP + 24x 1/10/25G SFP<br>+ 6x 40/100G QSFP                                                                                                                                           |
| NCS-55A1-48Q6H      | 1           | 48x 1/10/25G SFP + 6x 40/100G QSFP                                                                                                                                                              |
| NCS-55A2-MOD        | 2           | 24x 1/10G SFP + 16x 1/10/25G SFP<br>+ 2x 400G MPAs <br><br>MPA 4x 100G QSFP<br>MPA 2x 100/200G DWDM CFP2<br>MPA 2x 100G QSFP + 1x 100/200G DWDM CFP2<br>MPA 12x 1/10G SFP                       |
| NCS-57B1-6D24/5DSE  | 1           | Base: 24x 100G QSFPDD + 6x400G QSFPDD <br> <br>Scale:24x 100G QSFPDD + 5x400G QSFPDD                                                                                                            |
| NCS-57C1-48Q6D-S    | 1           | 32x SFP28 + 16x SFP56 +<br>2x QSFP-DD(4x100G) + 4x QSFP-DD (400G)                                                                                                                               |
| NCS-57C3-MOD(S)-SYS | 3           | 48x 1/10/25G + 8x or 4x port 40/100G + <br><br>MPAs:<br>2x400G or 4x 100G MPA<br>MPA 4x 100G QSFP<br>MPA 2x 100/200G DWDM CFP2<br>MPA 2x 100G QSFP + 1x 100/200G DWDM CFP2<br>MPA 12x 1/10G SFP |
| **NCS-57D2-18DD-S**     | 2           | 66 Ports: 2x 400G QSFP-DD  +<br> <br>                 16x 400GQSFP-DD / 64x 100G QSFP-DD                                                                                                        |

## NCS-57D2 Product Specifications

### Product Design

For NCS-57D2 we have come up with Belly-to-Belly design concept. This helps in better air-flow and cooling and providing the environment to pack in more ZR/ZRP optics in a single chassis. It is designed to have air flow on both sides of PCB for uniform cooling of upper and lower rows to allow for coherent support. Total 16 Quads with 32 cages and 64 QSFP-DD ports. 

![Screenshot 2023-02-15 at 2.18.30 PM.png]({{site.baseurl}}/images/Screenshot 2023-02-15 at 2.18.30 PM.png)

**Front view**

![Screenshot 2023-01-09 at 11.02.11 AM.png]({{site.baseurl}}/images/Screenshot 2023-01-09 at 11.02.11 AM.png)

**Rear view**

![Screenshot 2023-01-09 at 2.34.44 PM.png]({{site.baseurl}}/images/Screenshot 2023-01-09 at 2.34.44 PM.png)

NCS-57D2 is a compact 2RU chassis purpose built for dense 400G aggregation. It offers 7.2 Terabits of 400GE/100GE optimized forwarding capacity, high power efficiency, QSFP-DD optics, deep packet buffering combined with next-generation security, automation, telemetry, segment routing, EVPN, and Equal-Cost Multipathing (ECMP) capabilities making it suitable for high capacity metro applications. The industry-leading Cisco IOS XR pushes the scalability of the hardware even further through the use of flexible [MDB profiles](https://xrdocs.io/ncs5500/tutorials/mdb-ncs5700/) (modular database) that allows ASIC and TCAM resources to be reconfigured as per the network use cases.

### Physical Interfaces

**Fixed ports**

- Integrated 40/100/400 Gigabit Ethernet support
- 18x 400G ports with QSFP-DD
- 66x 100G ports with QSFP-DD
- All ports support QSFP-28 and QSFP+
- Breakout options supported for 10G/25G/100G
- Support for ZR/ZR+ Coherent optics

**Other ports**

![]({{site.baseurl}}/images/Screenshot%202023-01-09%20at%2011.18.26%20AM.png)

- RJ-45 ports for console and management
- 1 Pulse Per Second (PPS) in/out, 10 MHz in/out, Time of Day (ToD)
- Internal Global Navigation Satellite System (GNSS) with antenna port
- 1 USB port

### Physical Specifications

**Dimensions**

- Height: 2RU 3.45” in. (8.76 cm)
- Width: 17.60 in. (43.94 cm)
- Depth: 23.62 in. (59.99 cm)

**Weight**

- 53 lb (24 kg) including fans and PSUs

**Power**

- Typical: 500W without optics
- Power handling of each quad = 43W

Note: Quad is a group of 4 ports. For example ports 0,1,2,3 makes one quad.
{: .notice--info}

## Naming Logic 

![]({{site.baseurl}}/images/Screenshot%202023-01-09%20at%2011.29.23%20AM.png)

NCS-57D2 is built with J2C+ chipset. The PIDs will vary depending on the licencing model used. 

| PID               | Licensing Model                  |
|-------------------|----------------------------------|
| NCS-57D2-18DD-SYS | Flexible Consumption Model (FCM) |
| NCS-57D2-18DD-S  | Perpetual/BAE                    |

Note: The show platform output will display the FCM Model

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:BGL-CB#show platform 
Node              Type                     State                    Config state
--------------------------------------------------------------------------------
0/RP0/CPU0        NCS-57D2-18DD-SYS(Active) IOS XR RUN               NSHUT
0/PM0             PSU2KW-ACPI              OPERATIONAL              NSHUT
0/PM1             PSU2KW-ACPI              OPERATIONAL              NSHUT
0/FT0             NC57-D2-FAN-FW           OPERATIONAL              NSHUT
0/FT1             NC57-D2-FAN-FW           OPERATIONAL              NSHUT
0/FT2             NC57-D2-FAN-FW           OPERATIONAL              NSHUT
0/FT3             NC57-D2-FAN-FW           OPERATIONAL              NSHUT
0/FB0             NC57-D2-FAN-FW           OPERATIONAL              NSHUT
RP/0/RP0/CPU0:BGL-CB#
</code>
</pre>
</div>

## NCS-57D2 Use cases

### High End Aggregation

![Screenshot 2023-01-09 at 12.52.33 PM.png]({{site.baseurl}}/images/Screenshot 2023-01-09 at 12.52.33 PM.png)

As the user applications are becoming more dynamic, requiring differentiated user experiences network resources needs to be optimised in terms of power and real estate. Catering bandwidth demand by deploying huge equipments is really not a smart business model both in terms of OPEX and environment. NCS-57D2 has been custom designed to cater high end aggregation use cases in a 2RU compact form factor. It supports 7.2Tbps of forwarding capacity. It provides 18 ports of high speed 400G or 66 ports of 100G at extremely low power.

### Rethink the architecture

![Screenshot 2023-01-09 at 1.38.11 PM.png]({{site.baseurl}}/images/Screenshot 2023-01-09 at 1.38.11 PM.png)

Cisco's [Converged SDN Transport Solution](https://blogs.cisco.com/sp/routed-optical-networking-its-about-the-architecture), is an architecture that delivers improved operational efficiencies and simplicity. The solution works by merging IP and Optical onto a single layer where all the switching is done at Layer 3. Routers are connected with standardized [400G ZR/ZR+ coherent pluggable optics](https://www.cisco.com/c/en/us/td/docs/iosxr/ncs5500/interfaces/73x/configuration/guide/b-interfaces-hardware-component-cg-ncs5500-73x/m-zr-zrp.html). With a single service layer based upon IP, flexible management tools can leverage [telemetry](https://www.cisco.com/c/en/us/td/docs/iosxr/ncs5500/telemetry/77x/b-telemetry-cg-ncs5500-78x.html) and [model-driven programmability](https://www.cisco.com/c/en/us/td/docs/iosxr/ncs5500/programmability/76x/b-programmability-cg-ncs5500-76x.html) to streamline lifecycle operations.

### End to End Network Slicing using Segment Routing/Segment Routing v6

Latency is one of most important KPIs when it comes to service provider networks. Applications must be capable of reaching the end-user quickly enough to prevent the degradation of the experience. Network slicing and segment routing provide intelligent routing and traffic differentiation required to efficiently support this distributed architecture. With Network Slicing we can have independent networks on the same physical infrastructure. NCS5500/5700 portfolio supports full set of [Segment routing/SRV6](https://www.cisco.com/c/en/us/td/docs/iosxr/ncs5500/segment-routing/76x/b-segment-routing-cg-ncs5500-76x.html) features to help the operators design an efficient and future ready architecture.

### Scale better with 3-layer spine-leaf architecture

![Screenshot 2023-01-11 at 10.34.01 AM.png]({{site.baseurl}}/images/Screenshot 2023-01-11 at 10.34.01 AM.png)

The above design example is a three-tiered spine-leaf topology. Leverage the power of MP-BGP EVPN to distributes the Layer 2 and Layer 3 reachability information for the overlay network. NCS5500/5700 portfolio supports full set of [EVPN features](https://www.cisco.com/c/en/us/td/docs/iosxr/ncs5500/vpn/78x/b-l2vpn-cg-ncs5500-78x/evpn-features.html) to scale the datacenter fabric. The design allows flexibility to run MP-BGP in a multi-POD environment across different AS or within a single AS. EVPN routes can be distributed among PODs through MP-eBGP peering without the need for additional configuration.

### Trustworthy Infrastructure

Build a [trustworthy infrastructure](https://www.cisco.com/site/us/en/products/security/resources.html) with in-built software security of [Cisco IOS-XR](https://www.cisco.com/c/en/us/td/docs/iosxr/ncs5500/security/77x/b-system-security-cg-ncs5500-77x.html) along with hardware security features of NCS5500/5700 portfolio. The NCS5500/5700 supports: 
- Secure boot with Cisco TAM chip
- Anti-counterfeit and Trust Anchor Infrastructure
- Image Signing
- Run-time Defense
- Encrypted Transport
- DDoS Protection 
- Integrity Measurement and Verification Infrastructure

### Other feature support

Along with above major use cases, the NCS-57D2 (and entire NCS5500/5700 portfolio) support full features of 

- Overlay Layer2 services using EVPN and VPLS
- IPSLA support including TWAMP and TWAMP Lite
- Multicast
- BGP features
- Class C timing 
- QoS
- Telemetry

## NCS-57D2 Port Details

### Port Numbering 

![Screenshot 2023-01-09 at 2.14.26 PM.png]({{site.baseurl}}/images/Screenshot 2023-01-09 at 2.14.26 PM.png)

Above figure shows the port numbering of all the 66 ports of NCS-57D2

### 400G

![Screenshot 2023-01-09 at 2.16.03 PM.png]({{site.baseurl}}/images/Screenshot 2023-01-09 at 2.16.03 PM.png)

NCS-57D2 support 1 400G ZR/ZRP/Grey optics per quad. Only P0 in each quad can be configured with 400G i.e ports 0,4,8 .. 60. When ports P0 are configured with 400G, other 3 ports in the quad are disabled. Ports 65 and 66 can be individually configured for 400G

### 100G

![Screenshot 2023-01-09 at 2.16.37 PM.png]({{site.baseurl}}/images/Screenshot 2023-01-09 at 2.16.37 PM.png)

All the 66 ports can be configured with 100G native speeds

### 40G

![Screenshot 2023-01-09 at 2.18.01 PM.png]({{site.baseurl}}/images/Screenshot 2023-01-09 at 2.18.01 PM.png)

All the 66 ports can be configured with 40G native speeds

### Breakout combination 25G

![Screenshot 2023-01-09 at 2.23.50 PM.png]({{site.baseurl}}/images/Screenshot 2023-01-09 at 2.23.50 PM.png)

- 4x25G breakout option is supported only in ports P0 and P3 in each quad.
- If P0 and P3 is configured with the above breakout option, ports P1 and P2 are disabled. 
- 4x25G Breakout with native 100G in a port group is supported. 
- 4x25G Breakout with native 40G in a port group is not supported.
- Port 64 and 65 can both be configured with 4x25G breakout option.
- Above breakout is not supported with 400G in any port. 

### Breakout combination 10G

![Screenshot 2023-01-09 at 2.23.58 PM.png]({{site.baseurl}}/images/Screenshot 2023-01-09 at 2.23.58 PM.png)

- 4x10G breakout option is supported only in ports P0 and P3 in each quad.
- If P0 and P3 is configured with the above breakout option, ports P1 and P2 are disabled.
- 4x10G breakout option plus native 100G/40G in a port group is supported.
- Port 64 and 65 can both be configured with 4x10G breakout option.
- Above breakout is not supported with 400G in any port 


### Breakout combination 25G and 10G

![Screenshot 2023-01-11 at 9.59.46 AM.png]({{site.baseurl}}/images/Screenshot 2023-01-11 at 9.59.46 AM.png)

- 4x25G and 4x10G Breakout cannot co-exist together in a single quad
- 4x25G and 4x10G Breakout can be configured individually on port 64 and 65


## Port Assignment to ASIC core

![Screenshot 2023-01-09 at 2.31.35 PM.png]({{site.baseurl}}/images/Screenshot 2023-01-09 at 2.31.35 PM.png)

The ASIC has 2 cores. The above figure shows the port mapping to all the interfaces. The important thing to highlight from the above output is the default speed of the interfaces when the platform boots up is 100G. We can change the port speed of each port as per the requirements.


## MDB Profiles

The platform will support the below profiles:

- L2MAX
- L3MAX

## NCS-57D2 Video 

<iframe width="1236" height="695" src="https://www.youtube.com/embed/zPpbkILJgew" title=" Introducing the NCS-57D2-18DD" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## References

- [NCS5500/5700 XRdocs](https://xrdocs.io/ncs5500/) 
- [NCS5500/5700 Modular WhitePaper](https://www.cisco.com/c/en/us/products/collateral/routers/network-convergence-system-5500-series/network-convergence-sys-wp.html)
- [NCS5500/5700 Fixed WhitePaper](https://www.cisco.com/c/dam/en/us/products/collateral/routers/network-convergence-system-5500-series/ncs5500-fixed-platform-architecture-white-paper.pdf)
