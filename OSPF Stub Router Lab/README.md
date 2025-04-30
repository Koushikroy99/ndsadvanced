<p align="center">
    <img src="./cisco-logo.png" alt="Logo" width="200">
</p>

<h2 align="center"> NDS Advanced GNS3 Projects (Sunny Sir - NDS Lab)</h2>

---

## 📝About this project

This lab demonstrates the behavior and use case of a Stub Router in OSPF, which is not the same as a Stub Area.

The lab uses a 5-router topology (R1–R5) connected in a full mesh where OSPF Area 0 is configured. One router (R2) is configured as a stub router using the max-metric router-lsa command. This makes other routers avoid using R2 as a transit path while still maintaining its OSPF adjacency and participating in LSDB exchange.

This feature is commonly used in the following real-world scenarios:

When a router is undergoing maintenance

During network migrations

When temporary failover behavior is needed

For reducing transit traffic load on a particular router


---
## Networking Labs

### 5. OSPF Stub Router Lab

<p align="center">
    <img src="./5. OSPF Stub Router Lab.png" alt="5. OSPF Stub Router Lab">
</p>

<details>
<summary><strong>⚙️ OSPF Configuration - 3 Areas + Virtual Link</strong></summary>

<br>

## 🧩 Network Topology:
Implement full OSPF routing between R1, R2, R3, R4, R5

Make R2 act as a Stub Router using max-metric router-lsa

Observe how other routers avoid R2 for transit

---

## 🌐 IP Addressing & Subnetting:

| Link     | Interface 1         | IP            | Interface 2         | IP            | Subnet         |
|----------|---------------------|---------------|---------------------|---------------|----------------|
| R1–R2    | R1 f0/0             | 10.0.12.1      | R2 f0/0             | 10.0.12.2      | 255.255.255.0  |
| R2–R3    | R2 f1/0             | 10.0.23.1      | R3 f0/0             | 10.0.23.2      | 255.255.255.0  |
| R3–R4    | R3 f1/0             | 10.0.34.1      | R4 f0/0             | 10.0.34.2      | 255.255.255.0  |
| R4–R5    | R4 f1/0             | 10.0.45.1      | R5 f0/0             | 10.0.45.2      | 255.255.255.0  |
| R5–R1    | R5 f1/0             | 10.0.15.1      | R1 f1/0             | 10.0.15.2      | 255.255.255.0  |
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
 ip address 10.0.12.1 255.255.255.0
 no shutdown
interface f1/0
 ip address 10.0.15.2 255.255.255.0
 no shutdown
router ospf 1
 router-id 1.1.1.1
 network 10.0.0.0 0.255.255.255 area 0

```
### 🔧 R2 Configuration (Stub Router)
```bash
conf t
interface f0/0
 ip address 10.0.12.2 255.255.255.0
 no shutdown
interface f1/0
 ip address 10.0.23.1 255.255.255.0
 no shutdown
router ospf 1
 router-id 2.2.2.2
 network 10.0.0.0 0.255.255.255 area 0
 max-metric router-lsa

```
### 🔧 R3 Configuration 
```bash
conf t
interface f0/0
 ip address 10.0.23.2 255.255.255.0
 no shutdown
interface f1/0
 ip address 10.0.34.1 255.255.255.0
 no shutdown
router ospf 1
 router-id 3.3.3.3
 network 10.0.0.0 0.255.255.255 area 0

```
### 🔧 R4 Configuration
```bash
conf t
interface f0/0
 ip address 10.0.34.2 255.255.255.0
 no shutdown
interface f1/0
 ip address 10.0.45.1 255.255.255.0
 no shutdown
router ospf 1
 router-id 4.4.4.4
 network 10.0.0.0 0.255.255.255 area 0

```
### 🔧 R5 Configuration (ABR)
```bash
conf t
interface f0/0
 ip address 10.0.45.2 255.255.255.0
 no shutdown
interface f1/0
 ip address 10.0.15.1 255.255.255.0
 no shutdown
router ospf 1
 router-id 5.5.5.5
 network 10.0.0.0 0.255.255.255 area 0

```

## ✅ FINAL TESTING:

### From any router:
```bash
show ip ospf neighbor
show ip route ospf

```
### On R2 (Check Max Metric):
```bash
show ip ospf | include Maximum

```