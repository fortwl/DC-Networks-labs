# Lab4
## Схема:
![[Pasted image 20251230093126.png]]
## План работ:
- Схему и адресацию используем из лабы 1, также настраиваем лупбеки для последующего анонсирования.
- out-delay 0 отдельно не настраиваю, т.к. на Arista он по-умолчанию равен 0.
- Настраиваем underlay на **eBGP**
- Спайны используют AS 65500, а лифы 65501-65534
- router-id равен адресу на лупбеке
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
peer-filter LEAVES  
  10 match as-range 65501-65534 result accept
interface Loopback0
   ip address 172.16.2.1/32
router bgp 65500
   router-id 172.16.2.1
   no bgp default ipv4-unicast
   timers bgp 3 9
   bgp listen range 10.1.0.0/16 peer-group UNDERLAY_POD1 peer-filter LEAVES
   neighbor UNDERLAY_POD1 peer group
   neighbor UNDERLAY_POD1 out-delay 0
   neighbor UNDERLAY_POD1 bfd
   !
   address-family ipv4
      neighbor UNDERLAY_POD1 activate
      network 172.16.2.1/32
```
#### Leaf1
```
interface Loopback0
   ip address 172.16.1.1/32
router bgp 65501
   router-id 172.16.1.1
   no bgp default ipv4-unicast
   timers bgp 3 9
   maximum-paths 2 ecmp 2
   neighbor UNDERLAY_POD1 peer group
   neighbor UNDERLAY_POD1 remote-as 65500
   neighbor UNDERLAY_POD1 bfd
   neighbor 10.1.1.2 peer group UNDERLAY_POD1
   neighbor 10.1.2.2 peer group UNDERLAY_POD1
   !
   address-family ipv4
      neighbor UNDERLAY_POD1 activate
      network 172.16.1.1/32
```

## Результаты после конвергенции BGP
#### Leaf1:
```
leaf1#sh ip bgp  
BGP routing table information for VRF default  
Router identifier 172.16.1.1, local AS number 65501  
Route status codes: s - suppressed contributor, * - valid, > - active, E - ECMP head, e - ECMP  
                   S - Stale, c - Contributing to ECMP, b - backup, L - labeled-unicast  
                   % - Pending best path selection  
Origin codes: i - IGP, e - EGP, ? - incomplete  
RPKI Origin Validation codes: V - valid, I - invalid, U - unknown  
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop  
  
         Network                Next Hop              Metric  AIGP       LocPref Weight  Path  
* >      172.16.1.1/32          -                     -       -          -       0       i  
* >Ec    172.16.1.2/32          10.1.1.2              0       -          100     0       65500 65502 i  
*  ec    172.16.1.2/32          10.1.2.2              0       -          100     0       65500 65502 i  
* >Ec    172.16.1.3/32          10.1.1.2              0       -          100     0       65500 65503 i  
*  ec    172.16.1.3/32          10.1.2.2              0       -          100     0       65500 65503 i  
* >      172.16.2.1/32          10.1.1.2              0       -          100     0       65500 i  
* >      172.16.2.2/32          10.1.2.2              0       -          100     0       65500 i  

leaf1#sh bgp sum  
BGP summary information for VRF default  
Router identifier 172.16.1.1, local AS number 65501  
Neighbor          AS Session State AFI/SAFI                AFI/SAFI State   NLRI Rcd   NLRI Acc  
-------- ----------- ------------- ----------------------- -------------- ---------- ----------  
10.1.1.2       65500 Established   IPv4 Unicast            Negotiated              3          3  
10.1.2.2       65500 Established   IPv4 Unicast            Negotiated              3          3  
```
#### Leaf2:
```
leaf2#sh ip bgp  
BGP routing table information for VRF default  
Router identifier 172.16.1.2, local AS number 65502  
Route status codes: s - suppressed contributor, * - valid, > - active, E - ECMP head, e - ECMP  
                   S - Stale, c - Contributing to ECMP, b - backup, L - labeled-unicast  
                   % - Pending best path selection  
Origin codes: i - IGP, e - EGP, ? - incomplete  
RPKI Origin Validation codes: V - valid, I - invalid, U - unknown  
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop  
  
         Network                Next Hop              Metric  AIGP       LocPref Weight  Path  
