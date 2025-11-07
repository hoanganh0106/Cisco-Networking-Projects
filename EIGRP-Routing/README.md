# 🧩 EIGRP-Routing
This project simulates a network using **EIGRP (Enhanced Interior Gateway Routing Protocol)** between three Cisco routers connected in a full-mesh triangle topology.  
**Objective:** Configure dynamic routing so that all three LANs (VLAN 10, VLAN 20, VLAN 30) can communicate through EIGRP AS 100.

---

## 🧱 Topology Overview
![topology](./topology.png)

**Devices Used:**  
- 3 × Cisco 4331 Routers (ISR4331)  
- 3 × Cisco 3650 Switches  
- 3 × PCs  
- Cisco Packet Tracer 9.0

---

## 🔗 Logical Topology Description

**LAN Segments**
- **VLAN 10 (192.168.10.0/24)**  
  `PC1 (192.168.10.10)` ↔ `SW1` ↔ `R1 G0/0 (192.168.10.1)`
- **VLAN 20 (192.168.20.0/24)**  
  `PC2 (192.168.20.10)` ↔ `SW2` ↔ `R2 G0/0 (192.168.20.1)`
- **VLAN 30 (192.168.30.0/24)**  
  `PC3 (192.168.30.10)` ↔ `SW3` ↔ `R3 G0/0 (192.168.30.1)`

**WAN (Router Interconnections)**  
Routers form a **triangle EIGRP topology** using /30 subnets:  
- `R1 S0/1/0 (10.1.1.1)` ↔ `R2 S0/1/1 (10.1.1.2)`  
- `R1 S0/1/1 (10.1.1.5)` ↔ `R3 S0/1/1 (10.1.1.6)`  
- `R2 S0/1/0 (10.1.1.9)` ↔ `R3 S0/1/0 (10.1.1.10)`

---

## 🌐 IP Addressing Plan

| Device | Interface | IP Address | Subnet Mask | Description |
|---------|------------|-------------|--------------|--------------|
| PC1 | NIC | 192.168.10.10 | 255.255.255.0 | Client in VLAN 10 |
| R1 | G0/0/0 | 192.168.10.1 | 255.255.255.0 | Connects VLAN 10 |
| R1 | S0/1/0 | 10.1.1.1 | 255.255.255.252 | Link to R2 |
| R1 | S0/1/1 | 10.1.1.5 | 255.255.255.252 | Link to R3 |
| R2 | G0/0/0 | 192.168.20.1 | 255.255.255.0 | Connects VLAN 20 |
| R2 | S0/1/1 | 10.1.1.2 | 255.255.255.252 | Link to R1 |
| R2 | S0/1/0 | 10.1.1.9 | 255.255.255.252 | Link to R3 |
| PC2 | NIC | 192.168.20.10 | 255.255.255.0 | Client in VLAN 20 |
| R3 | G0/0/0 | 192.168.30.1 | 255.255.255.0 | Connects VLAN 30 |
| R3 | S0/1/1 | 10.1.1.6 | 255.255.255.252 | Link to R1 |
| R3 | S0/1/0 | 10.1.1.10 | 255.255.255.252 | Link to R2 |
| PC3 | NIC | 192.168.30.10 | 255.255.255.0 | Client in VLAN 30 |

---

## ⚙️ EIGRP Configuration (AS 100)
```bash
router eigrp 100
network 192.168.10.0
network 192.168.20.0
network 192.168.30.0
network 10.1.1.0
no auto-summary
