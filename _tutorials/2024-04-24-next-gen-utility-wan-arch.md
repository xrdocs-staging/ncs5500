---
published: false
date: '2024-04-24 18:23 +0530'
title: next-gen-utility-WAN-arch
---
## Next Generation Utility WAN Architecture

In an era where digital transformation is paramount, utilities are actively modernizing their network infrastructure to harness the evolving advantages of substation automation, with a focus on operational simplicity and scalability. At the forefront of this modernization effort is the next-generation utility wide area network (WAN) architecture, leveraging the new-age transport and services. The simplification provided by this new-age networking paradigm incorporating Cisco's state-of-the-art technologies is tailored to fulfill the growing demands of substation automation. By undertaking this journey, utilities not only enhance their operational efficiency through improved connectivity and performance but also achieve cost savings, thereby establishing a new benchmark for resilient and robust networking within the industry.

The transition from IP MPLS transport to Segment Routing (SR), based on the source routing paradigm, evolves the transport by collapsing multiple layers of technology into the simplified SR with IGP extensions. SR can operate with either an MPLS or an IPv6 forwarding data plane. Currently, utilities are shifting from IP MPLS to SR-MPLS. 

In the realm of services, a significant shift is underway from traditional L2VPN services, which rely on LDP data plane based signaling, to EVPN that leverages MP-BGP control plane signaling. The adoption of EVPN by today's utilities eliminates the redundancy of a separate LDP data plane for layer 2 services and reinforces the move towards simplification. EVPN offers substantial benefits to operators, including a wide range of multihoming and load balancing capabilities, superior scalability, and robust loop avoidance mechanisms. 

Broadly, utilities’ services are classified into three categories ([Next generation digital substation WAN](https://blogs.cisco.com/industrial-iot/next-generation-digital-substation-wan?ccid=cc002185&oid=pstit032047)):

- Layer 3 Substation to Datacenter: IP based supervisory control and data acquisition (SCADA) data, IP based CCTV, enterprise data and IP telephony

- Layer 2 Substation to Substation for Ethernet based Teleprotection: Layer 2 multicast protocols (IEC61850 GOOSE & SV), Virtual machine migrations (for virtualized applications) and third-party SCADA traffic

- Layer 2 Substation to Substation for Traditional Teleprotection: Power Protection point-to-point services using specific utility protocols over non-Ethernet connections

