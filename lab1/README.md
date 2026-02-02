# Lab1
## Схема:
![[Pasted image 20251230093126.png]]
## План работ:
- Все порты - 1Gbit/s
- На leaf устройствах порты с 1 по 9 подключаются к spine, а с 10 по 36 к оконечным устройствам.
## Адресация:
Второй октет - номер пода, третий - номер спайна.
#### Spine1:
- Ethernet1 - 10.1.1.2/30
- Ethernet2 - 10.1.1.6/30
- Ethernet3 - 10.1.1.10/30
#### Spine2:
- Ethernet1 - 10.1.2.2/30
- Ethernet2 - 10.1.2.6/30
- Ethernet3 - 10.1.2.10/30
#### Leaf1:
- Ethernet1 - 10.1.1.1/30
- Ethernet2 - 10.1.2.1/30
#### Leaf2:
- Ethernet1 - 10.1.1.5/30
- Ethernet2 - 10.1.2.5/30
#### Leaf3:
- Ethernet1 - 10.1.1.9/30
- Ethernet2 - 10.1.2.9/30

## Конфигурация:
#### Spine1:
```
localhost>ena  
localhost#conf t  
localhost(config)#hostname spine1  
spine1(config)#int eth1  
spine1(config-if-Et1)#no sw  
spine1(config-if-Et1)#ip add 10.1.1.2/30  
spine1(config-if-Et1)#int eth2  
spine1(config-if-Et2)#no sw  
spine1(config-if-Et2)#ip add 10.1.1.6/30  
spine1(config-if-Et2)#int eth3  
spine1(config-if-Et3)#no sw  
spine1(config-if-Et3)#ip add 10.1.1.10/30  
spine1(config-if-Et3)#end  
spine1#copy run start  
Copy completed successfully.
```
#### Spine2:
```
localhost>ena  
localhost#conf t  
localhost(config)#hostname spine2  
spine2(config)#int eth1  
spine2(config-if-Et1)#no sw  
spine2(config-if-Et1)#ip add 10.1.2.2/30  
spine2(config-if-Et1)#int eth2  
spine2(config-if-Et2)#no sw  
spine2(config-if-Et2)#ip add 10.1.2.6/30  
spine2(config-if-Et2)#int eth3  
spine2(config-if-Et3)#no sw  
spine2(config-if-Et3)#ip add 10.1.2.10/30  
spine2(config-if-Et3)#end  
spine2#copy run start  
Copy completed successfully.
```
#### Leaf1:
```
localhost>ena  
localhost#conf t  
localhost(config)#hostname leaf1  
leaf1(config)#int eth1  
leaf1(config-if-Et1)#no sw  
leaf1(config-if-Et1)#ip add 10.1.1.1/30  
leaf1(config-if-Et1)#int eth2  
leaf1(config-if-Et2)#no sw  
leaf1(config-if-Et2)#ip add 10.1.2.1/30
leaf1(config-if-Et2)#end  
leaf1#copy run start  
Copy completed successfully.
```
#### Leaf2:
```
localhost>ena  
localhost#conf t  
localhost(config)#hostname leaf2  
leaf2(config)#int eth1  
leaf2(config-if-Et1)#no sw  
leaf2(config-if-Et1)#ip add 10.1.1.5/30  
leaf2(config-if-Et1)#int eth2  
leaf2(config-if-Et2)#no sw  
leaf2(config-if-Et2)#ip add 10.1.2.5/30
leaf2(config-if-Et2)#end  
leaf2#copy run start  
Copy completed successfully.
```
#### Leaf3:
```
localhost>ena  
localhost#conf t  
localhost(config)#hostname leaf3  
leaf3(config)#int eth1  
leaf3(config-if-Et1)#no sw  
leaf3(config-if-Et1)#ip add 10.1.1.9/30  
leaf3(config-if-Et1)#int eth2  
leaf3(config-if-Et2)#no sw  
leaf3(config-if-Et2)#ip add 10.1.2.9/30
leaf3(config-if-Et2)#end  
leaf3#copy run start  
Copy completed successfully.
```