* >Ec    172.16.1.1/32          10.1.1.6              0       -          100     0       65500 65501 i  
*  ec    172.16.1.1/32          10.1.2.6              0       -          100     0       65500 65501 i  
* >      172.16.1.2/32          -                     -       -          -       0       i  
* >Ec    172.16.1.3/32          10.1.1.6              0       -          100     0       65500 65503 i  
*  ec    172.16.1.3/32          10.1.2.6              0       -          100     0       65500 65503 i  
* >      172.16.2.1/32          10.1.1.6              0       -          100     0       65500 i  
* >      172.16.2.2/32          10.1.2.6              0       -          100     0       65500 i  

leaf2#sh bgp sum  
BGP summary information for VRF default  
Router identifier 172.16.1.2, local AS number 65502  
Neighbor          AS Session State AFI/SAFI                AFI/SAFI State   NLRI Rcd   NLRI Acc  
-------- ----------- ------------- ----------------------- -------------- ---------- ----------  
10.1.1.6       65500 Established   IPv4 Unicast            Negotiated              3          3  
10.1.2.6       65500 Established   IPv4 Unicast            Negotiated              3          3  
```
#### Leaf3:
```
leaf3#sh ip bgp  
BGP routing table information for VRF default  
Router identifier 172.16.1.3, local AS number 65503  
Route status codes: s - suppressed contributor, * - valid, > - active, E - ECMP head, e - ECMP  
                   S - Stale, c - Contributing to ECMP, b - backup, L - labeled-unicast  
                   % - Pending best path selection  
Origin codes: i - IGP, e - EGP, ? - incomplete  
RPKI Origin Validation codes: V - valid, I - invalid, U - unknown  
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop  
  
         Network                Next Hop              Metric  AIGP       LocPref Weight  Path  
* >Ec    172.16.1.1/32          10.1.1.10             0       -          100     0       65500 65501 i  
*  ec    172.16.1.1/32          10.1.2.10             0       -          100     0       65500 65501 i  
* >Ec    172.16.1.2/32          10.1.1.10             0       -          100     0       65500 65502 i  
*  ec    172.16.1.2/32          10.1.2.10             0       -          100     0       65500 65502 i  
* >      172.16.1.3/32          -                     -       -          -       0       i  
* >      172.16.2.1/32          10.1.1.10             0       -          100     0       65500 i  
* >      172.16.2.2/32          10.1.2.10             0       -          100     0       65500 i  

leaf3#sh bgp summary    
BGP summary information for VRF default  
Router identifier 172.16.1.3, local AS number 65503  
Neighbor           AS Session State AFI/SAFI                AFI/SAFI State   NLRI Rcd   NLRI Acc  
--------- ----------- ------------- ----------------------- -------------- ---------- ----------  
10.1.1.10       65500 Established   IPv4 Unicast            Negotiated              3          3  
10.1.2.10       65500 Established   IPv4 Unicast            Negotiated              3          3  
```
#### Spine1:
```
spine1#sh ip bgp  
BGP routing table information for VRF default  
Router identifier 172.16.2.1, local AS number 65500  
Route status codes: s - suppressed contributor, * - valid, > - active, E - ECMP head, e - ECMP  
                   S - Stale, c - Contributing to ECMP, b - backup, L - labeled-unicast  
                   % - Pending best path selection  
Origin codes: i - IGP, e - EGP, ? - incomplete  
RPKI Origin Validation codes: V - valid, I - invalid, U - unknown  
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop  
  
         Network                Next Hop              Metric  AIGP       LocPref Weight  Path  
* >      172.16.1.1/32          10.1.1.1              0       -          100     0       65501 i  
* >      172.16.1.2/32          10.1.1.5              0       -          100     0       65502 i  
* >      172.16.1.3/32          10.1.1.9              0       -          100     0       65503 i  
* >      172.16.2.1/32          -                     -       -          -       0       i  

spine1#sh bgp sum  
BGP summary information for VRF default  
Router identifier 172.16.2.1, local AS number 65500  
Neighbor          AS Session State AFI/SAFI                AFI/SAFI State   NLRI Rcd   NLRI Acc  
-------- ----------- ------------- ----------------------- -------------- ---------- ----------  
10.1.1.1       65501 Established   IPv4 Unicast            Negotiated              1          1  
10.1.1.5       65502 Established   IPv4 Unicast            Negotiated              1          1  
10.1.1.9       65503 Established   IPv4 Unicast            Negotiated              1          1  
```
#### Spine2:
```
spine2#sh ip bgp  
BGP routing table information for VRF default  
Router identifier 172.16.2.2, local AS number 65500  
Route status codes: s - suppressed contributor, * - valid, > - active, E - ECMP head, e - ECMP  
                   S - Stale, c - Contributing to ECMP, b - backup, L - labeled-unicast  
                   % - Pending best path selection  
