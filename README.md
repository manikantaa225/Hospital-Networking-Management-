# Hospital-Networking-Management-System-Cisco-Packet-Tracer

## Overview
This project represents a multi-floor enterprise network topology designed using Cisco Packet Tracer.
The network is divided into three floors, each with its own VLANs for efficient traffic segmentation and management. Inter-VLAN communication is enabled using router-on-a-stick (sub-interfaces), and OSPF (Open Shortest Path First) is implemented as the dynamic routing protocol to ensure connectivity between different floors.

Key features of the project:
Three Routers (R1, R2, R3) connected via serial links in a triangular topology.
Three Switches (one per floor) managing VLANs for PCs, Printers, and Wireless devices.
Eight VLANs (VLAN 10–80) mapped to different departments/floors.

DHCP pools on routers for automatic IP allocation in each VLAN.
Wireless Access Points for laptops and smartphones.
OSPF Area 0 used to share routes between routers and ensure network scalability.
Inter-VLAN Routing for communication across VLANs and floors.
This setup simulates a real-world enterprise building with multiple departments across floors, where VLANs enhance security and efficiency, and OSPF ensures full connectivity.

##  Network Topology

<img width="1599" height="673" alt="Image" src="https://github.com/user-attachments/assets/fadf6bd0-2b55-4204-934d-87b0d9c948bb" />

---

##  IP Addressing Scheme

| VLAN  | Subnet            | Default Gateway | Floor |
|-------|------------------|-----------------|-------|
| 10    | 192.168.1.0/24   | 192.168.1.1     | 3     |
| 20    | 192.168.2.0/24   | 192.168.2.1     | 3     |
| 30    | 192.168.3.0/24   | 192.168.3.1     | 2     |
| 40    | 192.168.4.0/24   | 192.168.4.1     | 2     |
| 50    | 192.168.5.0/24   | 192.168.5.1     | 2     |
| 60    | 192.168.6.0/24   | 192.168.6.1     | 1     |
| 70    | 192.168.7.0/24   | 192.168.7.1     | 1     |
| 80    | 192.168.8.0/24   | 192.168.8.1     | 1     |

 Links (Serial Interfaces)
- R1 ↔ R3 → 10.10.10.0/30  
- R2 ↔ R3 → 10.10.10.4/30  
- R1 ↔ R2 → 10.10.10.8/30  

---

## FLOOR 1

<img width="757" height="530" alt="Image" src="https://github.com/user-attachments/assets/46d4ee96-104f-442c-a2c9-61cf716e34aa" />
## Device used   <br>
3 PCs → PC5, PC6, PC7   <br>
3 Printers → Printer5, Printer6, Printer7       <br>
1 Laptop → Laptop0    <br>
1 Smartphone → Smartphone2     <br>
1 Access Point → Access Point4    <br>
1 Switch → Switch1     <br>
1 Router → Router1     <br>

## Router  Configurations
hostname Router1  <br>

!    <br>
DHCP   <br>
ip dhcp pool Abhi   <br>
 network 192.168.6.0 255.255.255.0   <br>
 default-router 192.168.6.1   <br>
 dns-server 192.168.6.1    <br>

ip dhcp pool Mani      <br>
 network 192.168.7.0 255.255.255.0    <br>
 default-router 192.168.7.1    <br>
 dns-server 192.168.7.1    <br>

ip dhcp pool Chetan    <br>
 network 192.168.8.0 255.255.255.0   <br>
 default-router 192.168.8.1    <br>
 dns-server 192.168.8.1     <br>
 !   <br>
  
 !  <br>
Interfaces    <br>
interface GigabitEthernet0/0   <br>
 no ip address   <br>
 no shutdown    <br>


interface GigabitEthernet0/0.60    <br>
 encapsulation dot1Q 60     <br>
 ip address 192.168.6.1 255.255.255.0    <br>


interface GigabitEthernet0/0.70    <br>
 encapsulation dot1Q 70        <br>
 ip address 192.168.7.1 255.255.255.0     <br>


interface GigabitEthernet0/0.80    <br>
 encapsulation dot1Q 80     <br>
 ip address 192.168.8.1 255.255.255.0    <br>


interface Serial0/3/0     <br>
 ip address 10.10.10.1 255.255.255.252    <br>
 clock rate 64000     <br>
 no shutdown    <br>


