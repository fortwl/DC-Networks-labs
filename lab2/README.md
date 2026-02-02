# Lab2
## Схема:
![scheme](../attachments/Pasted_image_20251230093126.png)
## План работ:
- Схему и адресацию используем из лабы 1, также настраиваем лупбеки для последующего анонсирования.
- Включаем passive-interface default + no passive для нужных интерфейсов
- Указываем router-id
- Настраиваем интерфейсы
	- ip ospf area 1
	- ip ospf network point-to-point

## Таблица router-id
- Spine1 - 11.11.11.11
- Spine2 - 22.22.22.22
- Leaf1 - 1.1.1.1
- Leaf2 - 2.2.2.2
- Leaf3 - 3.3.3.3

## Таблица лупбеков
- Spine1 - 172.16.2.1/32
- Spine2 - 172.16.2.2/32
- Leaf1 - 172.16.1.1/32
- Leaf2 - 172.16.1.2/32
- Leaf3 - 172.16.1.3/32

## Примеры конфигурации
#### Spine1
```
...
interface Ethernet1  
  no switchport  
  ip address 10.1.1.2/30  
  ip ospf network point-to-point  
  ip ospf area 0.0.0.1  
interface Ethernet2  
  no switchport  
  ip address 10.1.1.6/30  
  ip ospf network point-to-point  
  ip ospf area 0.0.0.1  
interface Ethernet3  
  no switchport  
  ip address 10.1.1.10/30  
  ip ospf network point-to-point  
  ip ospf area 0.0.0.1
interface Loopback0  
  ip address 172.16.2.1/32  
  ip ospf area 0.0.0.1
...
ip routing  
...
router ospf 1  
  router-id 11.11.11.11  
  passive-interface default  
  no passive-interface Ethernet1  
  no passive-interface Ethernet2  
  no passive-interface Ethernet3  
  max-lsa 12000  
```
#### Leaf1
```
...
interface Ethernet1  
  no switchport  
  ip address 10.1.1.1/30  
  ip ospf network point-to-point  
  ip ospf area 0.0.0.1  
interface Ethernet2  
  no switchport  
  ip address 10.1.2.1/30  
  ip ospf network point-to-point  
  ip ospf area 0.0.0.1  
interface Loopback0  
  ip address 172.16.1.1/32  
  ip ospf area 0.0.0.1
...
ip routing  
...
router ospf 1  
  router-id 1.1.1.1  
  passive-interface default  
  no passive-interface Ethernet1  
  no passive-interface Ethernet2  
  max-lsa 12000
```

## Результаты после конвергенции OSPF Area1

