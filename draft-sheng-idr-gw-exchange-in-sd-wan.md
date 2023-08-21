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

To interconnect geographically faraway branches or SASE resources, multi-segment SD-WAN is often deployed via cloud backbone{{!I-D.draft-dmk-rtgwg-multisegment-sdwan}}. As shown in {{Scenario}}, CPE 1 and CPE 2 are connected to the nearest Cloud GW in different regions. For the traffic from CPE 1 to CPE 2, the CPE 1 will encapsulate the address of the egress GW(GW3) and destination CPE(CPE2) in the dataplane(see {{Section 5.2 of I-D.draft-dmk-rtgwg-multisegment-sdwan}}). To accomplish that, CPE needs to know their peers' corresponding GW addresses. This document proposes an control plane enhancement based on {{!I-D.draft-ietf-idr-sdwan-edge-discovery}} to exchange the corresponding GW address between CPEs.

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

# Corresponding GW Sub-TLV

Corresponding GW Sub-TLV within the Hybrid Underlay Tunnel UPDATE indicates the address of the corresponding GW.
The following is the structure of the Corresponding GW Sub-TLV:

~~~
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |  Type=TBD2 (C-GW  subTLV)     |  Length (2 Octets)            |
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
