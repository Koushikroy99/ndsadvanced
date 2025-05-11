<p align="center">
    <img src="./cisco-logo.png" alt="Logo" width="200">
</p>

<h2 align="center"> NDS Advanced GNS3 Projects (Sunny Sir - NDS Lab)</h2>

---

## 📝About this project

NG is a multi-branch enterprise headquartered in Delhi (AS 200), with branches in Hyderabad (AS 100) and Kolkata (AS 300). The company maintains a centralized networking strategy where Delhi serves as the main branch, operating an internal OSPF domain. The BGP infrastructure uses Route Reflector (RR) methodology to simplify iBGP peering.

Main Branch (Delhi) – AS 200 (R2, R3, R4, R5)

Hyderabad Branch – AS 100 (R1)

Kolkata Branch – AS 300 (R6)

R4 is the Route Reflector (RR) in AS 200

Internal Routing Protocol in AS 200 is OSPF


---
## Networking Labs

### 6. BGP RR Lab

<p align="center">
    <img src="./6. BGP RR Lab.png" alt="6. BGP RR Lab">
</p>

<details>
<summary><strong>⚙️ BGP RR Configuration</strong></summary>

<br>

## 🧩 Network Topology:
Run OSPF between R2, R3, R4, and R5

Configure R4 as Route Reflector

Use iBGP among internal routers

Redistribute OSPF into BGP as needed

---


## 🛠️ Step-by-Step Configuration

### 🔌 1. Physical Setup in GNS3
- 🧱 Devices Required:
- Drag and drop:
  - 6 Cisco Routers (e.g., Cisco 7200 or 3725 with appropriate IOS)
  - Ethernet connections between routers
---

### 🔧 R1 (Hyderabad - AS 100) Configuration 

```bash
conf t
interface f0/0
 ip address 10.1.1.10 255.255.255.0
 no shutdown

interface loopback0
 ip address 1.1.1.1 255.255.255.0

router bgp 100
 network 1.1.1.0 mask 255.255.255.0
 neighbor 10.1.1.20 remote-as 200


```
### 🔧 R2 (Delhi AS 200 ) Configuration 
```bash
conf t
interface f0/0
 ip address 10.1.1.20 255.255.255.0
 no shutdown

interface f1/0
 ip address 20.1.1.10 255.255.255.0
 no shutdown

router ospf 1
 network 10.1.1.0 0.0.0.255 area 0
 network 20.1.1.0 0.0.0.255 area 0

router bgp 200
 bgp log-neighbor-changes
 neighbor 10.1.1.10 remote-as 100
 neighbor 20.1.1.20 remote-as 200

```
### 🔧 R3 (Delhi AS 200 ) Configuration
```bash
conf t
interface f0/0
 ip address 20.1.1.20 255.255.255.0
 no shutdown

interface f1/0
 ip address 30.1.1.10 255.255.255.0
 no shutdown

router ospf 1
 network 20.1.1.0 0.0.0.255 area 0
 network 30.1.1.0 0.0.0.255 area 0

router bgp 200
 bgp log-neighbor-changes
 neighbor 20.1.1.10 remote-as 200
 neighbor 30.1.1.20 remote-as 200


```
### 🔧 R4 (Delhi AS 200 ) Route Reflector Configuration
```bash
conf t
interface f0/0
 ip address 30.1.1.20 255.255.255.0
 no shutdown

interface f1/0
 ip address 40.1.1.10 255.255.255.0
 no shutdown

router ospf 1
 network 30.1.1.0 0.0.0.255 area 0
 network 40.1.1.0 0.0.0.255 area 0

route-map NG permit 10
 match ip address 1

router bgp 200
 bgp log-neighbor-changes
 bgp cluster-id 4.4.4.4
 neighbor 30.1.1.10 remote-as 200
 neighbor 40.1.1.20 remote-as 200
 neighbor 30.1.1.10 route-reflector-client
 neighbor 40.1.1.20 route-reflector-client

```
### 🔧 R5 (Delhi AS 200 ) Edge Router Configuration
```bash
conf t
interface f0/0
 ip address 40.1.1.20 255.255.255.0
 no shutdown

interface f1/0
 ip address 50.1.1.10 255.255.255.0
 no shutdown

router ospf 1
 network 40.1.1.0 0.0.0.255 area 0
 network 50.1.1.0 0.0.0.255 area 0

router bgp 200
 bgp log-neighbor-changes
 neighbor 40.1.1.10 remote-as 200
 neighbor 50.1.1.20 remote-as 300

```

### 🔧 R6 (Kolkata AS 300 ) Configuration
```bash
conf t
hostname R6
interface f0/0
 ip address 50.1.1.20 255.255.255.0
 no shutdown

interface loopback0
 ip address 2.2.2.2 255.255.255.0

router bgp 300
 network 2.2.2.0 mask 255.255.255.0
 neighbor 50.1.1.10 remote-as 200

```