#### Leaf1:
```
leaf1#sh ip ospf  
OSPF instance 1 with ID 1.1.1.1 VRF default  
Supports opaque LSA  
Maximum number of LSA allowed 12000  
 Threshold for warning message 75%  
 Ignore-time 5 minutes, reset-time 5 minutes  
 Ignore-count allowed 5, current 0  
It is not an autonomous system boundary router and is not an area border router  
Initial SPF schedule delay 0 msecs  
Minimum hold time between two consecutive SPFs 5000 msecs  
Current hold time between two consecutive SPFs 5000 msecs  
Maximum wait time between two consecutive SPFs 5000 msecs  
Minimum LSA arrival 1000 msecs  
Initial LSA throttle delay 1000 msecs  
Minimum hold time for LSA throttle 5000 msecs  
Maximum wait time for LSA throttle 5000 msecs  
Number of external LSA 0, Checksum sum 0  
Number of opaque AS LSA 0, Checksum sum 0  
Number of areas in this router is 1. 1 normal, 0 stub, 0 nssa  
Number of LSA 5  
Time since last SPF 2037 secs  
No Scheduled SPF  
Adjacency exchange-start threshold is 20  
Maximum number of next-hops supported in ECMP is 128  
Retransmission threshold for LSA is 10  
Number of backbone neighbors is 0  
Routes over tunnel interfaces disabled  
Graceful-restart is not configured  
Graceful-restart-helper mode is enabled  
Area 0.0.0.1  
Number of interface in this area is 3  
  It is a normal area  
  Traffic engineering is disabled  
  Area has None authentication    
  SPF algorithm executed 12 times  
  Number of LSA 5. Checksum Sum 226721  
  Number of opaque link LSA 0. Checksum Sum 0  
  Number of opaque area LSA 0. Checksum Sum 0  
  Number of indication LSA 0  
  Number of DC-clear LSA 0  
  
leaf1#sh ip ospf neighbor  
Neighbor ID     Instance VRF      Pri State                  Dead Time   Address         Interface  
22.22.22.22     1        default  0   FULL                   00:00:31    10.1.2.2        Ethernet2  
11.11.11.11     1        default  0   FULL                   00:00:38    10.1.1.2        Ethernet1  

leaf1#sh ip ospf int br  
  Interface          Instance VRF        Area            IP Address         Cost  State      Nbrs  
  Et2                1        default    0.0.0.1         10.1.2.1/30        10    P2P        1  
  Et1                1        default    0.0.0.1         10.1.1.1/30        10    P2P        1  
  Lo0                1        default    0.0.0.1         172.16.1.1/32      10    DR         0
```
#### Leaf2:
```
leaf2#sh ip ospf  
OSPF instance 1 with ID 2.2.2.2 VRF default  
Supports opaque LSA  
Maximum number of LSA allowed 12000  
 Threshold for warning message 75%  
 Ignore-time 5 minutes, reset-time 5 minutes  
 Ignore-count allowed 5, current 0  
It is not an autonomous system boundary router and is not an area border router  
Initial SPF schedule delay 0 msecs  
Minimum hold time between two consecutive SPFs 5000 msecs  
Current hold time between two consecutive SPFs 5000 msecs  
Maximum wait time between two consecutive SPFs 5000 msecs  
Minimum LSA arrival 1000 msecs  
Initial LSA throttle delay 1000 msecs  
Minimum hold time for LSA throttle 5000 msecs  
Maximum wait time for LSA throttle 5000 msecs  
Number of external LSA 0, Checksum sum 0  
Number of opaque AS LSA 0, Checksum sum 0  
Number of areas in this router is 1. 1 normal, 0 stub, 0 nssa  
Number of LSA 5  
Time since last SPF 2093 secs  
No Scheduled SPF  
Adjacency exchange-start threshold is 20  
Maximum number of next-hops supported in ECMP is 128  
Retransmission threshold for LSA is 10  
Number of backbone neighbors is 0  
Routes over tunnel interfaces disabled  
Graceful-restart is not configured  
Graceful-restart-helper mode is enabled  
Area 0.0.0.1  
Number of interface in this area is 3  
  It is a normal area  
  Traffic engineering is disabled  
  Area has None authentication    
  SPF algorithm executed 12 times  
  Number of LSA 5. Checksum Sum 226721  
  Number of opaque link LSA 0. Checksum Sum 0  
  Number of opaque area LSA 0. Checksum Sum 0  
  Number of indication LSA 0  
  Number of DC-clear LSA 0  
  
leaf2#sh ip ospf neighbor  
Neighbor ID     Instance VRF      Pri State                  Dead Time   Address         Interface  
22.22.22.22     1        default  0   FULL                   00:00:29    10.1.2.6        Ethernet2  
11.11.11.11     1        default  0   FULL                   00:00:29    10.1.1.6        Ethernet1  

leaf2#sh ip ospf int br  
  Interface          Instance VRF        Area            IP Address         Cost  State      Nbrs  
  Et2                1        default    0.0.0.1         10.1.2.5/30        10    P2P        1  
  Et1                1        default    0.0.0.1         10.1.1.5/30        10    P2P        1  
  Lo0                1        default    0.0.0.1         172.16.1.2/32      10    DR         0
```
#### Leaf3:
```
leaf3#sh ip ospf  
OSPF instance 1 with ID 3.3.3.3 VRF default  
Supports opaque LSA  
Maximum number of LSA allowed 12000  
 Threshold for warning message 75%  
 Ignore-time 5 minutes, reset-time 5 minutes  
 Ignore-count allowed 5, current 0  
It is not an autonomous system boundary router and is not an area border router  
Initial SPF schedule delay 0 msecs  
Minimum hold time between two consecutive SPFs 5000 msecs  
Current hold time between two consecutive SPFs 5000 msecs  
Maximum wait time between two consecutive SPFs 5000 msecs  
Minimum LSA arrival 1000 msecs  
Initial LSA throttle delay 1000 msecs  
Minimum hold time for LSA throttle 5000 msecs  
Maximum wait time for LSA throttle 5000 msecs  
Number of external LSA 0, Checksum sum 0  
Number of opaque AS LSA 0, Checksum sum 0  
Number of areas in this router is 1. 1 normal, 0 stub, 0 nssa  
Number of LSA 5  
Time since last SPF 2099 secs  
No Scheduled SPF  
Adjacency exchange-start threshold is 20  
Maximum number of next-hops supported in ECMP is 128  
Retransmission threshold for LSA is 10  
Number of backbone neighbors is 0  
Routes over tunnel interfaces disabled  
Graceful-restart is not configured  
Graceful-restart-helper mode is enabled  
Area 0.0.0.1  
Number of interface in this area is 3  
  It is a normal area  
  Traffic engineering is disabled  
  Area has None authentication    
  SPF algorithm executed 11 times  
  Number of LSA 5. Checksum Sum 226721  
  Number of opaque link LSA 0. Checksum Sum 0  
  Number of opaque area LSA 0. Checksum Sum 0  
  Number of indication LSA 0  
  Number of DC-clear LSA 0  
  
leaf3#sh ip ospf neighbor    
Neighbor ID     Instance VRF      Pri State                  Dead Time   Address         Interface  
22.22.22.22     1        default  0   FULL                   00:00:37    10.1.2.10       Ethernet2  
11.11.11.11     1        default  0   FULL                   00:00:32    10.1.1.10       Ethernet1  

leaf3#sh ip ospf int brief    
  Interface          Instance VRF        Area            IP Address         Cost  State      Nbrs  
  Et2                1        default    0.0.0.1         10.1.2.9/30        10    P2P        1  
  Et1                1        default    0.0.0.1         10.1.1.9/30        10    P2P        1  
  Lo0                1        default    0.0.0.1         172.16.1.3/32      10    DR         0
```
#### Spine1:
```
spine1#sh ip ospf  
OSPF instance 1 with ID 11.11.11.11 VRF default  
Supports opaque LSA  
Maximum number of LSA allowed 12000  
 Threshold for warning message 75%  
 Ignore-time 5 minutes, reset-time 5 minutes  
 Ignore-count allowed 5, current 0  
It is not an autonomous system boundary router and is not an area border router  
Initial SPF schedule delay 0 msecs  
Minimum hold time between two consecutive SPFs 5000 msecs  
Current hold time between two consecutive SPFs 5000 msecs  
Maximum wait time between two consecutive SPFs 5000 msecs  
Minimum LSA arrival 1000 msecs  
Initial LSA throttle delay 1000 msecs  
Minimum hold time for LSA throttle 5000 msecs  
Maximum wait time for LSA throttle 5000 msecs  
Number of external LSA 0, Checksum sum 0  
Number of opaque AS LSA 0, Checksum sum 0  
Number of areas in this router is 1. 1 normal, 0 stub, 0 nssa  
Number of LSA 5  
Time since last SPF 2101 secs  
No Scheduled SPF  
Adjacency exchange-start threshold is 20  
Maximum number of next-hops supported in ECMP is 128  
Retransmission threshold for LSA is 10  
Number of backbone neighbors is 0  
Routes over tunnel interfaces disabled  
Graceful-restart is not configured  
Graceful-restart-helper mode is enabled  
Area 0.0.0.1  
Number of interface in this area is 4  
  It is a normal area  
  Traffic engineering is disabled  
  Area has None authentication    
  SPF algorithm executed 12 times  
  Number of LSA 5. Checksum Sum 226721  
  Number of opaque link LSA 0. Checksum Sum 0  
  Number of opaque area LSA 0. Checksum Sum 0  
  Number of indication LSA 0  
  Number of DC-clear LSA 0  
  
spine1#sh ip ospf neighbor    
Neighbor ID     Instance VRF      Pri State                  Dead Time   Address         Interface  
2.2.2.2         1        default  0   FULL                   00:00:33    10.1.1.5        Ethernet2  
1.1.1.1         1        default  0   FULL                   00:00:36    10.1.1.1        Ethernet1  
3.3.3.3         1        default  0   FULL                   00:00:37    10.1.1.9        Ethernet3  

spine1#sh ip ospf int brie  
  Interface          Instance VRF        Area            IP Address         Cost  State      Nbrs  
  Et2                1        default    0.0.0.1         10.1.1.6/30        10    P2P        1  
  Et1                1        default    0.0.0.1         10.1.1.2/30        10    P2P        1  
  Et3                1        default    0.0.0.1         10.1.1.10/30       10    P2P        1  
  Lo0                1        default    0.0.0.1         172.16.2.1/32      10    DR         0
```
#### Spine2:
```
spine2#sh ip ospf  
OSPF instance 1 with ID 22.22.22.22 VRF default  
Supports opaque LSA  
Maximum number of LSA allowed 12000  
 Threshold for warning message 75%  
 Ignore-time 5 minutes, reset-time 5 minutes  
 Ignore-count allowed 5, current 0  
It is not an autonomous system boundary router and is not an area border router  
Initial SPF schedule delay 0 msecs  
Minimum hold time between two consecutive SPFs 5000 msecs  
Current hold time between two consecutive SPFs 5000 msecs  
Maximum wait time between two consecutive SPFs 5000 msecs  
Minimum LSA arrival 1000 msecs  
Initial LSA throttle delay 1000 msecs  
Minimum hold time for LSA throttle 5000 msecs  
Maximum wait time for LSA throttle 5000 msecs  
Number of external LSA 0, Checksum sum 0  
Number of opaque AS LSA 0, Checksum sum 0  
Number of areas in this router is 1. 1 normal, 0 stub, 0 nssa  
Number of LSA 5  
Time since last SPF 2090 secs  
No Scheduled SPF  
Adjacency exchange-start threshold is 20  
Maximum number of next-hops supported in ECMP is 128  
Retransmission threshold for LSA is 10  
Number of backbone neighbors is 0  
Routes over tunnel interfaces disabled  
Graceful-restart is not configured  
Graceful-restart-helper mode is enabled  
Area 0.0.0.1  
Number of interface in this area is 4  
  It is a normal area  
  Traffic engineering is disabled  
  Area has None authentication    
  SPF algorithm executed 12 times  
  Number of LSA 5. Checksum Sum 226721  
  Number of opaque link LSA 0. Checksum Sum 0  
  Number of opaque area LSA 0. Checksum Sum 0  
  Number of indication LSA 0  
  Number of DC-clear LSA 0  
  
spine2#sh ip ospf neighbor    
Neighbor ID     Instance VRF      Pri State                  Dead Time   Address         Interface  
2.2.2.2         1        default  0   FULL                   00:00:33    10.1.2.5        Ethernet2  
1.1.1.1         1        default  0   FULL                   00:00:34    10.1.2.1        Ethernet1  
3.3.3.3         1        default  0   FULL                   00:00:29    10.1.2.9        Ethernet3  

spine2#sh ip ospf interface brief  
  Interface          Instance VRF        Area            IP Address         Cost  State      Nbrs  
  Et2                1        default    0.0.0.1         10.1.2.6/30        10    P2P        1  
  Et1                1        default    0.0.0.1         10.1.2.2/30        10    P2P        1  
  Et3                1        default    0.0.0.1         10.1.2.10/30       10    P2P        1  
  Lo0                1        default    0.0.0.1         172.16.2.2/32      10    DR         0
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
O        10.1.1.4/30 [110/20]  
          via 10.1.1.2, Ethernet1  
O        10.1.1.8/30 [110/20]  
          via 10.1.1.2, Ethernet1  
C        10.1.2.0/30  
          directly connected, Ethernet2  
O        10.1.2.4/30 [110/20]  
          via 10.1.2.2, Ethernet2  
O        10.1.2.8/30 [110/20]  
          via 10.1.2.2, Ethernet2  
C        172.16.1.1/32  
          directly connected, Loopback0  
O        172.16.1.2/32 [110/30]  
          via 10.1.1.2, Ethernet1  
          via 10.1.2.2, Ethernet2  
O        172.16.1.3/32 [110/30]  
          via 10.1.1.2, Ethernet1  
          via 10.1.2.2, Ethernet2  
O        172.16.2.1/32 [110/20]  
          via 10.1.1.2, Ethernet1  
O        172.16.2.2/32 [110/20]  
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
  
O        10.1.1.0/30 [110/20]  
          via 10.1.1.6, Ethernet1  
C        10.1.1.4/30  
          directly connected, Ethernet1  
O        10.1.1.8/30 [110/20]  
          via 10.1.1.6, Ethernet1  
O        10.1.2.0/30 [110/20]  
          via 10.1.2.6, Ethernet2  
C        10.1.2.4/30  
          directly connected, Ethernet2  
O        10.1.2.8/30 [110/20]  
          via 10.1.2.6, Ethernet2  
O        172.16.1.1/32 [110/30]  
          via 10.1.1.6, Ethernet1  
          via 10.1.2.6, Ethernet2  
C        172.16.1.2/32  
          directly connected, Loopback0  
O        172.16.1.3/32 [110/30]  
          via 10.1.1.6, Ethernet1  
          via 10.1.2.6, Ethernet2  
O        172.16.2.1/32 [110/20]  
          via 10.1.1.6, Ethernet1  
O        172.16.2.2/32 [110/20]  
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
  
O        10.1.1.0/30 [110/20]  
          via 10.1.1.10, Ethernet1  
O        10.1.1.4/30 [110/20]  
          via 10.1.1.10, Ethernet1  
C        10.1.1.8/30  
          directly connected, Ethernet1  
O        10.1.2.0/30 [110/20]  
          via 10.1.2.10, Ethernet2  
O        10.1.2.4/30 [110/20]  
          via 10.1.2.10, Ethernet2  
C        10.1.2.8/30  
          directly connected, Ethernet2  
O        172.16.1.1/32 [110/30]  
          via 10.1.1.10, Ethernet1  
          via 10.1.2.10, Ethernet2  
O        172.16.1.2/32 [110/30]  
          via 10.1.1.10, Ethernet1  
          via 10.1.2.10, Ethernet2  
C        172.16.1.3/32  
          directly connected, Loopback0  
O        172.16.2.1/32 [110/20]  
          via 10.1.1.10, Ethernet1  
O        172.16.2.2/32 [110/20]  
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
O        10.1.2.0/30 [110/20]  
          via 10.1.1.1, Ethernet1  
O        10.1.2.4/30 [110/20]  
          via 10.1.1.5, Ethernet2  
O        10.1.2.8/30 [110/20]  
          via 10.1.1.9, Ethernet3  
O        172.16.1.1/32 [110/20]  
          via 10.1.1.1, Ethernet1  
O        172.16.1.2/32 [110/20]  
          via 10.1.1.5, Ethernet2  
O        172.16.1.3/32 [110/20]  
          via 10.1.1.9, Ethernet3  
C        172.16.2.1/32  
          directly connected, Loopback0  
O        172.16.2.2/32 [110/30]  
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
  
O        10.1.1.0/30 [110/20]  
          via 10.1.2.1, Ethernet1  
O        10.1.1.4/30 [110/20]  
          via 10.1.2.5, Ethernet2  
O        10.1.1.8/30 [110/20]  
          via 10.1.2.9, Ethernet3  
C        10.1.2.0/30  
          directly connected, Ethernet1  
C        10.1.2.4/30  
          directly connected, Ethernet2  
C        10.1.2.8/30  
          directly connected, Ethernet3  
O        172.16.1.1/32 [110/20]  
          via 10.1.2.1, Ethernet1  
O        172.16.1.2/32 [110/20]  
          via 10.1.2.5, Ethernet2  
O        172.16.1.3/32 [110/20]  
          via 10.1.2.9, Ethernet3  
O        172.16.2.1/32 [110/30]  
          via 10.1.2.1, Ethernet1  
          via 10.1.2.5, Ethernet2  
          via 10.1.2.9, Ethernet3  
C        172.16.2.2/32  
          directly connected, Loopback0
```

