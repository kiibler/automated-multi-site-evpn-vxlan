## Site 2 VNIs

```text
dc1-site2-bgw163# show nve vni
Interface VNI      Multicast-group   State Mode Type [BD/VRF]      Flags
--------- -------- ----------------- ----- ---- ------------------ -----
nve1      10010    UnicastBGP        Up    CP   L2 [10]            MS-IR 
nve1      10011    UnicastBGP        Up    CP   L2 [11]            MS-IR 
nve1      10020    UnicastBGP        Up    CP   L2 [20]            MS-IR 
nve1      10021    UnicastBGP        Up    CP   L2 [21]            MS-IR 
nve1      10100    n/a               Up    CP   L3 [TENANT-A]           
nve1      10200    n/a               Up    CP   L3 [TENANT-B]
```

## Site 2 Designated Forwarders

```text
dc1-site2-bgw163# show nve ethernet-segment 
  ES State: Up 
  NVE Interface: nve1 
   NVE State: Up 
   Host Learning Mode: control-plane
  Active Vlans: 1,10-11,20-21,100,200 
   DF Vlans: 1,11,21 
   Active VNIs: 10010-10011,10020-10021,10100,10200  
  Number of ES members: 2 
  My ordinal: 1 
  DF List: 10.1.12.162 10.1.12.163  
```

```text
dc1-site2-bgw162# show nve ethernet-segment 
  ES State: Up 
  NVE Interface: nve1 
   NVE State: Up 
   Host Learning Mode: control-plane
  Active Vlans: 1,10-11,20-21,100,200 
   DF Vlans: 10,20,100,200 
   Active VNIs: 10010-10011,10020-10021,10100,10200 
  Number of ES members: 2 
  My ordinal: 0 
  DF List: 10.1.12.162 10.1.12.163
```

## Site 2 NVE peers

```text
dc1-site2-bgw163# show nve peers
Interface Peer-IP                                 State LearnType Uptime   Router-Mac       
--------- --------------------------------------  ----- --------- -------- -----------------
nve1      10.1.11.160                             Up    CP        00:23:14 n/a  
nve1      10.1.11.161                             Up    CP        00:23:21 n/a  
nve1      10.1.12.102                             Up    CP        1d13h    0045.a010.1b08   
nve1      10.1.12.103                             Up    CP        1d13h    0045.7210.1b08   
nve1      10.1.12.162                             Up    CP        1d13h    n/a  
nve1      10.100.11.160                           Up    CP        00:23:14 n/a  
nve1      10.100.11.161                           Up    CP        00:23:21 0200.0a64.0ba1   
nve1      10.100.12.162                           Up    CP        00:23:21 0200.0a64.0ca2   
```

## Site 2 multisite links

```text
dc1-site2-bgw163# show nve multisite fabric-links 
Interface      State 
---------      ----- 
Ethernet1/1    Up   
Ethernet1/2    Up   
```

```text
dc1-site2-bgw163# show nve multisite dci-links 
Interface      State 
---------      ----- 
Ethernet1/3    Up   
Ethernet1/4    Up  
```

## Site 2 eBGP peers to DC Core

```text
dc1-site2-bgw163# show bgp ipv4 unicast summary
Neighbor        V    AS    MsgRcvd    MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.163.190.2    4 65010       2359       2345       52    0    0 00:42:01 13       
10.163.191.2    4 65010       2359       2345       52    0    0 00:41:53 13    
```

## Site 2 iBGP and eBGP peers

```text
dc1-site2-bgw163# show bgp l2vpn evpn summary
Neighbor        V    AS    MsgRcvd    MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.0.10.190     4 65010       2642       2497     3673    0    0 00:24:44 20    
10.0.10.191     4 65010       2639       2497     3673    0    0 00:24:45 20    
10.0.12.132     4 65012       2843       2328     3673    0    0    1d13h 40    
10.0.12.133     4 65012       2843       2328     3673    0    0    1d13h 40    
    
Neighbor        T    AS PfxRcd     Type-2     Type-3     Type-4     Type-5    
10.0.10.190     E 65010 20         16         4          0          0         
10.0.10.191     E 65010 20         16         4          0          0         
10.0.12.132     I 65012 40         27         12         1          0         
10.0.12.133     I 65012 40         27         12         1          0  
```

## Site 2 VNI 10100 EVPN Routes (`Type 2`)

```text
dc1-site2-bgw163# show bgp l2vpn evpn vni-id 10100
   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.0.12.163:4    (L3VNI 10100)
* i[2]:[0]:[0]:[48]:[623c.4a6f.6861]:[32]:[172.16.1.113]/272
                      10.100.12.162                     100          0 65010 65011 i
*>e                   10.100.11.161                                  0 65010 65011 i
*>i[2]:[0]:[0]:[48]:[8aa3.37a1.6028]:[32]:[172.16.1.117]/272
                      10.1.12.103                       100          0 i
* i[2]:[0]:[0]:[48]:[de03.5731.7a59]:[32]:[172.16.0.112]/272
                      10.100.12.162                     100          0 65010 65011 i
*>e                   10.100.11.161                                  0 65010 65011 i
*>i[2]:[0]:[0]:[48]:[e62e.1408.b7c1]:[32]:[172.16.0.114]/272
                      10.1.12.102                       100          0 i
* i[2]:[0]:[0]:[48]:[ea3a.b604.d231]:[32]:[172.16.1.111]/272
                      10.100.12.162                     100          0 65010 65011 i
*>e                   10.100.11.161                                  0 65010 65011 i
* i[2]:[0]:[0]:[48]:[fe62.5026.feb9]:[32]:[172.16.0.110]/272
                      10.100.12.162                     100          0 65010 65011 i
*>e                   10.100.11.161                                  0 65010 65011 i
```