interface Serial0/3/1    <br>
 ip address 10.10.10.10 255.255.255.252    <br>
 clock rate 64000     <br>
 no shutdown      <br>
 ! <br>

 !   <br> 
 Routing     <br>
 router ospf 10     <br>
 network 10.10.10.0 0.0.0.3 area 0     <br>
 network 10.10.10.8 0.0.0.3 area 0     <br>
 network 192.168.6.0 0.0.0.255 area 0     <br>
 network 192.168.7.0 0.0.0.255 area 0     <br>
 network 192.168.8.0 0.0.0.255 area 0     <br>
!     <br>

! <br>
SSH  <br>
username abhi secret cisco123 <br>
ip domain-name hospital.local  <br>
crypto key generate rsa   <br>
ip ssh version 2    <br>
line vty 0 4     <br>
transport input ssh    <br>
login local    <br>
 !     <br>

<img width="975" height="681" alt="Image" src="https://github.com/user-attachments/assets/f4ec748d-203e-40ce-ad21-a9ec49339293" />

## Switch configuration  <br>
hostname Switch1  <br>
! <br>
vlan 60   <br>
 name HR    <br>
vlan 70   <br>
 name Finance   <br>
vlan 80   <br>
 name Management   <br>
!   <br>

!  <br>
interface range fa0/2 , fa0/3   <br>
 switchport mode access    <br>
 switchport access vlan 60    <br>

interface range fa0/4 , fa0/5    <br>
 switchport mode access     <br>
 switchport access vlan 70    <br>

interface range fa0/6 , fa0/7     <br>
 switchport mode access     <br>
 switchport access vlan 80    <br>

interface fa0/8    <br>
 switchport mode access       <br>
 switchport access vlan 60     <br>
! <br>

! <br>
interface fa0/1     <br>
 switchport trunk encapsulation dot1q   <br>
 switchport mode trunk    <br>
! <br>

<img width="908" height="565" alt="Image" src="https://github.com/user-attachments/assets/3ab8eeb1-2297-41c1-9cc7-8573f4238b75" />

## FLOOR 2

<img width="769" height="358" alt="Image" src="https://github.com/user-attachments/assets/2a8d7dce-9739-4d39-9b51-23e750313c0a" />

## Device used   <br>
3 PCs → PC2, PC3, PC4   <br>
3 Printers → Printer2, Printer3, Printer4       <br>
1 Laptop → Laptop1    <br>
1 Smartphone → Smartphone0    <br>
1 Access Point → Access Point5    <br>
1 Switch → Switch2     <br>
1 Router → Router2     <br>

## Router configuration

hostname Router2 <br>
!  <br>
DHCP   <br>
ip dhcp pool vlan30  <br>
 network 192.168.3.0 255.255.255.0    <br>
 default-router 192.168.3.1    <br>
 dns-server 192.168.3.1    <br>

ip dhcp pool vlan40   <br>
 network 192.168.4.0 255.255.255.0   <br> 
 default-router 192.168.4.1    <br>
 dns-server 192.168.4.1     <br>

ip dhcp pool vlan50   <br>
 network 192.168.5.0 255.255.255.0    <br>
 default-router 192.168.5.1    <br>
 dns-server 192.168.5.1    <br>
!

! <br>
Interfaces    <br>
interface GigabitEthernet0/0    <br>
 no ip address  <br>
 no shutdown   <br>

interface GigabitEthernet0/0.30  <br>
 encapsulation dot1Q 30    <br>
 ip address 192.168.3.1 255.255.255.0    <br>

interface GigabitEthernet0/0.40  <br>
 encapsulation dot1Q 40    <br>
 ip address 192.168.4.1 255.255.255.0    <br>

interface GigabitEthernet0/0.50    <br>
 encapsulation dot1Q 50     <br>
 ip address 192.168.5.1 255.255.255.0   <br>

interface Serial0/3/0    <br>
 ip address 10.10.10.6 255.255.255.252   <br>
 no shutdown     <br>

interface Serial0/3/1   <br>
 ip address 10.10.10.9 255.255.255.252    <br>
 clock rate 64000      <br>
 no shutdown       <br>
 !    <br>

