---
published: true
date: '2024-05-19 22:39 +0530'
title: 'SRv6 on NCS 500/5500: SRv6 QoS'
author: Paban Sarma
position: hidden
excerpt: >-
  QoS Modes for SRv6 Transport on the NCS 5500/500 & 5700 Platforms. Detailed
  review and configuration examples various configuration options across
  releases.
tags:
  - iosxr
  - SRv6
  - NCS 5500
  - NCS 5700
---
## Overview
In our previous tutorials in this series, we covered various aspects on SRv6 transport implementaion on the NCS 500/5500/5700 series platforms. In this tutorial, we will cover another important aspects, i.e. QoS propagation for SRv6 transport on the NCS 500 and NCS 5500 series platforms. The following figure shows a typical SRv6 encapsulated traffic and from the same it is evident that managing core quality of service for an SRv6 transport network is as simple as managing IPv6 QoS. The simplest way would be to manage the same, by use of the IPv6 DSCP or Precedence field in the SRv6 encapsulation field. 

![QoS-Filed-SRv6-Header]({{site.baseurl}}/images/srv6-qos-filed.png)


## SRv6 QoS Options/Modes

The following table summerizes the qos modes available. 

| Mode # | Ingress Policy-map | L2VPN | VPNv4 | VPNv6 |
|:-----: | :-----------------:|:-----:|:-----:|:------:|
|1: Deafult | NA				| TC ==**0** | TC == **0**| TC ==**0** |
|2: Propagation| NA	| IPv6 Prec == PCP | IPv6 DSCP = IPv4 DSCP | IPv6 TC == IPv6 TC |
|3: Precedence| _set qos-group_ **X** | IPv6 Prec == **X** |IPv6 Prec == **X** |IPv6 Prec == **X** |
|4: DSCP| _set ip encapsulation class-of-service_ **X** | IPv6 DSCP == **X** |IPv6 DSCP == **X** |IPv6 DSCP == **X** |

###  1: Default
Coming to platform implemenation on the NCS 500/5500 and 5700 series routers, by default the QoS fields in the SRv6 header are not set. 

![default-qos-mode]({{site.baseurl}}/images/default.png)

  
### 2: Propagation Mode
In this mode QoS bits from the actual payload is propagated to the imposed SRv6 header QoS field in the following manner:

- L2VPN : PCP bits of the l2 vlan header are copied to IPv6 Prec filed of the SRv6 header
- VPNv4 : DSCP bits of the IPv4 header are copied to IPv6 DSCP filed of SRv6 header
- VPNv6 : DSCP (TC) bits of the IPv6 header copied to IPv6 DSCP (TC) filed of SRv6 header

![propagate-mode]({{site.baseurl}}/images/propagate.png)


### 3: Ingress Policy Map for IPv6 precedence
In this mode we can apply an ingress policy-maps on the UNI  i.e the interface where customer traffic is entering the ingress PE. The use of `set qos-group <0-7>` within the classes of the policy-map sets the IPv6 Precedence corresponding to the qos-group value.

This Mode is available from IOS XR 7.7.x. The below policy-map for example will set IPv6 precedence values as 7, 5 and 1 respectively for the traffic matching PRIO, DATA and default classes while egressing out of the Core interface. 

```
policy-map srv6-qos-group
 class PRIO
  set qos-group 7
 ! 
 class DATA
  set qos-group 5
 ! 
 class class-default
  set qos-group 1
 ! 
 end-policy-map
! 
end
```


![]({{site.baseurl}}images/pmap.png)

### 4: Ingress Policy Map for IPv6 DSCP
In this mode we can apply an ingress policy-maps on the UNI  i.e the interface where customer traffic is entering the ingress PE. There is a new modular qos CLI (MQC) introduced to use The use of `set ip encapsulation class-of-service <0-63>` within the classes of the policy-map sets the IPv6 DSCP values corresponding to the policy-map. This modes bring in more granularity to the QoS options within the SRv6 Core.

