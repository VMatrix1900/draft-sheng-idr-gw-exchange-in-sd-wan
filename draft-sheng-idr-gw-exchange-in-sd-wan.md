---
stand_alone: true
category: std
submissionType: IETF
ipr: trust200902
lang: en

title: Corresponding Gateway Exchange in Multi-segment SD-WAN
abbrev: Gateway Info Exchange in SDWAN
docname: draft-sheng-idr-gw-exchange-in-sd-wan-latest
obsoletes:
updates:
# date: 2022-02-02 -- date is filled in automatically by xml2rfc if not given

area: "Routing"
workgroup: "Inter-Domain Routing"

kw:
  - Multi-segment SD-WAN

author:
 -
  ins: C. Sheng
  name: Cheng Sheng
  organization: Huawei
  street: Beiqing Road
  city: Beijing
  email: shengcheng@huawei.com
 -
  ins: H. Shi
  name: Hang Shi
  organization: Huawei
  email: shihang9@huawei.com
  role: editor
  street: Beiqing Road
  city: Beijing
  country: China
 -
  ins: L. Dunbar
  name: Linda Dunbar
  organization: Futurewei
  email: linda.dunbar@futurewei.com


--- abstract

The document describes the control plane enhancement for multi-segment SD-WAN to exchange the corresponding GW information between edges.

--- middle

# Introduction {#intro}

{{!I-D.draft-dmk-rtgwg-multisegment-sdwan}} describes the enterprises utilizing Cloud Providers’ backbone to interconnect their branch offices. As shown in Figure 1, CPE-1 and CPE-2 are connected to their respective Cloud Gateways (GW) in different regions. CPE-1 and CPE-2 maintain the pairwise IPsec SAs. The IPsec encrypted traffic from CPE-1 to CPE-2 is encapsulated by the GENEVE header {{!RFC8926}}, with the outer destination address being the GW1. {{!I-D.draft-dmk-rtgwg-multisegment-sdwan}} specifies a set of sub-TLVs to carry the information of the GWs of the destination branches, e.g., GW3 for CPE-2 and other attributes.

To accomplish that, CPE-1 needs to know their peers' corresponding GW addresses. This document proposes a BGP extension based on {{!I-D.draft-ietf-idr-sdwan-edge-discovery}} for a CPE to advertise its directly connected GW address to other CPEs.


~~~
  (^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^)
  (            Region2             )
  (            +-----+             )
  (            | GW2 |             )
  (            +-----+             )
  (           /       \            )
  (          /  Cloud  \           )
  (         /  Backbone \          )
  ( Region1/             \Region3  )
  ( +-----+               +-----+  )
  ( | GW1 |---------------| GW3 |  )
  ( +--+--+               +--+--+  )
  (^^^^|^^^^^^^^^^^^^^^^^^^^^|^^^^^)
       |                     |
    +--+--+               +--+--+
    |CPE 1|               |CPE 2|
    +-----+               +-----+
~~~
{: #Scenario  title="Multi-segment SD-WAN"}


# Requirements Language

{::boilerplate bcp14-tagged}

The following acronyms and terms are used in this document:

- Cloud DC: 	Off-Premises Data Center, managed by 3rd party, that hosts applications, services, and workload for different organizations or tenants.
- CPE:        Customer (Edge) Premises Equipment.
- OnPrem:		On Premises data centers and branch offices.
- RR          Route Reflector.
- SD-WAN: 		Software Defined Wide Area Network. In this document, “SD-WAN” refers to a policy-driven transporting of IP packets over multiple underlay networks for better WAN bandwidth management, visibility, and control.
- VPN         Virtual Private Network.

# Extension to SD-WAN Underlay UPDATE for Corresponding GWs

The Client Routes Update is the same as described in {{Section 5 of I-D.draft-ietf-idr-sdwan-edge-discovery}}.

## Corresponding GW Sub-TLV

The Corresponding GW Sub-TLV, within the SD-WAN-Hybrid Tunnel TLV (code point 25), carries the corresponding GW address(es) with which the SD-WAN edge is associated.

The following is the structure of the Corresponding GW Sub-TLV:

~~~
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |   Type=TBD    |    length     |  reserved     |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   | Corresponding GW Addr Family  | Address                       |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+ (variable)                    +
   ~                                                               ~
   |             Corresponding GW Address                          |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

# Security Considerations

This document does not introduce any new security considerations.

# IANA Considerations

TBD.

--- back