! <br>
Routing   <br> 
router ospf 10     <br>
 network 10.10.10.4 0.0.0.3 area 0    <br>
 network 10.10.10.8 0.0.0.3 area 0      <br>
 network 192.168.3.0 0.0.0.255 area 0     <br>
 network 192.168.4.0 0.0.0.255 area 0   <br>
 network 192.168.5.0 0.0.0.255 area 0     <br>

 ! <br>
SSH  <br>
username chetan secret cisco123   <br>
ip domain-name hospital.local    <br>
crypto key generate rsa      <br>
ip ssh version 2      <br>
line vty 0 4       <br>
transport input ssh    <br>
login local       <br>
<img width="988" height="614" alt="Image" src="https://github.com/user-attachments/assets/6550e3fd-4f04-465d-82c3-de9f9effab24" />

## Switch confiiguration
hostname Switch2 <br>
!     <br>
vlan 30  <br>
name Reception   <br>
vlan 40   <br>
name Pharmacy  <br>
vlan 50  <br>
name Nursing   <br>
!   <br>

!  <br>
interface  range  fa0/2 , fa0/3   <br>
switchport mode access    <br>
switchport access vlan 30  <br>

interface range  fa0/4 , fa0/5   <br>
switchport mode access     <br>
switchport access vlan 40    <br>

interface  range fa0/6  , fa0/7  , fa0/8  <br> 
switchport mode access  <br> 
switchport access vlan 50    <br> 
! <br>

! <br>
interface fa0/1   <br>
switchport mode trunk  <br>
switchport trunk allowed vlan 1,30,40,50  <br>
! <br>

<img width="891" height="595" alt="Image" src="https://github.com/user-attachments/assets/a6646b7b-5f68-428a-bf29-4aefd5df31a2" />

## Router 3
hostname Router3

! <br>
DHCP   <br>
ip dhcp pool vlan10 <br>  
 network 192.168.1.0 255.255.255.0  <br>
 default-router 192.168.1.1   <br>
 dns-server 192.168.1.1    <br>

ip dhcp pool vlan20    <br>
 network 192.168.2.0 255.255.255.0    <br>
 default-router 192.168.2.1   <br>
 dns-server 192.168.2.1     <br>
! <br>

!  <br>
Interfaces  <br>
interface GigabitEthernet0/0  <br>
 no ip address  <br>
 no shutdown   <br>

interface GigabitEthernet0/0.10    <br>
 encapsulation dot1Q 10    <br>
 ip address 192.168.1.1 255.255.255.0     <br>

interface GigabitEthernet0/0.20    <br>
 encapsulation dot1Q 20      <br>
 ip address 192.168.2.1 255.255.255.0    <br>

 
interface Serial0/3/0   <br>
 ip address 10.10.10.2 255.255.255.252    <br>
 no shutdown    <br>

interface Serial0/3/1    <br>
 ip address 10.10.10.5 255.255.255.252    <br>
 no shutdown     <br>
!<br>
 
! <br> 
Routing    <br>
router ospf 10   <br>
 network 10.10.10.0 0.0.0.3 area 0   <br>
 network 10.10.10.4 0.0.0.3 area 0    <br>
 network 192.168.1.0 0.0.0.255 area 0    <br>
 network 192.168.2.0 0.0.0.255 area 0    <br>

<img width="972" height="585" alt="Image" src="https://github.com/user-attachments/assets/7f955568-1b26-4697-82bc-1766a27599e0" />
<img width="799" height="308" alt="Image" src="https://github.com/user-attachments/assets/35877749-4790-4060-a520-427c37102b7a" />
 









Switch# show vlan brief   <br>
VLAN 10 → Fa0/2, Fa0/3, Fa0/6   <br>
VLAN 20 → Fa0/4, Fa0/5   <br>
Trunk → Fa0/1 (VLANs 1,10,20)   <br>

<img width="954" height="641" alt="Image" src="https://github.com/user-attachments/assets/a82fec29-3213-4305-9cd6-06b93b620254" />


 Routing Protocol   <br>
OSPF Area 0 is used across all routers.   <br>
Provides inter-VLAN communication between floors.    <br>

📡 DHCP Configuration     <br>
Each VLAN is assigned a separate DHCP pool from its connected router.   <br>
Devices (PCs, Laptops, Smartphones, Printers) get IPs dynamically.    <br>

##  Output   <br>


<img width="822" height="699" alt="Image" src="https://github.com/user-attachments/assets/7cc510d1-8103-4d8d-9914-cb49679de552" />