## Проверка доступности

#### Leaf1:
```
leaf1#trace 172.16.1.2  
traceroute to 172.16.1.2 (172.16.1.2), 30 hops max, 60 byte packets  
1  10.1.1.2 (10.1.1.2)  2.908 ms  2.799 ms  2.681 ms  
2  172.16.1.2 (172.16.1.2)  6.424 ms  8.582 ms  8.505 ms  
leaf1#trace 172.16.1.3  
traceroute to 172.16.1.3 (172.16.1.3), 30 hops max, 60 byte packets  
1  10.1.1.2 (10.1.1.2)  2.620 ms  2.694 ms  2.564 ms  
2  172.16.1.3 (172.16.1.3)  6.537 ms  6.232 ms  7.878 ms  
leaf1#trace 172.16.2.2  
traceroute to 172.16.2.2 (172.16.2.2), 30 hops max, 60 byte packets  
1  172.16.2.2 (172.16.2.2)  3.486 ms  3.336 ms  3.197 ms  
leaf1#trace 172.16.2.1  
traceroute to 172.16.2.1 (172.16.2.1), 30 hops max, 60 byte packets  
1  172.16.2.1 (172.16.2.1)  3.308 ms  2.974 ms  3.098 ms
```
#### Leaf2:
```
leaf2#trace 172.16.1.1  
traceroute to 172.16.1.1 (172.16.1.1), 30 hops max, 60 byte packets  
1  10.1.1.6 (10.1.1.6)  3.245 ms  2.950 ms  2.815 ms  
2  172.16.1.1 (172.16.1.1)  9.048 ms  9.572 ms  9.655 ms  
leaf2#trace 172.16.1.3  
traceroute to 172.16.1.3 (172.16.1.3), 30 hops max, 60 byte packets  
1  10.1.1.6 (10.1.1.6)  2.806 ms  2.370 ms  2.241 ms  
2  172.16.1.3 (172.16.1.3)  6.407 ms  6.860 ms  8.917 ms  
leaf2#trace 172.16.2.2  
traceroute to 172.16.2.2 (172.16.2.2), 30 hops max, 60 byte packets  
1  172.16.2.2 (172.16.2.2)  3.159 ms  2.801 ms  2.675 ms  
leaf2#trace 172.16.2.1  
traceroute to 172.16.2.1 (172.16.2.1), 30 hops max, 60 byte packets  
1  172.16.2.1 (172.16.2.1)  2.620 ms  2.695 ms  2.570 ms
```
#### Leaf3:
```
leaf3#trace 172.16.1.1  
traceroute to 172.16.1.1 (172.16.1.1), 30 hops max, 60 byte packets  
1  10.1.1.10 (10.1.1.10)  2.773 ms  2.830 ms  2.684 ms  
2  172.16.1.1 (172.16.1.1)  6.133 ms  7.848 ms  9.124 ms  
leaf3#trace 172.16.1.2  
traceroute to 172.16.1.2 (172.16.1.2), 30 hops max, 60 byte packets  
1  10.1.1.10 (10.1.1.10)  2.767 ms  2.572 ms  2.440 ms  
2  172.16.1.2 (172.16.1.2)  6.296 ms  6.509 ms  7.445 ms  
leaf3#trace 172.16.2.2  
traceroute to 172.16.2.2 (172.16.2.2), 30 hops max, 60 byte packets  
1  172.16.2.2 (172.16.2.2)  3.151 ms  2.976 ms  2.843 ms  
leaf3#trace 172.16.2.1  
traceroute to 172.16.2.1 (172.16.2.1), 30 hops max, 60 byte packets  
1  172.16.2.1 (172.16.2.1)  3.535 ms  3.462 ms  3.609 ms
```
#### Spine1:
```
spine1#trace 172.16.2.2  
traceroute to 172.16.2.2 (172.16.2.2), 30 hops max, 60 byte packets  
1  10.1.1.1 (10.1.1.1)  3.366 ms  3.088 ms  2.937 ms  
2  172.16.2.2 (172.16.2.2)  6.621 ms  6.524 ms  8.096 ms
```
#### Spine2:
```
spine2#trace 172.16.2.1  
traceroute to 172.16.2.1 (172.16.2.1), 30 hops max, 60 byte packets  
1  10.1.2.1 (10.1.2.1)  3.073 ms  3.148 ms  3.007 ms  
2  172.16.2.1 (172.16.2.1)  8.294 ms  8.236 ms  9.621 ms
```