This Mode is available from IOS XR 24.2.x . The below policy-map for example will set IPv6 DSCP values as 56 (cs7), 40 (cs5) and 8(cs1) respectively for the traffic matching PRIO, DATA and default classes while egressing out of the Core interface. 


```
policy-map srv6-ip-encap
 class PRIO
  set ip encapsulation class-of-service 56
 ! 
 class DATA
  set ip encapsulation class-of-service 40
 ! 
 class class-default
  set ip encapsulation class-of-service 8
 ! 
 end-policy-map
```


![dscp-via-ip-encap]({{site.baseurl}}images/pmap-extend.png)


## Configurations on NCS 5500/500 Systems
Configurations on NCS 5500/500 system as discussed in our previous articles are done via hw-module profiles. 
### Default Mode
By daefult, there is no traffic-class related configuration needed. Only the basic hw-module profile to enable SRv6 needs to configured.

```
hw-module profile segment-routing srv6 mode micro-segment format f3216
```

### Propagation Mode
The hardware module profile is appended with traffic-class propagate option. 
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
Along with the hardware module prfile an ingress policy-map marking _qos-group_ values must be applied to the UNI interface. In this article we will refer to the sample policy-map documented in previous section. 

```
interface TenG 0/0/0/1
 service-policy input srv6-qos-group
 . . . 
 . . .
```
### Ingress Policy Map for IPv6 DSCP

```
hw-module profile segment-routing srv6 mode micro-segment format f3216
 encapsulation
   traffic-class policy-map-extend    
```
Along with the hardware module prfile an ingress policy-map marking _qos-group_ values must be applied to the UNI interface. In this article we will refer to the sample policy-map documented in previous section. 

```
interface TenG 0/0/0/1
 service-policy input srv6-ip-encap
 . . . 
 . . .
```

## Configurations for NCS 5700 Systems

NCS 5700 platforms don't need hw-mdoule profile. QoS encapsulations options are configured under global "segment-routing srv6" configurations. NCS 5700 also gives more flexibility in terms of opting for policy-map based mode and propagation modes on services selectively. 
### Default Mode
No additional config is needed. 
 
### Propagation Mode
Propagation mode on these platforms is configured under global segment routing configuration. 

```
segment-routing
 srv6
  encapsulation
   traffic-class propagate
  !
```
### Ingress Policy Map for IPv6 precedence
For this mode, we must have the propagation mode enabled under global segment-routing configuration.  Once, that is in place, we can apply the policy-map on ingress to set IPv6 prec on SRv6 header.
```
segment-routing
 srv6
  encapsulation
   traffic-class propagate
  !
 ```
 
 ```
interface TenG 0/0/0/1
 service-policy input srv6-qos-group
 . . . 
 . . .
```


### Ingress Policy Map for IPv6 DSCP

For this mode, we must have the propagation mode enabled under global segment-routing configuration.  Once, that is in place, we can apply the policy-map on ingress to set IPv6 DSCP on SRv6 header.
```
segment-routing
 srv6
  encapsulation
   traffic-class propagate
  !
 ```
 
 ```
interface TenG 0/0/0/1
 service-policy input srv6-ip-encap
 . . . 
 . . .
```
## Restrictions & Best Practices

Few Points to Note :

- Setting of _qos-group_ and _ip encapsulation class-of-service_ is not supproted together on both ncs 5500/500 and 5700 platforms
- on the NCS 5500 & 500 Platform _set ip encapsulation class-of-service_ is allowed when the specific hardware module profile is applied. 
- Change in the srv6 hardware module profile needs a reload of the router.

## Conclusion
In this article we discussed the different qos modes available for SRv6 transport  on the NCS 5500/5700 platform  as of the latest IOS XR 24.2.1 release. While propagation mode is the simplest way to maintain QoS with respect to payload QoS. Use of IPv6 precedence or DSCP via the policy-map options (either qos-group or _ip encapsulation class-of-service_) gives better control and granularity over the QoS options for different services within the same PE.