## Site 2 VNI 10200 EVPN Routes (`Type 2`)

```text
dc1-site2-bgw163# show bgp l2vpn evpn vni-id 10200
   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.0.12.163:5    (L3VNI 10200)
*>i[2]:[0]:[0]:[48]:[ae45.ed22.19dc]:[32]:[172.16.0.115]/272
                      10.1.12.102                       100          0 i
*>i[2]:[0]:[0]:[48]:[b6b8.04ad.b5bd]:[32]:[172.16.0.118]/272
                      10.1.12.103                       100          0 i
*>i[2]:[0]:[0]:[48]:[c2c5.7392.68dc]:[32]:[172.16.1.119]/272
                      10.1.12.103                       100          0 i
*>i[2]:[0]:[0]:[48]:[ca3f.728d.1545]:[32]:[172.16.1.116]/272
                      10.1.12.102                       100          0 i
```

## Site 2 L2VNIs mac address-tables

```text
dc1-site2-bgw163# show mac address-table vni 10010
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
C   10     de03.5731.7a59   dynamic  NA         F      F    nve1(10.100.11.161)
C   10     e62e.1408.b7c1   dynamic  NA         F      F    nve1(10.1.12.102)
C   10     fe62.5026.feb9   dynamic  NA         F      F    nve1(10.100.11.161)
```

```text
dc1-site2-bgw163# show mac address-table vni 10011
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
C   11     623c.4a6f.6861   dynamic  NA         F      F    nve1(10.100.11.161)
C   11     8aa3.37a1.6028   dynamic  NA         F      F    nve1(10.1.12.103)
C   11     ea3a.b604.d231   dynamic  NA         F      F    nve1(10.100.11.161)
```

```text
dc1-site2-bgw163# show mac address-table vni 10020
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
C   20     ae45.ed22.19dc   dynamic  NA         F      F    nve1(10.1.12.102)
C   20     b6b8.04ad.b5bd   dynamic  NA         F      F    nve1(10.1.12.103)
```

```text
dc1-site2-bgw163# show mac address-table vni 10021
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
C   21     c2c5.7392.68dc   dynamic  NA         F      F    nve1(10.1.12.103)
C   21     ca3f.728d.1545   dynamic  NA         F      F    nve1(10.1.12.102)
```

## Site 2 Ip routing table for TENANT-A

```text
dc1-site2-bgw163# show ip route bgp vrf TENANT-A
172.16.0.110/32, ubest/mbest: 1/0
    *via 10.100.11.161%default, [20/0], 00:32:13, bgp-65012, external, tag 65010
, segid: 10100 tunnelid: 0xa640ba1 encap: VXLAN
 
172.16.0.112/32, ubest/mbest: 1/0
    *via 10.100.11.161%default, [20/0], 00:32:13, bgp-65012, external, tag 65010
, segid: 10100 tunnelid: 0xa640ba1 encap: VXLAN
 
172.16.0.114/32, ubest/mbest: 1/0
    *via 10.1.12.102%default, [200/0], 1d13h, bgp-65012, internal, tag 65012, se
gid: 10100 tunnelid: 0xa010c66 encap: VXLAN
 
172.16.1.111/32, ubest/mbest: 1/0
    *via 10.100.11.161%default, [20/0], 00:32:13, bgp-65012, external, tag 65010
, segid: 10100 tunnelid: 0xa640ba1 encap: VXLAN
 
172.16.1.113/32, ubest/mbest: 1/0
    *via 10.100.11.161%default, [20/0], 00:32:13, bgp-65012, external, tag 65010
, segid: 10100 tunnelid: 0xa640ba1 encap: VXLAN
 
172.16.1.117/32, ubest/mbest: 1/0
    *via 10.1.12.103%default, [200/0], 1d13h, bgp-65012, internal, tag 65012, se
gid: 10100 tunnelid: 0xa010c67 encap: VXLAN
```

## Site 2 Ip routing table for TENANT-B

```text
dc1-site2-bgw163# show ip route bgp vrf TENANT-B
172.16.0.115/32, ubest/mbest: 1/0
    *via 10.1.12.102%default, [200/0], 1d13h, bgp-65012, internal, tag 65012, se
gid: 10200 tunnelid: 0xa010c66 encap: VXLAN
 
172.16.0.118/32, ubest/mbest: 1/0
    *via 10.1.12.103%default, [200/0], 00:01:06, bgp-65012, internal, tag 65012,
 segid: 10200 tunnelid: 0xa010c67 encap: VXLAN
 
172.16.1.116/32, ubest/mbest: 1/0
    *via 10.1.12.102%default, [200/0], 1d13h, bgp-65012, internal, tag 65012, se
gid: 10200 tunnelid: 0xa010c66 encap: VXLAN
 
172.16.1.119/32, ubest/mbest: 1/0
    *via 10.1.12.103%default, [200/0], 1d13h, bgp-65012, internal, tag 65012, se
gid: 10200 tunnelid: 0xa010c67 encap: VXLAN
```
