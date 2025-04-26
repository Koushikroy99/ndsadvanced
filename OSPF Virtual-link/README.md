<p align="center">
    <img src="./cisco-logo.png" alt="Logo" width="200">
</p>

<h2 align="center"> NDS Advanced GNS3 Projects (Sunny Sir - NDS Lab)</h2>

---

## 📝About this project

Welcome to my OSPF Multi-Area + Virtual Link GNS3 Project Repository, inspired by real-world enterprise network design practices.
This GNS3 project demonstrates OSPF Multi-Area Routing using 7 Cisco routers connected across three different OSPF areas — Area 0 (Backbone), Area 1 (Transit Area), and Area 2 (Standard Area).

The lab focuses on:

OSPF Area segmentation and hierarchical design.

Manual Router-ID configuration for ABRs (R3 and R5).

Virtual Link implementation between non-directly connected areas (Area 2 to Area 0 via Area 1).

Seamless OSPF neighbor adjacency across all routers.


---
## Networking Labs

### 2. OSPF Multi Area Lab

<p align="center">
    <img src="./3. OSPF Virtual-link.png" alt="3. OSPF Virtual-link">
</p>

<details>
<summary><strong>⚙️ OSPF Configuration - 3 Areas + Virtual Link</strong></summary>

<br>

## 🧩 Network Topology:
- **7 Routers** (R1 to R7)
- OSPF divided into **3 areas**:
  - **Area 2:** R1 ↔ R2 ↔ R3
  - **Area 1 (Transit Area):** R3 ↔ R4 ↔ R5
  - **Area 0:** R5 ↔ R6 ↔ R7
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
  - 7 Cisco Routers (e.g., Cisco 7200 or 3725 with appropriate IOS)
  - Ethernet connections between routers
---

### 🔧 R1 Configuration

```bash
conf t
interface f0/0
 ip address 10.1.1.10 255.255.255.0
 no shut
router ospf 1
network 10.1.1.10 0.0.0.0 area 2

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
network 10.1.1.20 0.0.0.0 area 2
network 20.1.1.20 0.0.0.0 area 2

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
router-id 1.1.1.1
network 20.1.1.10 0.0.0.0 area 2
network 30.1.1.10 0.0.0.0 area 1

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
network 30.1.1.20 0.0.0.0 area 1
network 40.1.1.20 0.0.0.0 area 1
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
router-id 2.2.2.2
network 40.1.1.10 0.0.0.0 area 1
network 50.1.1.10 0.0.0.0 area 0
area 1 virtual-link 1.1.1.1

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
network 50.1.1.20 0.0.0.0 area 0
network 60.1.1.20 0.0.0.0 area 0

```

### 🔧 R7 Configuration
```bash
conf t
interface f0/0
 ip address 60.1.1.10 255.255.255.0
 no shut
router ospf 1
network 60.1.1.10 0.0.0.0 area 0

```

## ✅ FINAL TESTING:

### From any router::
```bash
ping <Other Router's IP Address>      # Test end-to-end reachability
show ip route                         # Verify OSPF learned routes
show ip ospf neighbor                 # Confirm OSPF neighbor relationships
```
