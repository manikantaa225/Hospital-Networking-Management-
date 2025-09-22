# Hospital-Networking-Management
# Multi-Floor VLAN & OSPF Topology

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

**WAN Links (Serial Interfaces):**
- R1 ↔ R3 → 10.10.10.0/30  
- R2 ↔ R3 → 10.10.10.4/30  
- R1 ↔ R2 → 10.10.10.8/30  

---

## Router Configurations

### Router1
cisco
hostname Router1 <br>
ip dhcp pool Abhi  <br>
 network 192.168.6.0 255.255.255.0
 default-router 192.168.6.1
 dns-server 192.168.6.1
ip dhcp pool Mani
 network 192.168.7.0 255.255.255.0
 default-router 192.168.7.1
 dns-server 192.168.7.1
ip dhcp pool Chetan
 network 192.168.8.0 255.255.255.0
 default-router 192.168.8.1
 dns-server 192.168.8.1
!
router ospf 10
 network 10.10.10.8 0.0.0.3 area 0
 network 10.10.10.0 0.0.0.3 area 0
 network 192.168.6.0 0.0.0.255 area 0
 network 192.168.7.0 0.0.0.255 area 0
 network 192.168.8.0 0.0.0.255 area 0

<img width="975" height="681" alt="Image" src="https://github.com/user-attachments/assets/f4ec748d-203e-40ce-ad21-a9ec49339293" />
<img width="757" height="530" alt="Image" src="https://github.com/user-attachments/assets/46d4ee96-104f-442c-a2c9-61cf716e34aa" />


hostname Router2
ip dhcp pool vlan30
 network 192.168.3.0 255.255.255.0
 default-router 192.168.3.1
 dns-server 192.168.3.1
ip dhcp pool vlan40
 network 192.168.4.0 255.255.255.0
 default-router 192.168.4.1
 dns-server 192.168.4.1
ip dhcp pool vlan50
 network 192.168.5.0 255.255.255.0
 default-router 192.168.5.1
 dns-server 192.168.5.1
!
router ospf 10
 network 10.10.10.8 0.0.0.3 area 0
 network 10.10.10.4 0.0.0.3 area 0
 network 192.168.3.0 0.0.0.255 area 0
 network 192.168.4.0 0.0.0.255 area 0
 network 192.168.5.0 0.0.0.255 area 0

<img width="988" height="614" alt="Image" src="https://github.com/user-attachments/assets/1b50c170-5c40-45cc-93ae-20d9435b650b" />
<img width="769" height="358" alt="Image" src="https://github.com/user-attachments/assets/5ef0ed91-6382-49ea-896c-3476606eb391" />
 
hostname Router3
ip dhcp pool vlan10
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 192.168.1.1
ip dhcp pool vlan20
 network 192.168.2.0 255.255.255.0
 default-router 192.168.2.1
 dns-server 192.168.2.1
!
router ospf 10
 network 10.10.10.4 0.0.0.3 area 0
 network 10.10.10.0 0.0.0.3 area 0
 network 192.168.1.0 0.0.0.255 area 0
 network 192.168.2.0 0.0.0.255 area 0

<img width="972" height="585" alt="Image" src="https://github.com/user-attachments/assets/7f955568-1b26-4697-82bc-1766a27599e0" />
<img width="799" height="308" alt="Image" src="https://github.com/user-attachments/assets/35877749-4790-4060-a520-427c37102b7a" />
 
Switch# show vlan brief
VLAN 60 → Fa0/2, Fa0/3, Fa0/8
VLAN 70 → Fa0/4, Fa0/5
VLAN 80 → Fa0/6, Fa0/7
Trunk → Fa0/1 (VLANs 1,60,70,80)

<img width="908" height="565" alt="Image" src="https://github.com/user-attachments/assets/3ab8eeb1-2297-41c1-9cc7-8573f4238b75" />


Switch# show vlan brief
VLAN 30 → Fa0/2, Fa0/3
VLAN 40 → Fa0/4, Fa0/5
VLAN 50 → Fa0/6, Fa0/7, Fa0/8
Trunk → Fa0/1 (VLANs 1,30,40,50)

<img width="891" height="595" alt="Image" src="https://github.com/user-attachments/assets/a6646b7b-5f68-428a-bf29-4aefd5df31a2" />

Switch# show vlan brief
VLAN 10 → Fa0/2, Fa0/3, Fa0/6
VLAN 20 → Fa0/4, Fa0/5
Trunk → Fa0/1 (VLANs 1,10,20)

<img width="954" height="641" alt="Image" src="https://github.com/user-attachments/assets/a82fec29-3213-4305-9cd6-06b93b620254" />


⚙️ Routing Protocol
OSPF Area 0 is used across all routers.
Provides inter-VLAN communication between floors.

📡 DHCP Configuration
Each VLAN is assigned a separate DHCP pool from its connected router.
Devices (PCs, Laptops, Smartphones, Printers) get IPs dynamically.

##  Output

<img width="822" height="699" alt="Image" src="https://github.com/user-attachments/assets/7cc510d1-8103-4d8d-9914-cb49679de552" />
