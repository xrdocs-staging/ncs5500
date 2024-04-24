---
published: true
date: '2024-04-24 18:23 +0530'
title: Next Generation Utility WAN Architecture
author: Ananya Bose
position: hidden
---
## Overview

In an era where digital transformation is paramount, utilities are actively modernizing their network infrastructure to harness the evolving advantages of substation automation, with a focus on operational simplicity and scalability. At the forefront of this modernization effort is the next-generation utility wide area network (WAN) architecture, leveraging the new-age transport and services. The simplification provided by this new-age networking paradigm incorporating Cisco's state-of-the-art technologies is tailored to fulfill the growing demands of substation automation. By undertaking this journey, utilities not only enhance their operational efficiency through improved connectivity and performance but also achieve cost savings, thereby establishing a new benchmark for resilient and robust networking within the industry.

The transition from IP MPLS transport to Segment Routing (SR), based on the source routing paradigm, evolves the transport by collapsing multiple layers of technology into the simplified SR with IGP extensions. SR can operate with either an MPLS or an IPv6 forwarding data plane. Currently, utilities are shifting from IP MPLS to SR-MPLS. 

In the realm of services, a significant shift is underway from traditional L2VPN services, which rely on LDP data plane based signaling, to EVPN that leverages MP-BGP control plane signaling. The adoption of EVPN by today's utilities eliminates the redundancy of a separate LDP data plane for layer 2 services and reinforces the move towards simplification. EVPN offers substantial benefits to operators, including a wide range of multihoming and load balancing capabilities, superior scalability, and robust loop avoidance mechanisms. 

Broadly, utilities’ services are classified into three categories:

- Layer 3 Substation to Datacenter: IP based supervisory control and data acquisition (SCADA) data, IP based CCTV, enterprise data and IP telephony

- Layer 2 Substation to Substation for Ethernet based Teleprotection: Layer 2 multicast protocols (IEC61850 GOOSE & SV), Virtual machine migrations (for virtualized applications) and third-party SCADA traffic

- Layer 2 Substation to Substation for Traditional Teleprotection: Power Protection point-to-point services using specific utility protocols over non-Ethernet connections

As the first phase of network transformation, L3 services are now delivered over SR-MPLS based transport, connecting the substation to the Datacenter. eBGP peering is implemented between Cisco’s IR8340 router as CE and Cisco’s NCS540 router as PE at either site. Considering that the core capacity requirement of utilities is not expected to surpass 50G in the near future, the low-density compact NCS540 from the Cisco NCS portfolio is the transport platform of choice for the core infrastructure. For utilities demanding sub-50ms convergence for any Layer 3 service, SR's Topology-Independent Loop-Free Alternate Fast Reroute (TI-LFA FRR) capabilities can be activated. 

## Layer 2 Teleprotection

A key requirement of utility WAN transport networks is path predictability for latency sensitive applications such as Teleprotection. To this end, utilities require the ability to co-route traffic over a bidirectional and defined path in the network, minimizing the asymmetry delay for both the active and protection paths. This requirement mirrors the behavior of traditional synchronous optical network (SONET) and synchronous digital hierarchy (SDH) networks that operated with unidirectional path switched ring (UPSR) and subnetwork connection protection (SNC-P). 

In traditional IP MPLS based networks, Flex LSP is leveraged to meet utilities’ Teleprotection requirements. Flex LSP facilitates the establishment of bidirectional label-switched paths (LSPs) dynamically through the Resource Reservation Protocol–Traffic Engineering (RSVP-TE). Call Admission Control (CAC) ensures that the LSP has sufficient bandwidth to accommodate the Layer 2 circuit.

With the evolution in transport, SR now offers path predictability required for utility WAN Layer 2 Teleprotection services by leveraging the latest Circuit-style Segment Routing Traffic Engineering (CS SR-TE) capabilities. CS SR-TE offers bidirectional co-routed path behavior, end-to-end path protection + restoration and strict bandwidth guarantees. CS SR-TE therefore provides deterministic path behavior aligning with utility WAN network requirements.

