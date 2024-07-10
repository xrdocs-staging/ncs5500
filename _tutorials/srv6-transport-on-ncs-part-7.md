---
published: false
date: '2024-05-19 22:39 +0530'
title: 'SRv6 on NCS 500/5500: SRv6 QoS'
author: Paban Sarma
position: hidden
---
## Overview
In our previous tutorials in this series, we covered various aspects on SRv6 transport implementaion on the NCS 500/5500/5700 series platforms. In this tutorial, we will cover another important aspects, i.e. QoS propagation for SRv6 transport on the NCS 500 and NCS 5500 series platforms. The following figure shows a typical SRv6 encapsulated traffic and from the same it is evident that managing core quality of service for an SRv6 transport network is as simple as managing IPv6 QoS. The simplest way would be to manage the same, by use of the IPv6 DSCP or Precedence field in the SRv6 encapsulation field. 

<placeholder for different field in SRv6 packet>>

## SRv6 QoS Options/Modes

### Default
Coming to platform implemenation on the NCS 500/5500 and 5700 series routers, by default the QoS fields in the SRv6 header are not set. 
### Propagation Mode
In this mode QoS bits from the actual payload is propagated to the imposed SRv6 header QoS field in the following manner:

- L2VPN : PCP bits of the l2 vlan header are copied to IPv6 Prec filed of the SRv6 header
- VPNv4 : DSCP bits of the IPv4 header are copied to IPv6 DSCP filed of SRv6 header
- VPNv6 : DSCP (TC) bits of the IPv6 header copied to IPv6 DSCP (TC) filed of SRv6 header

### Ingress Policy Map for IPv6 precedence
In this mode we can apply an ingress policy-maps on the UNI  i.e the interface where customer traffic is entering the ingress PE. The use of `set qos-group <0-7>` within the classes of the policy-map sets the IPv6 Precedence corresponding to the qos-group value.

This Mode is available from IOS XR 7.7.x

### Ingress Policy Map for IPv6 DSCP
In this mode we can apply an ingress policy-maps on the UNI  i.e the interface where customer traffic is entering the ingress PE. There is a new modular qos CLI (MQC) introduced to use The use of `set ip encapsulation class-of-service <0-63>` within the classes of the policy-map sets the IPv6 DSCP values corresponding to the policy-map. This modes bring in more granularity to the QoS options within the SRv6 Core.

This Mode is available from IOS XR 24.2.x

## Configurations on NCS 5500/500 Systems
Configurations on NCS 5500/500 system as discussed in our previous articles are done via hw-module profiles. 
### Default Mode

```
hw-module profile segment-routing srv6 mode micro-segment format f3216
```

### Propagation Mode
```
hw-module profile segment-routing srv6 mode micro-segment format f3216 encapsulation traffic-class propagate
```

### Ingress Policy Map for IPv6 precedence
```
hw-module profile segment-routing srv6 mode micro-segment format f3216
 encapsulation
  l2-traffic
   traffic-class propagate
  !
  l3-traffic
   traffic-class policy-map
  !
```

### Ingress Policy Map for IPv6 DSCP


## Configurations for NCS 5700 Systems

NCS 5700 platforms don't need hw-mdoule profile. QoS encapsulations options are configured under global "segment-routing srv6" configurations.
### Default Mode
 
### Propagation Mode

### Ingress Policy Map for IPv6 precedence

### Ingress Policy Map for IPv6 DSCP
## Conclusion
