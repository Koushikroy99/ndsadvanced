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

### 2. OSPF Multi Area Lab

<p align="center">
    <img src="./2. OSPF Multi Area Lab.png" alt="OSPF Multi Area Lab">
</p>

<details>
<summary><strong>⚙️ Steps to Configure OSPF Multi-Area Routing (7 Routers)</strong></summary>

<br>

## 🧩 Network Topology:
- **7 Routers** (R1 to R7)
- OSPF divided into **3 areas**:
  - **Area 1:** R1 ↔ R2 ↔ R3
  - **Area 0 (Backbone):** R3 ↔ R4 ↔ R5
  - **Area 2:** R5 ↔ R6 ↔ R7
- ABRs: **R3** and **R5**

---

## 🌐 IP Addressing & Subnetting:

| Router | Interface | IP Address     | Area   |
|--------|-----------|----------------|--------|
| R1     | f0/0      | 10.1.1.10/24   | Area 1 |
| R2     | f0/0      | 10.1.1.20/24   | Area 1 |
| R2     | f1/0      | 20.1.1.20/24   | Area 1 |
| R3     | f0/0      | 20.1.1.10/24   | Area 1 |
| R3     | f1/0      | 30.1.1.10/24   | Area 0 |
| R4     | f0/0      | 30.1.1.20/24   | Area 0 |
| R4     | f1/0      | 40.1.1.20/24   | Area 0 |
| R5     | f0/0      | 40.1.1.10/24   | Area 0 |
| R5     | f1/0      | 50.1.1.10/24   | Area 2 |
| R6     | f0/0      | 50.1.1.20/24   | Area 2 |
| R6     | f1/0      | 60.1.1.20/24   | Area 2 |
| R7     | f0/0      | 60.1.1.10/24   | Area 2 |

---

## 🛠️ Step-by-Step Configuration

### 🔌 1. Physical Setup in GNS3
- 🧱 Devices Required:
- Drag and drop:
  - 5 Cisco Routers (e.g., Cisco 7200 or 3725 with appropriate IOS)
  - Ethernet connections between routers
---

### 🔧 R1 Configuration

```bash
conf t
interface f0/0
 ip address 10.1.1.10 255.255.255.0
 no shut
router ospf 1
 network 10.1.1.0 0.0.0.255 area 1

```
### 🔧 R2 Configuration
```bash
conf t
interface f0/0
 ip address 10.1.1.20 255.255.255.0
 no shut
interface f1/0
 ip address 20.1.1.20 255.255.255.0
 no shut
router ospf 1
 network 10.1.1.0 0.0.0.255 area 1
 network 20.1.1.0 0.0.0.255 area 1

```
### 🔧 R3 Configuration (ABR)
```bash
conf t
interface f0/0
 ip address 20.1.1.10 255.255.255.0
 no shut
interface f1/0
 ip address 30.1.1.10 255.255.255.0
 no shut
router ospf 1
 network 20.1.1.0 0.0.0.255 area 1
 network 30.1.1.0 0.0.0.255 area 0

```
### 🔧 R4 Configuration
```bash
conf t
interface f0/0
 ip address 30.1.1.20 255.255.255.0
 no shut
interface f1/0
 ip address 40.1.1.20 255.255.255.0
 no shut
router ospf 1
 network 30.1.1.0 0.0.0.255 area 0
 network 40.1.1.0 0.0.0.255 area 0

```
### 🔧 R5 Configuration (ABR)
```bash
conf t
interface f0/0
 ip address 40.1.1.10 255.255.255.0
 no shut
interface f1/0
 ip address 50.1.1.10 255.255.255.0
 no shut
router ospf 1
 network 40.1.1.0 0.0.0.255 area 0
 network 50.1.1.0 0.0.0.255 area 2

```
### 🔧 R6 Configuration
```bash
conf t
interface f0/0
 ip address 50.1.1.20 255.255.255.0
 no shut
interface f1/0
 ip address 60.1.1.20 255.255.255.0
 no shut
router ospf 1
 network 50.1.1.0 0.0.0.255 area 2
 network 60.1.1.0 0.0.0.255 area 2

```

### 🔧 R7 Configuration
```bash
conf t
interface f0/0
 ip address 60.1.1.10 255.255.255.0
 no shut
router ospf 1
 network 60.1.1.0 0.0.0.255 area 2

```

## ✅ FINAL TESTING:

### From any router::
```bash
ping <Other router IP>
show ip route
show ip ospf neighbor
```