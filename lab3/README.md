# Lab3
## Схема:
![scheme](../lab1/attachments/Pasted_image_20251230093126.png)
## План работ:
- Схему и адресацию используем из лабы 1, также настраиваем лупбеки для последующего анонсирования.
- Используем одну зону на один под.
- Указываем Network Entity Title по алгоритму:
  49.0001.LOOPBACK_ADDR.00
- Настраиваем интерфейсы
	- isis enable UNDERLAY
	- isis network point-to-point
	- На лупбеках добавляем isis passive

## Таблица лупбеков
- Spine1 - 172.16.2.1/32
- Spine2 - 172.16.2.2/32
- Leaf1 - 172.16.1.1/32
- Leaf2 - 172.16.1.2/32
- Leaf3 - 172.16.1.3/32

## Пример конфигурации
#### Spine1
```
...
interface Ethernet1  
  isis enable UNDERLAY  
  isis network point-to-point  
interface Ethernet2  
  isis enable UNDERLAY  
  isis network point-to-point  
interface Ethernet3  
  isis enable UNDERLAY  
  isis network point-to-point  
interface Loopback0  
  isis enable UNDERLAY  
router isis UNDERLAY  
  net 49.0001.1720.1600.2001.00  
  is-type level-1  
  log-adjacency-changes  
  !  
  address-family ipv4 unicast
```
#### Leaf1
```
...
interface Ethernet1  
  isis enable UNDERLAY  
  isis network point-to-point  
interface Ethernet2  
  isis enable UNDERLAY  
  isis network point-to-point  
interface Loopback0  
  isis enable UNDERLAY  
router isis UNDERLAY  
  net 49.0001.1720.1600.1001.00  
  is-type level-1  
  log-adjacency-changes  
  !  
  address-family ipv4 unicast
```

## Результаты после конвергенции ISIS level1

