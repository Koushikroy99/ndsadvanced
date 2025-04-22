<p align="center">
    <img src="./cisco-logo.png" alt="Logo" width="200">
</p>

<h2 align="center"> NDS Advanced GNS3 Projects (Sunny Sir - NDS Lab)</h2>

---

## 📝About this project

Welcome to my **NDS Advanced GNS3 Projects Repository**, inspired by **Sunny Sir** and the **(NDS)** training.  
Here, you will find real-world Cisco GNS3 lab topologies and configurations simulating advanced enterprise and ISP-level networks. These labs are designed for CCNA, CCNP learners, and networking enthusiasts aiming to enhance hands-on skills using real Cisco IOS images in GNS3.

---
## Networking Labs

### 1. Basic OSPF Routing Lab with 5 Routers and 2 PCs

<p align="center">
    <img src="./1. OSPF Lab.png" alt="OSPF Routing Lab">
</p>

<details>
<summary><strong>⚙️ Steps to Configure OSPF Routing (5 Routers and 2 PCs)</strong></summary>

<br>

## 🧩 Network Topology:
- **5 Routers** (R1 to R5) connected in a linear fashion
- **2 PCs** (PC1 connected to R1, PC2 connected to R5)
- **/30 subnets** for router-to-router connections
- **/24 subnets** for PC connections

---

## 🏢 Network Structure:

### 🖥️ PCs:
- **PC1** connected to Router R1
- **PC2** connected to Router R5

### 🌐 Routers:
- **R1** to **R5** (configured with OSPF routing in Area 0)

---

## 🌐 IP Addressing & Subnetting:

- **Main Network:**  
  - **Router-to-Router links:** `/30 subnet (10.0.x.x)`
  - **PC connections:** `/24 subnet (192.168.x.x)`
  
### IP Address Table:

| Device | Interface | Connects To  | IP Address       | Subnet Mask     |
|--------|-----------|--------------|------------------|-----------------|
| PC1    | f0/0      | R1 f0/0      | 192.168.10.10    | 255.255.255.0   |
| R1     | f0/0      | PC1 f0/0     | 192.168.10.1     | 255.255.255.0   |
| R1     | f1/0      | R2 f0/0      | 10.0.12.1        | 255.255.255.252 |
| R2     | f0/0      | R1 f1/0      | 10.0.12.2        | 255.255.255.252 |
| R2     | f1/0      | R3 f0/0      | 10.0.23.1        | 255.255.255.252 |
| R3     | f0/0      | R2 f1/0      | 10.0.23.2        | 255.255.255.252 |
| R3     | f1/0      | R4 f0/0      | 10.0.34.1        | 255.255.255.252 |
| R4     | f0/0      | R3 f1/0      | 10.0.34.2        | 255.255.255.252 |
| R4     | f1/0      | R5 f0/0      | 10.0.45.1        | 255.255.255.252 |
| R5     | f0/0      | R4 f1/0      | 10.0.45.2        | 255.255.255.252 |
| R5     | f1/0      | PC2 f0/0     | 192.168.20.1     | 255.255.255.0   |
| PC2    | f0/0      | R5 f1/0      | 192.168.20.10    | 255.255.255.0   |

---

## 🛠️ Step-by-Step Configuration

### 🔌 1. Physical Setup in Cisco Packet Tracer
- Drag and drop:
  - 5 Routers (R1 to R5)
  - 2 PCs (PC1 and PC2)
  - 5 Copper Straight-Through Cables for connecting devices

---

### 🌐 2. Configure the Router

####  PC1 (Simulated using Router) Configuration:
```bash
conf t
interface f0/0
 ip address 192.168.10.10 255.255.255.0
 no shut
exit
ip route 0.0.0.0 0.0.0.0 192.168.10.1

```
#### R1 Configuration
```bash
conf t
interface f0/0
 ip address 192.168.10.1 255.255.255.0
 no shut
interface f1/0
 ip address 10.0.12.1 255.255.255.252
 no shut
router ospf 1
 network 192.168.10.0 0.0.0.255 area 0
 network 10.0.12.0 0.0.0.3 area 0

```
#### R3 Configuration
```bash
conf t
interface f0/0
 ip address 10.0.23.2 255.255.255.252
 no shut
interface f1/0
 ip address 10.0.34.1 255.255.255.252
 no shut
router ospf 1
 network 10.0.23.0 0.0.0.3 area 0
 network 10.0.34.0 0.0.0.3 area 0

```
#### R4 Configuration
```bash
conf t
interface f0/0
 ip address 10.0.34.2 255.255.255.252
 no shut
interface f1/0
 ip address 10.0.45.1 255.255.255.252
 no shut
router ospf 1
 network 10.0.34.0 0.0.0.3 area 0
 network 10.0.45.0 0.0.0.3 area 0

```
#### R5 Configuration
```bash
conf t
interface f0/0
 ip address 10.0.45.2 255.255.255.252
 no shut
interface f1/0
 ip address 192.168.20.1 255.255.255.0
 no shut
router ospf 1
 network 10.0.45.0 0.0.0.3 area 0
 network 192.168.20.0 0.0.0.255 area 0

```
####  PC2 (Simulated using Router) Configuration:
```bash
conf t
interface f0/0
 ip address 192.168.20.10 255.255.255.0
 no shut
exit
ip route 0.0.0.0 0.0.0.0 192.168.20.1

```

## ✅ FINAL TESTING:

### From PC1, try:
```bash
ping 192.168.20.10
```

### From any router::
```bash
show ip route
show ip ospf neighbor
```