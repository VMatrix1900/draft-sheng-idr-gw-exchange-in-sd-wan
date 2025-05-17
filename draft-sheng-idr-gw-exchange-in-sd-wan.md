---
stand_alone: true
category: std
submissionType: IETF
ipr: trust200902
lang: en

title: Associated Gateway Exchange in Multi-segment SD-WAN
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
  country: China
 -
  ins: J. Dong
  name: Jie Dong
  role: editor
  organization: Huawei
  email: jie.dong@huawei.com
  country: China
 -
  ins: L. Dunbar
  name: Linda Dunbar
  organization: Futurewei
  email: linda.dunbar@futurewei.com
 -
  ins: G. Mishra
  name: Gyan Mishra
  organization: Verizon
  email: gyan.s.mishra@verizon.com


--- abstract

The document describes the control plane enhancement for multi-segment SD-WAN to exchange the associated GW information between edges.

--- middle

# Introduction {#intro}

{{!I-D.draft-dmk-rtgwg-multisegment-sdwan}} describes how enterprises leverage Cloud Providers’ backbone infrastructure to interconnect their branch offices. As illustrated in Figure 1, CPE-1 and CPE-2 establish connections to their respective Cloud Gateways (GW) in distinct regions. CPE-1 and CPE-2 maintain the pairwise IPsec Security Associations (SAs). The IPsec encrypted traffic from CPE-1 to CPE-2 is encapsulated by the GENEVE header {{!RFC8926}}, with the outer destination address being the GW1.

{{!I-D.draft-dmk-rtgwg-multisegment-sdwan}} specifies a set of sub-TLVs to convey information about the GWs associated with the destination branches, such as GW3 for CPE-2, along with additional attributes. To accomplish this, CPE-1 must be aware of the associated GW addresses of their peers. This document proposes a BGP extension building upon {{!I-D.draft-ietf-idr-sdwan-edge-discovery}}, enabling a CPE to advertise its directly connected GW address to other CPEs.

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

- Cloud DC: Off-Premises Data Center, managed by 3rd party, that hosts applications, services, and workload for different organizations or tenants.
- CPE: Customer (Edge) Premises Equipment.
- OnPrem: On Premises data centers and branch offices.
- RR: Route Reflector.
- SD-WAN: Software Defined Wide Area Network. In this document, “SD-WAN” refers to a policy-driven transporting of IP packets over multiple underlay networks for better WAN bandwidth management, visibility, and control.
- VPN: Virtual Private Network.

# Extension to SD-WAN Underlay UPDATE for Associated GWs

The Client Routes Update is the same as described in {{Section 5 of I-D.draft-ietf-idr-sdwan-edge-discovery}}.

## NLRI SD-WAN SAFI Route Type For GW

Adding a new attribute (Associated-Gateway Sub-TLV) to the SD-WAN-Hybrid Tunnel Encoding which is included in the SD-WAN SAFI (=74) Underlay Tunnel Update:

~~~
+------------------+
|    Route Type    | 2 octet
+------------------+
|     Length       | 2 Octet
+------------------+
|   Type Specific  |
~ Value (Variable) ~
|                  |
+------------------+
~~~

NLRI Route-Type = 2: For advertising the detailed properties of the transit gateways for the edge. The SD-WAN NLRI Route-Type =2 has the following encoding:

~~~
+------------------+
|  Route Type = 2  | 2 octet
+------------------+
|     Length       | 2 Octet
+------------------+
|   SD-WAN Color   | 4 octets
+------------------+
|  SD-WAN-Node-ID  | 4 or 16 octets
+------------------+
~~~

SD-WAN-Color: To represent a group of tunnels that correlate with the Color-Extended-community included in a client route UPDATE. When multiple SD-WAN edges can reach a client route co-located at one site, the SD-WAN- Color can represent a group of tunnels terminated at those SD-WAN edges co-located at the site, which effectively represents the site.

SD-WAN Node ID: The node's IPv4 or IPv6 address.

## Associated GW Sub-TLV

The Associated GW Sub-TLV, within the SD-WAN-Hybrid Tunnel TLV (code point 25), carries the associated GW address(es) with which the SD-WAN edge is associated.

The following is the structure of the associated GW Sub-TLV:

~~~
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |               Type=TBD    |    length     |  Priority         |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   | Associated GW Addr Family     | Address                       |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+ (variable)                    +
   ~                                                               ~
   |             Associated GW Address                             |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

Priority (1-255) indicate the preference of the GW. The higher the value, the more preference is the GW.

Priority = 0 represents that the connection exists but request Cloud Backbone not to choose the GW if possible.

# Manageability Considerations

Effective management of SD-WAN edge nodes and the exchange of associated cloud gateway information are critical aspects in ensuring a robust and scalable SD-WAN deployment.

# Security Considerations

This document does not introduce any new security considerations.

# IANA Considerations

Need IANA to assign a new Sub-TLV Type under the SD-WAN-Hybrid Tunnel TLV.

- SD-WAN Associated GW Sub-TLV.

--- back
