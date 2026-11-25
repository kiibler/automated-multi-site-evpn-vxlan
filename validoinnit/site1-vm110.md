## Tenant A VM 110 Ip address

```text
site1-pve1-tenant-a-web-vm110:~# ip addr show dev eth0
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether fe:62:50:26:fe:b9 brd ff:ff:ff:ff:ff:ff
    inet 172.16.0.110/24 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::fc62:50ff:fe26:feb9/64 scope link 
       valid_lft forever preferred_lft forever
```

## Tenant A internal ping

```text
site1-pve1-tenant-a-web-vm110:~# ping 172.16.0.1
PING 172.16.0.1 (172.16.0.1): 56 data bytes
64 bytes from 172.16.0.1: seq=0 ttl=255 time=2.176 ms
^C
--- 172.16.0.1 ping statistics ---
1 packets transmitted, 1 packets received, 0% packet loss

site1-pve1-tenant-a-web-vm110:~# ping 172.16.0.112
PING 172.16.0.112 (172.16.0.112): 56 data bytes
64 bytes from 172.16.0.112: seq=0 ttl=64 time=9.626 ms
^C
--- 172.16.0.112 ping statistics ---
1 packets transmitted, 1 packets received, 0% packet loss

site1-pve1-tenant-a-web-vm110:~# ping 172.16.0.114
PING 172.16.0.114 (172.16.0.114): 56 data bytes
64 bytes from 172.16.0.114: seq=0 ttl=64 time=34.888 ms
^C
--- 172.16.0.114 ping statistics ---
1 packets transmitted, 1 packets received, 0% packet loss

site1-pve1-tenant-a-web-vm110:~# ping 172.16.1.111
PING 172.16.1.111 (172.16.1.111): 56 data bytes
64 bytes from 172.16.1.111: seq=0 ttl=63 time=6.571 ms
^C
--- 172.16.1.111 ping statistics ---
1 packets transmitted, 1 packets received, 0% packet loss

site1-pve1-tenant-a-web-vm110:~# ping 172.16.1.113
PING 172.16.1.113 (172.16.1.113): 56 data bytes
64 bytes from 172.16.1.113: seq=0 ttl=62 time=14.249 ms
^C
--- 172.16.1.113 ping statistics ---
1 packets transmitted, 1 packets received, 0% packet loss

site1-pve1-tenant-a-web-vm110:~# ping 172.16.1.117
PING 172.16.1.117 (172.16.1.117): 56 data bytes
64 bytes from 172.16.1.117: seq=0 ttl=60 time=21.511 ms
^C
--- 172.16.1.117 ping statistics ---
1 packets transmitted, 1 packets received, 0% packet loss
```

## Tenant A external ping

```text
site1-pve1-tenant-a-web-vm110:~# ping 172.16.0.115
PING 172.16.0.115 (172.16.0.115): 56 data bytes
^C
--- 172.16.0.115 ping statistics ---
4 packets transmitted, 0 packets received, 100% packet loss

site1-pve1-tenant-a-web-vm110:~# ping 172.16.0.118
PING 172.16.0.118 (172.16.0.118): 56 data bytes
^C
--- 172.16.0.118 ping statistics ---
4 packets transmitted, 0 packets received, 100% packet loss

site1-pve1-tenant-a-web-vm110:~# ping 172.16.1.116
PING 172.16.1.116 (172.16.1.116): 56 data bytes
^C
--- 172.16.1.116 ping statistics ---
4 packets transmitted, 0 packets received, 100% packet loss

site1-pve1-tenant-a-web-vm110:~# ping 172.16.1.119
PING 172.16.1.119 (172.16.1.119): 56 data bytes
^C
--- 172.16.1.119 ping statistics ---
4 packets transmitted, 0 packets received, 100% packet loss
```
