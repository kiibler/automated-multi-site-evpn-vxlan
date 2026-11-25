## Tenant B VM 118 Ip address

```text
site2-pve3-tenant-b-web-vm118:~# ip addr show dev eth0
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether b6:b8:04:ad:b5:bd brd ff:ff:ff:ff:ff:ff
    inet 172.16.0.118/24 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::b4b8:4ff:fead:b5bd/64 scope link 
       valid_lft forever preferred_lft forever
```

## Tenant B internal ping

```text
site2-pve3-tenant-b-web-vm118:~# ping 172.16.0.1
PING 172.16.0.1 (172.16.0.1): 56 data bytes
64 bytes from 172.16.0.1: seq=0 ttl=255 time=3.220 ms
^C
--- 172.16.0.1 ping statistics ---
1 packets transmitted, 1 packets received, 0% packet loss

site2-pve3-tenant-b-web-vm118:~# ping 172.16.0.115
PING 172.16.0.115 (172.16.0.115): 56 data bytes
64 bytes from 172.16.0.115: seq=0 ttl=64 time=16.823 ms
^C
--- 172.16.0.115 ping statistics ---
1 packets transmitted, 1 packets received, 0% packet loss

site2-pve3-tenant-b-web-vm118:~# ping 172.16.1.119
PING 172.16.1.119 (172.16.1.119): 56 data bytes
64 bytes from 172.16.1.119: seq=0 ttl=63 time=2.856 ms
^C
--- 172.16.1.119 ping statistics ---
1 packets transmitted, 1 packets received, 0% packet loss

site2-pve3-tenant-b-web-vm118:~# ping 172.16.1.116
PING 172.16.1.116 (172.16.1.116): 56 data bytes
64 bytes from 172.16.1.116: seq=0 ttl=62 time=8.771 ms
^C
--- 172.16.1.116 ping statistics ---
1 packets transmitted, 1 packets received, 0% packet loss
```

## Tenant B external ping

```text
site2-pve3-tenant-b-web-vm118:~# ping 172.16.0.110
PING 172.16.0.110 (172.16.0.110): 56 data bytes
^C
--- 172.16.0.110 ping statistics ---
4 packets transmitted, 0 packets received, 100% packet loss

site2-pve3-tenant-b-web-vm118:~# ping 172.16.0.112
PING 172.16.0.112 (172.16.0.112): 56 data bytes
^C
--- 172.16.0.112 ping statistics ---
4 packets transmitted, 0 packets received, 100% packet loss

site2-pve3-tenant-b-web-vm118:~# ping 172.16.0.114
PING 172.16.0.114 (172.16.0.114): 56 data bytes
^C
--- 172.16.0.114 ping statistics ---
4 packets transmitted, 0 packets received, 100% packet loss

site2-pve3-tenant-b-web-vm118:~# ping 172.16.1.111
PING 172.16.1.111 (172.16.1.111): 56 data bytes
^C
--- 172.16.1.111 ping statistics ---
4 packets transmitted, 0 packets received, 100% packet loss

site2-pve3-tenant-b-web-vm118:~# ping 172.16.1.113
PING 172.16.1.113 (172.16.1.113): 56 data bytes
^C
--- 172.16.1.113 ping statistics ---
4 packets transmitted, 0 packets received, 100% packet loss

site2-pve3-tenant-b-web-vm118:~# ping 172.16.1.117
PING 172.16.1.117 (172.16.1.117): 56 data bytes
^C
--- 172.16.1.117 ping statistics ---
4 packets transmitted, 0 packets received, 100% packet loss
```
