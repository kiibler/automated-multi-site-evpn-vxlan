## Site 1 VNIs

```text
dc1-site1-bgw160# show nve vni
Interface VNI      Multicast-group   State Mode Type [BD/VRF]      Flags
--------- -------- ----------------- ----- ---- ------------------ -----
nve1      10010    UnicastBGP        Up    CP   L2 [10]            MS-IR 
nve1      10011    UnicastBGP        Up    CP   L2 [11]            MS-IR 
nve1      10100    n/a               Up    CP   L3 [TENANT-A]      
```

## Site 1 Designated Forwarders

```text
dc1-site1-bgw160# show nve ethernet-segment 
  ES State: Up 
  NVE Interface: nve1 
   NVE State: Up 
   Host Learning Mode: control-plane
  Active Vlans: 1,10-11,100 
   DF Vlans: 10,100 
   Active VNIs: 10010-10011,10100  
  Number of ES members: 2 
  My ordinal: 0
  DF List: 10.1.11.160 10.1.11.161
```

```text
dc1-site1-bgw161# show nve ethernet-segment 
  ES State: Up 
  NVE Interface: nve1 
   NVE State: Up 
   Host Learning Mode: control-plane
  Active Vlans: 1,10-11,100 
   DF Vlans: 1,11 
   Active VNIs: 10010-10011,10100  
  Number of ES members: 2 
  My ordinal: 1 
  DF List: 10.1.11.160 10.1.11.161  
```

## Site 1 NVE peers

```text
dc1-site1-bgw160# show nve peers
Interface Peer-IP                                 State LearnType Uptime   Router-Mac       
--------- --------------------------------------  ----- --------- -------- -----------------
nve1      10.1.11.100                             Up    CP        1d12h    0045.e710.1b08   
nve1      10.1.11.101                             Up    CP        1d12h    0045.ce10.1b08   
nve1      10.1.11.161                             Up    CP        1d12h    n/a              
nve1      10.1.12.162                             Up    CP        1d13h    n/a              
nve1      10.1.12.163                             Up    CP        1d13h    n/a              
nve1      10.100.12.162                           Up    CP        1d12h    n/a              
nve1      10.100.12.163                           Up    CP        1d12h    0200.0a64.0ca3   
```

## Site 1 multisite links

```text
dc1-site1-bgw160# show nve multisite fabric-links 
Interface      State 
---------      ----- 
Ethernet1/1    Up   
Ethernet1/2    Up   

dc1-site1-bgw160# show nve multisite dci-links 
Interface      State 
---------      ----- 
Ethernet1/3    Up   
Ethernet1/4    Up
```

## Site 1 eBGP peers to DC Core

```text
dc1-site1-bgw160# show bgp ipv4 unicast summary
Neighbor        V    AS    MsgRcvd    MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.160.190.2    4 65010       2352       2338       47    0    0 00:35:32 13       
10.160.191.2    4 65010       2353       2339       47    0    0 00:35:26 13
```

## Site 1 iBGP and eBGP peers

```text
dc1-site1-bgw160# show bgp l2vpn evpn summary
Neighbor        V    AS    MsgRcvd    MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.0.10.190     4 65010       2785       2417     1985    0    0 00:18:35 16       
10.0.10.191     4 65010       2792       2419     1985    0    0 00:18:36 16    
10.0.11.130     4 65011       2628       2326     1985    0    0    1d13h 17    
10.0.11.131     4 65011       2621       2326     1985    0    0    1d13h 17    

Neighbor        T    AS PfxRcd     Type-2     Type-3     Type-4     Type-5    
10.0.10.190     E 65010 16         12         4          0          0         
10.0.10.191     E 65010 16         12         4          0          0         
10.0.11.130     I 65011 17         10         6          1          0         
10.0.11.131     I 65011 17         10         6          1          0  
```

## Site 1 VNI 10100 EVPN Routes (`Type 2`)

```text
dc1-site1-bgw160# show bgp l2vpn evpn vni-id 10100
Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.0.11.160:4    (L3VNI 10100)
*>i[2]:[0]:[0]:[48]:[623c.4a6f.6861]:[32]:[172.16.1.113]/272
                      10.1.11.101                       100          0 i
*>e[2]:[0]:[0]:[48]:[8aa3.37a1.6028]:[32]:[172.16.1.117]/272
                      10.100.12.163                                  0 65010 65012 i
*>i[2]:[0]:[0]:[48]:[de03.5731.7a59]:[32]:[172.16.0.112]/272
                      10.1.11.101                       100          0 i
*>e[2]:[0]:[0]:[48]:[e62e.1408.b7c1]:[32]:[172.16.0.114]/272
                      10.100.12.163                                  0 65010 65012 i
*>i[2]:[0]:[0]:[48]:[ea3a.b604.d231]:[32]:[172.16.1.111]/272
                      10.1.11.100                       100          0 i
*>i[2]:[0]:[0]:[48]:[fe62.5026.feb9]:[32]:[172.16.0.110]/272
                      10.1.11.100                       100          0 i
```

## Site 1 L2VNIs mac address-tables

```text
dc1-site1-bgw160# show mac address-table vni 10010
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
C   10     de03.5731.7a59   dynamic  NA         F      F    nve1(10.1.11.101)
C   10     e62e.1408.b7c1   dynamic  NA         F      F    nve1(10.100.12.163)
C   10     fe62.5026.feb9   dynamic  NA         F      F    nve1(10.1.11.100)

dc1-site1-bgw160# show mac address-table vni 10011
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
C   11     623c.4a6f.6861   dynamic  NA         F      F    nve1(10.1.11.101)
C   11     8aa3.37a1.6028   dynamic  NA         F      F    nve1(10.100.12.163)
C   11     ea3a.b604.d231   dynamic  NA         F      F    nve1(10.1.11.100)
```

## Site 1 Ip routing table for TENANT-A

```text
dc1-site1-bgw160# show ip route bgp vrf TENANT-A 
172.16.0.110/32, ubest/mbest: 1/0
    *via 10.1.11.100%default, [200/0], 1d13h, bgp-65011, internal, tag 65011, se
gid: 10100 tunnelid: 0xa010b64 encap: VXLAN
 
172.16.0.112/32, ubest/mbest: 1/0
    *via 10.1.11.101%default, [200/0], 1d13h, bgp-65011, internal, tag 65011, se
gid: 10100 tunnelid: 0xa010b65 encap: VXLAN
 
172.16.0.114/32, ubest/mbest: 1/0
    *via 10.100.12.163%default, [20/0], 00:28:30, bgp-65011, external, tag 65010
, segid: 10100 tunnelid: 0xa640ca3 encap: VXLAN
 
172.16.1.111/32, ubest/mbest: 1/0
    *via 10.1.11.100%default, [200/0], 1d13h, bgp-65011, internal, tag 65011, se
gid: 10100 tunnelid: 0xa010b64 encap: VXLAN
 
172.16.1.113/32, ubest/mbest: 1/0
    *via 10.1.11.101%default, [200/0], 1d13h, bgp-65011, internal, tag 65011, se
gid: 10100 tunnelid: 0xa010b65 encap: VXLAN
 
172.16.1.117/32, ubest/mbest: 1/0
    *via 10.100.12.163%default, [20/0], 00:28:30, bgp-65011, external, tag 65010
, segid: 10100 tunnelid: 0xa640ca3 encap: VXLAN
```