Latest CS SR-TE enhancements guarantee sub-50 ms switching times in the event of failures, a crucial requirement for utility WAN Teleprotection. Segment Routing Performance Measurement (SR-PM) offers the functionality of path protection via liveness detection for the working and protect paths. Recent innovations in SR-PM enables offload of this path liveness-check functionality from software to hardware, thereby enforcing the strict convergence times, as required for utilities’ Teleprotection applications. 

L2 Teleprotection use cases are delivered over EVPN VPWS point-to-point service between the substation PEs stitched to the underlying CS SR-TE policy, thereby offering circuit-like performance. Herein CE IE9300 is single homed to PE NCS540 at each substation end. Path predictability is therefore guaranteed with a deterministic next hop PE at the remote end, underpinned by CS SR-TE’s co-routed bidirectional path behavior.

_**Layer 2 Teleprotection service with Circuit-style SR-TE**_
![L2 Teleprotection with CS SR-TE - 2.png]({{site.baseurl}}/images/L2 Teleprotection with CS SR-TE - 2.png)



Cisco is partnering with Schweitzer Engineering Laboratories and Valiant Communications to deliver traditional Teleprotection services between substations involving non-Ethernet interfaces. 

SEL’s ICON platform provides support for non-Ethernet interfaces e.g. T1/E1, C37.94, RS-232 Async, E&M signalling, etc., required for utilities’ specific substation protection components to deliver Teleprotection services. ICON simultaneously provides Ethernet uplink interfacing to Cisco’s transport platform NCS540. NCS540 PEs form a ring interconnecting substations across the core, with an EVPN-VPWS services steered to an SR-TE policy between back-to-back PEs. The critical operational technology (OT) traffic for Teleprotection is then carried over the dedicated point-to-point paths in a single VLAN. The ring architecture ensures network resilience with fast convergence to the backup path, provided by a proprietary mechanism on SEL ICON.

_**NCS540 + SEL ICON solution for non-Ethernet based Teleprotection**_
![NCS540 + SEL - 2.png]({{site.baseurl}}/images/NCS540 + SEL - 2.png)



Cisco has also validated solutions to deliver Teleprotection in collaboration with Valiant Communications, specifically IEEE C37.94 Line Differential Protection and Distance Protection solutions over SR-MPLS Transport. Here again, the vendor device supports non-Ethernet connections within the substation and interfaces to NCS540 over Ethernet. VCL-2711 is the IEEE C37.94 over IP/MPLS transmission equipment that provides Line Differential Protection, and VCL-TP is the Distance Protection over IP/MPLS transmission equipment that provides support for 8 Binary Commands. The architecture relies on the fast convergence offered by the underlying SR Transport. For example, CS SR-TE can be implemented to enforce bidirectional co-routed path behaviour, with an EVPN-VPWS service stitched to the SR-TE policy. 

_**NCS540 + VCL-2711 C37.94 Line Differential solution over SR-MPLS**_
![VCL-2711-solution.png]({{site.baseurl}}/images/VCL-2711-solution.png)


_**NCS540 + VCL-TP Distance Protection solution over SR-MPLS**_
![VCL-TP-solution.png]({{site.baseurl}}/images/VCL-TP-solution.png)

SEL/Valiant’s expertise in utilities’ protection and automation equipment, combined with Cisco’s technology leadership in transport offers a comprehensive solution to address non-Ethernet based Teleprotection.

## References

- [Next generation digital substation WAN](https://blogs.cisco.com/industrial-iot/next-generation-digital-substation-wan?ccid=cc002185&oid=pstit032047)
- [Design Guide](https://www.cisco.com/c/dam/en/us/td/docs/solutions/Verticals/Utilities/SA/3-1/SA-3-1-DG.pdf?dtid=odicdc000509)
- [Solution Brief](https://www.cisco.com/c/dam/en/us/td/docs/solutions/Verticals/Utilities/WAN/WAN-Utility-SA.pdf?dtid=odicdc000509)