#### Leaf1:
```
leaf1#sh isis summary  
   
IS-IS Instance: UNDERLAY VRF: default  
 Instance ID: 0  
 System ID: 1720.1600.1001, administratively enabled  
 Router ID: IPv4: 10.1.2.1  
 Hostname: leaf1  
 Multi Topology disabled, not attached  
 IPv4 Preference: Level 1: 115, Level 2: 115  
 IPv6 Preference: Level 1: 115, Level 2: 115  
 IS-Type: Level 1, Number active interfaces: 3  
 Routes IPv4 only  
 LSP size maximum: Level 1: 1492, Level 2: 1492  
 Interval Type           Max Wait Initial Wait Hold Interval  
 ----------------------- -------- ------------ -------------  
 LSP generation interval      5 s        50 ms         50 ms  
 SPF interval             2000 ms      1000 ms       1000 ms  
 Current SPF hold interval(ms): Level 1: 1000, Level 2: 0  
 Last Level 1 SPF run 21 seconds ago  
 CSNP generation interval: 10 seconds  
 Dynamic Flooding: Disabled  
 Authentication mode: Level 1: None, Level 2: None  
 Graceful Restart: Disabled, Graceful Restart Helper: Enabled  
 Area addresses: 49.0001  
 level 1: number DIS interfaces: 0, LSDB size: 5  
   Area Leader: None  
   Overload Bit is not set.    
 Redistributed Level 1 routes: 0 limit: Not Configured  
 Redistributed Level 2 routes: 0 limit: Not Configured  

leaf1#sh isis neighbors    
   
Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id             
UNDERLAY  default  spine1           L1   Ethernet1          P2P               UP    29          1D                     
UNDERLAY  default  spine2           L1   Ethernet2          P2P               UP    24          23                     
```
#### Leaf2:
```
leaf2#sh isis summary  
   
IS-IS Instance: UNDERLAY VRF: default  
 Instance ID: 0  
 System ID: 1720.1600.1002, administratively enabled  
 Router ID: IPv4: 10.1.2.5  
 Hostname: leaf2  
 Multi Topology disabled, not attached  
 IPv4 Preference: Level 1: 115, Level 2: 115  
 IPv6 Preference: Level 1: 115, Level 2: 115  
 IS-Type: Level 1, Number active interfaces: 3  
 Routes IPv4 only  
 LSP size maximum: Level 1: 1492, Level 2: 1492  
 Interval Type           Max Wait Initial Wait Hold Interval  
 ----------------------- -------- ------------ -------------  
 LSP generation interval      5 s        50 ms         50 ms  
 SPF interval             2000 ms      1000 ms       1000 ms  
 Current SPF hold interval(ms): Level 1: 1000, Level 2: 0  
 Last Level 1 SPF run 5:04 minutes ago  
 CSNP generation interval: 10 seconds  
 Dynamic Flooding: Disabled  
 Authentication mode: Level 1: None, Level 2: None  
 Graceful Restart: Disabled, Graceful Restart Helper: Enabled  
 Area addresses: 49.0001  
 level 1: number DIS interfaces: 0, LSDB size: 5  
   Area Leader: None  
   Overload Bit is not set.    
 Redistributed Level 1 routes: 0 limit: Not Configured  
 Redistributed Level 2 routes: 0 limit: Not Configured  

leaf2#sh isis neighbors  
   
Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id             
UNDERLAY  default  spine1           L1   Ethernet1          P2P               UP    24          18                     
UNDERLAY  default  spine2           L1   Ethernet2          P2P               UP    22          1F                     
```
#### Leaf3:
```
leaf3#sh isis summary  
   
IS-IS Instance: UNDERLAY VRF: default  
 Instance ID: 0  
 System ID: 1720.1600.1003, administratively enabled  
 Router ID: IPv4: 10.1.2.9  
 Hostname: leaf3  
 Multi Topology disabled, not attached  
 IPv4 Preference: Level 1: 115, Level 2: 115  
 IPv6 Preference: Level 1: 115, Level 2: 115  
 IS-Type: Level 1, Number active interfaces: 3  
 Routes IPv4 only  
 LSP size maximum: Level 1: 1492, Level 2: 1492  
 Interval Type           Max Wait Initial Wait Hold Interval  
 ----------------------- -------- ------------ -------------  
 LSP generation interval      5 s        50 ms         50 ms  
 SPF interval             2000 ms      1000 ms       1000 ms  
 Current SPF hold interval(ms): Level 1: 1000, Level 2: 0  
 Last Level 1 SPF run 9:33 minutes ago  
 CSNP generation interval: 10 seconds  
 Dynamic Flooding: Disabled  
 Authentication mode: Level 1: None, Level 2: None  
 Graceful Restart: Disabled, Graceful Restart Helper: Enabled  
 Area addresses: 49.0001  
 level 1: number DIS interfaces: 0, LSDB size: 5  
   Area Leader: None  
   Overload Bit is not set.    
 Redistributed Level 1 routes: 0 limit: Not Configured  
 Redistributed Level 2 routes: 0 limit: Not Configured  

leaf3#sh isis neighbors  
   
Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id             
UNDERLAY  default  spine1           L1   Ethernet1          P2P               UP    27          1A                     
UNDERLAY  default  spine2           L1   Ethernet2          P2P               UP    29          22                     
```
#### Spine1:
```
spine1#sh isis summary  
   
IS-IS Instance: UNDERLAY VRF: default  
 Instance ID: 0  
 System ID: 1720.1600.2001, administratively enabled  
 Router ID: IPv4: 172.16.1.2  
 Hostname: spine1  
 Multi Topology disabled, not attached  
 IPv4 Preference: Level 1: 115, Level 2: 115  
 IPv6 Preference: Level 1: 115, Level 2: 115  
 IS-Type: Level 1, Number active interfaces: 4  
 Routes IPv4 only  
 LSP size maximum: Level 1: 1492, Level 2: 1492  
 Interval Type           Max Wait Initial Wait Hold Interval  
 ----------------------- -------- ------------ -------------  
 LSP generation interval      5 s        50 ms         50 ms  
 SPF interval             2000 ms      1000 ms       1000 ms  
 Current SPF hold interval(ms): Level 1: 1000, Level 2: 0  
 Last Level 1 SPF run 13:11 minutes ago  
 CSNP generation interval: 10 seconds  
 Dynamic Flooding: Disabled  
 Authentication mode: Level 1: None, Level 2: None  
 Graceful Restart: Disabled, Graceful Restart Helper: Enabled  
 Area addresses: 49.0001  
 level 1: number DIS interfaces: 0, LSDB size: 5  
   Area Leader: None  
   Overload Bit is not set.    
 Redistributed Level 1 routes: 0 limit: Not Configured  
 Redistributed Level 2 routes: 0 limit: Not Configured  

spine1#sh isis neighbors  
   
Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id             
UNDERLAY  default  leaf1            L1   Ethernet1          P2P               UP    24          1F                     
UNDERLAY  default  leaf2            L1   Ethernet2          P2P               UP    25          19                     
UNDERLAY  default  leaf3            L1   Ethernet3          P2P               UP    23          20                     
```
#### Spine2:
```
spine2#sh isis summary  
   
IS-IS Instance: UNDERLAY VRF: default  
 Instance ID: 0  
 System ID: 1720.1600.2002, administratively enabled  
 Router ID: IPv4: 10.1.2.10  
 Hostname: spine2  
 Multi Topology disabled, not attached  
 IPv4 Preference: Level 1: 115, Level 2: 115  
 IPv6 Preference: Level 1: 115, Level 2: 115  
 IS-Type: Level 1, Number active interfaces: 4  
 Routes IPv4 only  
 LSP size maximum: Level 1: 1492, Level 2: 1492  
 Interval Type           Max Wait Initial Wait Hold Interval  
 ----------------------- -------- ------------ -------------  
 LSP generation interval      5 s        50 ms         50 ms  
 SPF interval             2000 ms      1000 ms       1000 ms  
 Current SPF hold interval(ms): Level 1: 1000, Level 2: 0  
 Last Level 1 SPF run 11:15 minutes ago  
 CSNP generation interval: 10 seconds  
 Dynamic Flooding: Disabled  
 Authentication mode: Level 1: None, Level 2: None  
 Graceful Restart: Disabled, Graceful Restart Helper: Enabled  
 Area addresses: 49.0001  
 level 1: number DIS interfaces: 0, LSDB size: 5  
   Area Leader: None  
   Overload Bit is not set.    
 Redistributed Level 1 routes: 0 limit: Not Configured  
 Redistributed Level 2 routes: 0 limit: Not Configured  

spine2#sh isis neighbors  
   
Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id             
UNDERLAY  default  leaf1            L1   Ethernet1          P2P               UP    22          1E                     
UNDERLAY  default  leaf2            L1   Ethernet2          P2P               UP    26          21                     
UNDERLAY  default  leaf3            L1   Ethernet3          P2P               UP    26          1B                     
```