Origin codes: i - IGP, e - EGP, ? - incomplete  
RPKI Origin Validation codes: V - valid, I - invalid, U - unknown  
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop  
  
         Network                Next Hop              Metric  AIGP       LocPref Weight  Path  
* >      172.16.1.1/32          10.1.2.1              0       -          100     0       65501 i  
* >      172.16.1.2/32          10.1.2.5              0       -          100     0       65502 i  
* >      172.16.1.3/32          10.1.2.9              0       -          100     0       65503 i  
* >      172.16.2.2/32          -                     -       -          -       0       i  

spine2#sh bgp sum  
BGP summary information for VRF default  
Router identifier 172.16.2.2, local AS number 65500  
Neighbor          AS Session State AFI/SAFI                AFI/SAFI State   NLRI Rcd   NLRI Acc  
-------- ----------- ------------- ----------------------- -------------- ---------- ----------  
10.1.2.1       65501 Established   IPv4 Unicast            Negotiated              1          1  
10.1.2.5       65502 Established   IPv4 Unicast            Negotiated              1          1  
10.1.2.9       65503 Established   IPv4 Unicast            Negotiated              1          1  
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
C        10.1.2.0/30  
          directly connected, Ethernet2  
C        172.16.1.1/32  
          directly connected, Loopback0  
B E      172.16.1.2/32 [200/0]  
          via 10.1.1.2, Ethernet1  
          via 10.1.2.2, Ethernet2  
B E      172.16.1.3/32 [200/0]  
          via 10.1.1.2, Ethernet1  
          via 10.1.2.2, Ethernet2  
B E      172.16.2.1/32 [200/0]  
          via 10.1.1.2, Ethernet1  
B E      172.16.2.2/32 [200/0]  
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
  
C        10.1.1.4/30  
          directly connected, Ethernet1  
C        10.1.2.4/30  
          directly connected, Ethernet2  
B E      172.16.1.1/32 [200/0]  
          via 10.1.1.6, Ethernet1  
          via 10.1.2.6, Ethernet2  
C        172.16.1.2/32  
          directly connected, Loopback0  
B E      172.16.1.3/32 [200/0]  
          via 10.1.1.6, Ethernet1  
          via 10.1.2.6, Ethernet2  
B E      172.16.2.1/32 [200/0]  
          via 10.1.1.6, Ethernet1  
B E      172.16.2.2/32 [200/0]  
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
  
C        10.1.1.8/30  
          directly connected, Ethernet1  
C        10.1.2.8/30  
          directly connected, Ethernet2  
B E      172.16.1.1/32 [200/0]  
          via 10.1.1.10, Ethernet1  
          via 10.1.2.10, Ethernet2  
B E      172.16.1.2/32 [200/0]  
          via 10.1.1.10, Ethernet1  
          via 10.1.2.10, Ethernet2  
C        172.16.1.3/32  
          directly connected, Loopback0  
B E      172.16.2.1/32 [200/0]  
          via 10.1.1.10, Ethernet1  
B E      172.16.2.2/32 [200/0]  
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
B E      172.16.1.1/32 [200/0]  
          via 10.1.1.1, Ethernet1  
B E      172.16.1.2/32 [200/0]  
          via 10.1.1.5, Ethernet2  
B E      172.16.1.3/32 [200/0]  
          via 10.1.1.9, Ethernet3  
C        172.16.2.1/32  
          directly connected, Loopback0
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
  
C        10.1.2.0/30  
          directly connected, Ethernet1  
C        10.1.2.4/30  
          directly connected, Ethernet2  
C        10.1.2.8/30  
          directly connected, Ethernet3  
B E      172.16.1.1/32 [200/0]  
          via 10.1.2.1, Ethernet1  
B E      172.16.1.2/32 [200/0]  
          via 10.1.2.5, Ethernet2  
