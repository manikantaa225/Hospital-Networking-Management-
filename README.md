# Hospital-Networking-Management
# Multi-Floor VLAN & OSPF Topology

## 📌 Overview
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

## 🖼️ Network Topology


---

## 🗂️ IP Addressing Scheme

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

## 🔹 Router Configurations

### Router1
```cisco
hostname Router1
ip dhcp pool Abhi
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

Switch# show vlan brief
VLAN 60 → Fa0/2, Fa0/3, Fa0/8
VLAN 70 → Fa0/4, Fa0/5
VLAN 80 → Fa0/6, Fa0/7
Trunk → Fa0/1 (VLANs 1,60,70,80)


Switch# show vlan brief
VLAN 30 → Fa0/2, Fa0/3
VLAN 40 → Fa0/4, Fa0/5
VLAN 50 → Fa0/6, Fa0/7, Fa0/8
Trunk → Fa0/1 (VLANs 1,30,40,50)

Switch# show vlan brief
VLAN 10 → Fa0/2, Fa0/3, Fa0/6
VLAN 20 → Fa0/4, Fa0/5
Trunk → Fa0/1 (VLANs 1,10,20)

⚙️ Routing Protocol
OSPF Area 0 is used across all routers.
Provides inter-VLAN communication between floors.

📡 DHCP Configuration
Each VLAN is assigned a separate DHCP pool from its connected router.
Devices (PCs, Laptops, Smartphones, Printers) get IPs dynamically.