## Таблицы маршрутизации

#### Leaf1:
```
leaf1#sh ip route  
  
VRF: default  
Source Codes:  
      C - connected, S - static, K - kernel,  
      O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,  
      E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,  
      N2 - OSPF NSSA external type2, B - Other BGP Routes,  
      B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,  
      I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,  
      A O - OSPF Summary, NG - Nexthop Group Static Route,  
      V - VXLAN Control Service, M - Martian,  
      DH - DHCP client installed default route,  
      DP - Dynamic Policy Route, L - VRF Leaked,  
      G  - gRIBI, RC - Route Cache Route,  
      CL - CBF Leaked Route  
  
Gateway of last resort is not set  
  
C        10.1.1.0/30  
          directly connected, Ethernet1  
I L1     10.1.1.4/30 [115/20]  
          via 10.1.1.2, Ethernet1  
I L1     10.1.1.8/30 [115/20]  
          via 10.1.1.2, Ethernet1  
C        10.1.2.0/30  
          directly connected, Ethernet2  
I L1     10.1.2.4/30 [115/20]  
          via 10.1.2.2, Ethernet2  
I L1     10.1.2.8/30 [115/20]  
          via 10.1.2.2, Ethernet2  
C        172.16.1.1/32  
          directly connected, Loopback0  
I L1     172.16.1.2/32 [115/30]  
          via 10.1.1.2, Ethernet1  
          via 10.1.2.2, Ethernet2  
I L1     172.16.1.3/32 [115/30]  
          via 10.1.1.2, Ethernet1  
          via 10.1.2.2, Ethernet2  
I L1     172.16.2.1/32 [115/20]  
          via 10.1.1.2, Ethernet1  
I L1     172.16.2.2/32 [115/20]  
          via 10.1.2.2, Ethernet2
```
#### Leaf2:
```
leaf2#sh ip route  
  
VRF: default  
Source Codes:  
      C - connected, S - static, K - kernel,  
      O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,  
      E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,  
      N2 - OSPF NSSA external type2, B - Other BGP Routes,  
      B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,  
      I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,  
      A O - OSPF Summary, NG - Nexthop Group Static Route,  
      V - VXLAN Control Service, M - Martian,  
      DH - DHCP client installed default route,  
      DP - Dynamic Policy Route, L - VRF Leaked,  
      G  - gRIBI, RC - Route Cache Route,  
      CL - CBF Leaked Route  
  
Gateway of last resort is not set  
  
I L1     10.1.1.0/30 [115/20]  
          via 10.1.1.6, Ethernet1  
C        10.1.1.4/30  
          directly connected, Ethernet1  
I L1     10.1.1.8/30 [115/20]  
          via 10.1.1.6, Ethernet1  
I L1     10.1.2.0/30 [115/20]  
          via 10.1.2.6, Ethernet2  
C        10.1.2.4/30  
          directly connected, Ethernet2  
I L1     10.1.2.8/30 [115/20]  
          via 10.1.2.6, Ethernet2  
I L1     172.16.1.1/32 [115/30]  
          via 10.1.1.6, Ethernet1  
          via 10.1.2.6, Ethernet2  
C        172.16.1.2/32  
          directly connected, Loopback0  
I L1     172.16.1.3/32 [115/30]  
          via 10.1.1.6, Ethernet1  
          via 10.1.2.6, Ethernet2  
I L1     172.16.2.1/32 [115/20]  
          via 10.1.1.6, Ethernet1  
I L1     172.16.2.2/32 [115/20]  
          via 10.1.2.6, Ethernet2
```
#### Leaf3:
```
leaf3#sh ip route  
  
VRF: default  
Source Codes:  
      C - connected, S - static, K - kernel,  
      O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,  
      E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,  
      N2 - OSPF NSSA external type2, B - Other BGP Routes,  
      B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,  
      I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,  
      A O - OSPF Summary, NG - Nexthop Group Static Route,  
      V - VXLAN Control Service, M - Martian,  
      DH - DHCP client installed default route,  
      DP - Dynamic Policy Route, L - VRF Leaked,  
      G  - gRIBI, RC - Route Cache Route,  
      CL - CBF Leaked Route  
  
Gateway of last resort is not set  
  
I L1     10.1.1.0/30 [115/20]  
          via 10.1.1.10, Ethernet1  
I L1     10.1.1.4/30 [115/20]  
          via 10.1.1.10, Ethernet1  
C        10.1.1.8/30  
          directly connected, Ethernet1  
I L1     10.1.2.0/30 [115/20]  
          via 10.1.2.10, Ethernet2  
I L1     10.1.2.4/30 [115/20]  
          via 10.1.2.10, Ethernet2  
C        10.1.2.8/30  
          directly connected, Ethernet2  
I L1     172.16.1.1/32 [115/30]  
          via 10.1.1.10, Ethernet1  
          via 10.1.2.10, Ethernet2  
I L1     172.16.1.2/32 [115/30]  
          via 10.1.1.10, Ethernet1  
          via 10.1.2.10, Ethernet2  
C        172.16.1.3/32  
          directly connected, Loopback0  
I L1     172.16.2.1/32 [115/20]  
          via 10.1.1.10, Ethernet1  
I L1     172.16.2.2/32 [115/20]  
          via 10.1.2.10, Ethernet2
```
#### Spine1:
```
spine1#sh ip route  
  
VRF: default  
Source Codes:  
      C - connected, S - static, K - kernel,  
      O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,  
      E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,  
      N2 - OSPF NSSA external type2, B - Other BGP Routes,  
      B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,  
      I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,  
      A O - OSPF Summary, NG - Nexthop Group Static Route,  
      V - VXLAN Control Service, M - Martian,  
      DH - DHCP client installed default route,  
      DP - Dynamic Policy Route, L - VRF Leaked,  
      G  - gRIBI, RC - Route Cache Route,  
      CL - CBF Leaked Route  
  
Gateway of last resort is not set  
  
C        10.1.1.0/30  
          directly connected, Ethernet1  
C        10.1.1.4/30  
          directly connected, Ethernet2  
C        10.1.1.8/30  
          directly connected, Ethernet3  
I L1     10.1.2.0/30 [115/20]  
          via 10.1.1.1, Ethernet1  
I L1     10.1.2.4/30 [115/20]  
          via 10.1.1.5, Ethernet2  
I L1     10.1.2.8/30 [115/20]  
          via 10.1.1.9, Ethernet3  
I L1     172.16.1.1/32 [115/20]  
          via 10.1.1.1, Ethernet1  
I L1     172.16.1.2/32 [115/20]  
          via 10.1.1.5, Ethernet2  
I L1     172.16.1.3/32 [115/20]  
          via 10.1.1.9, Ethernet3  
C        172.16.2.1/32  
          directly connected, Loopback0  
I L1     172.16.2.2/32 [115/30]  
          via 10.1.1.1, Ethernet1  
          via 10.1.1.5, Ethernet2  
          via 10.1.1.9, Ethernet3
```
#### Spine2:
```
spine2#sh ip route  
  
VRF: default  
Source Codes:  
      C - connected, S - static, K - kernel,  
      O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,  
      E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,  
      N2 - OSPF NSSA external type2, B - Other BGP Routes,  
      B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,  
      I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,  
      A O - OSPF Summary, NG - Nexthop Group Static Route,  
      V - VXLAN Control Service, M - Martian,  
      DH - DHCP client installed default route,  
      DP - Dynamic Policy Route, L - VRF Leaked,  
      G  - gRIBI, RC - Route Cache Route,  
      CL - CBF Leaked Route  
  
Gateway of last resort is not set  
  
I L1     10.1.1.0/30 [115/20]  
          via 10.1.2.1, Ethernet1  
I L1     10.1.1.4/30 [115/20]  
          via 10.1.2.5, Ethernet2  
I L1     10.1.1.8/30 [115/20]  
          via 10.1.2.9, Ethernet3  
C        10.1.2.0/30  
          directly connected, Ethernet1  
C        10.1.2.4/30  
          directly connected, Ethernet2  
C        10.1.2.8/30  
          directly connected, Ethernet3  
I L1     172.16.1.1/32 [115/20]  
          via 10.1.2.1, Ethernet1  
I L1     172.16.1.2/32 [115/20]  
          via 10.1.2.5, Ethernet2  
I L1     172.16.1.3/32 [115/20]  
          via 10.1.2.9, Ethernet3  
I L1     172.16.2.1/32 [115/30]  
          via 10.1.2.1, Ethernet1  
          via 10.1.2.5, Ethernet2  
          via 10.1.2.9, Ethernet3  
C        172.16.2.2/32  
          directly connected, Loopback0
```