B E      172.16.1.3/32 [200/0]  
          via 10.1.2.9, Ethernet3  
C        172.16.2.2/32  
          directly connected, Loopback0
```

## Проверка доступности

#### Leaf1:
```
leaf1#trace 172.16.1.2 source 172.16.1.1  
traceroute to 172.16.1.2 (172.16.1.2), 30 hops max, 60 byte packets  
1  10.1.1.2 (10.1.1.2)  2.230 ms  2.375 ms  2.251 ms  
2  172.16.1.2 (172.16.1.2)  6.124 ms  6.313 ms  7.414 ms  

leaf1#trace 172.16.1.3 source 172.16.1.1  
traceroute to 172.16.1.3 (172.16.1.3), 30 hops max, 60 byte packets  
1  10.1.1.2 (10.1.1.2)  2.998 ms  2.690 ms  2.564 ms  
2  172.16.1.3 (172.16.1.3)  6.938 ms  8.017 ms  8.377 ms  

leaf1#trace 172.16.2.1 source 172.16.1.1  
traceroute to 172.16.2.1 (172.16.2.1), 30 hops max, 60 byte packets  
1  172.16.2.1 (172.16.2.1)  3.472 ms  3.167 ms  3.076 ms  

leaf1#trace 172.16.2.2 source 172.16.1.1  
traceroute to 172.16.2.2 (172.16.2.2), 30 hops max, 60 byte packets  
1  172.16.2.2 (172.16.2.2)  3.269 ms  2.970 ms  2.848 ms
```
#### Leaf2:
```
leaf2#trace 172.16.1.1 source 172.16.1.2  
traceroute to 172.16.1.1 (172.16.1.1), 30 hops max, 60 byte packets  
1  10.1.1.6 (10.1.1.6)  3.085 ms  2.778 ms  2.641 ms  
2  172.16.1.1 (172.16.1.1)  7.299 ms  7.542 ms  8.735 ms  

leaf2#trace 172.16.1.3 source 172.16.1.2  
traceroute to 172.16.1.3 (172.16.1.3), 30 hops max, 60 byte packets  
1  10.1.1.6 (10.1.1.6)  3.144 ms  2.922 ms  2.735 ms  
2  172.16.1.3 (172.16.1.3)  6.157 ms  7.292 ms  9.367 ms  

leaf2#trace 172.16.2.1 source 172.16.1.2  
traceroute to 172.16.2.1 (172.16.2.1), 30 hops max, 60 byte packets  
1  172.16.2.1 (172.16.2.1)  2.785 ms  2.676 ms  2.711 ms  

leaf2#trace 172.16.2.2 source 172.16.1.2  
traceroute to 172.16.2.2 (172.16.2.2), 30 hops max, 60 byte packets  
1  172.16.2.2 (172.16.2.2)  2.187 ms  2.069 ms  1.884 ms
```
#### Leaf3:
```
leaf3#trace 172.16.1.1 source 172.16.1.3  
traceroute to 172.16.1.1 (172.16.1.1), 30 hops max, 60 byte packets  
1  10.1.1.10 (10.1.1.10)  2.925 ms  5.191 ms  5.054 ms  
2  172.16.1.1 (172.16.1.1)  9.747 ms  11.158 ms  11.052 ms  

leaf3#trace 172.16.1.2 source 172.16.1.3  
traceroute to 172.16.1.2 (172.16.1.2), 30 hops max, 60 byte packets  
1  10.1.1.10 (10.1.1.10)  3.533 ms  3.280 ms  3.124 ms  
2  172.16.1.2 (172.16.1.2)  8.360 ms  8.239 ms  8.424 ms  

leaf3#trace 172.16.2.1 source 172.16.1.3  
traceroute to 172.16.2.1 (172.16.2.1), 30 hops max, 60 byte packets  
1  172.16.2.1 (172.16.2.1)  2.804 ms  2.714 ms  2.693 ms  

leaf3#trace 172.16.2.2 source 172.16.1.3  
traceroute to 172.16.2.2 (172.16.2.2), 30 hops max, 60 byte packets  
1  172.16.2.2 (172.16.2.2)  2.706 ms  2.710 ms  2.696 ms
```
#### Spine1:
При использовании eBGP коннект между спайнами, если он нужен, придётся организовывать, например с помощью статического роутинга.
Связность между спайнами и лифами продемонстрирована выше.