## Проверка доступности

#### Leaf1:
```
leaf1#traceroute 172.16.1.2  
traceroute to 172.16.1.2 (172.16.1.2), 30 hops max, 60 byte packets  
1  10.1.1.2 (10.1.1.2)  2.705 ms  2.461 ms  2.344 ms  
2  172.16.1.2 (172.16.1.2)  5.780 ms  6.474 ms  7.301 ms  
leaf1#traceroute 172.16.1.3  
traceroute to 172.16.1.3 (172.16.1.3), 30 hops max, 60 byte packets  
1  10.1.1.2 (10.1.1.2)  2.505 ms  2.400 ms  2.476 ms  
2  172.16.1.3 (172.16.1.3)  8.125 ms  8.015 ms  9.284 ms  
leaf1#traceroute 172.16.2.2  
traceroute to 172.16.2.2 (172.16.2.2), 30 hops max, 60 byte packets  
1  172.16.2.2 (172.16.2.2)  3.023 ms  2.682 ms  2.630 ms  
leaf1#traceroute 172.16.2.1  
traceroute to 172.16.2.1 (172.16.2.1), 30 hops max, 60 byte packets  
1  172.16.2.1 (172.16.2.1)  2.445 ms  2.598 ms  2.452 ms
```
#### Leaf2:
```
leaf2#traceroute 172.16.1.1  
traceroute to 172.16.1.1 (172.16.1.1), 30 hops max, 60 byte packets  
1  10.1.1.6 (10.1.1.6)  3.369 ms  3.028 ms  2.895 ms  
2  172.16.1.1 (172.16.1.1)  8.419 ms  8.374 ms  9.712 ms  
leaf2#traceroute 172.16.1.3  
traceroute to 172.16.1.3 (172.16.1.3), 30 hops max, 60 byte packets  
1  10.1.1.6 (10.1.1.6)  2.733 ms  2.635 ms  2.542 ms  
2  172.16.1.3 (172.16.1.3)  6.347 ms  6.617 ms  8.285 ms  
leaf2#traceroute 172.16.2.2  
traceroute to 172.16.2.2 (172.16.2.2), 30 hops max, 60 byte packets  
1  172.16.2.2 (172.16.2.2)  2.666 ms  2.566 ms  2.651 ms  
leaf2#traceroute 172.16.2.1  
traceroute to 172.16.2.1 (172.16.2.1), 30 hops max, 60 byte packets  
1  172.16.2.1 (172.16.2.1)  3.275 ms  3.025 ms  2.891 ms
```
#### Leaf3:
```
leaf3#traceroute 172.16.1.1  
traceroute to 172.16.1.1 (172.16.1.1), 30 hops max, 60 byte packets  
1  10.1.1.10 (10.1.1.10)  2.903 ms  2.754 ms  2.646 ms  
2  172.16.1.1 (172.16.1.1)  7.411 ms  7.746 ms  8.572 ms  
leaf3#traceroute 172.16.1.2  
traceroute to 172.16.1.2 (172.16.1.2), 30 hops max, 60 byte packets  
1  10.1.1.10 (10.1.1.10)  3.079 ms  2.739 ms  2.615 ms  
2  172.16.1.2 (172.16.1.2)  7.037 ms  7.055 ms  6.914 ms  
leaf3#traceroute 172.16.2.2  
traceroute to 172.16.2.2 (172.16.2.2), 30 hops max, 60 byte packets  
1  172.16.2.2 (172.16.2.2)  3.847 ms  3.788 ms  3.685 ms  
leaf3#traceroute 172.16.2.1  
traceroute to 172.16.2.1 (172.16.2.1), 30 hops max, 60 byte packets  
1  172.16.2.1 (172.16.2.1)  3.102 ms  3.150 ms  3.023 ms
```
#### Spine1:
```
spine1#traceroute 172.16.2.2  
traceroute to 172.16.2.2 (172.16.2.2), 30 hops max, 60 byte packets  
1  10.1.1.1 (10.1.1.1)  3.182 ms  3.280 ms  3.092 ms  
2  172.16.2.2 (172.16.2.2)  7.236 ms  7.675 ms  8.182 ms
```
#### Spine2:
```
spine2#traceroute 172.16.2.1  
traceroute to 172.16.2.1 (172.16.2.1), 30 hops max, 60 byte packets  
1  10.1.2.1 (10.1.2.1)  2.894 ms  3.181 ms  2.971 ms  
2  172.16.2.1 (172.16.2.1)  7.443 ms  7.681 ms  8.991 ms
```
