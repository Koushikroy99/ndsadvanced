<p align="center">
    <img src="./cisco-logo.png" alt="Logo" width="200">
</p>

<h2 align="center"> NDS Advanced GNS3 Projects (Sunny Sir - NDS Lab)</h2>

---

## 📝About this project

This lab we will learn how to enable MPLS in a network, how one router builds an LDP neighbor relationship with another router, and how they exchange MPLS labels to forward packets based on labels.

In an MPLS-enabled network, routers are usually divided into three types:

Customer Edge Router (CE): This is the client’s router. MPLS is not enabled on it.

Provider Edge Router (PE): This is the ISP’s edge router (POP router). CE routers are usually connected to this. MPLS is enabled on PE routers.

Provider Router (P): This is also an ISP router, usually working as a core router. MPLS must be enabled on it. P routers connect with PE routers, but not directly with any CE router.


---
## Networking Labs

### MPLS and LDP Neighbor Relationship

<p align="center">
    <img src="./MPLS and LDP Neighbor Relationship.png" alt="MPLS and LDP Neighbor Relationship Lab">
</p>

<details>
<summary><strong>⚙️ MPLS and LDP Neighbor Relationship Configuration</strong></summary>

<br>

## 🧩 Network Topology:
According to our network topology, the client routers C1-A and C1-B are CE routers, the ISP’s R1 and R4 are PE routers, and R2 and R3 are P routers.
---

## 🌐 IP Addressing & Loopback :

| Link    | Interface 1 | IP       | Interface 2 | IP       | Subnet        |
| ------- | ----------- | -------- | ----------- | -------- | ------------- |
| C1-A–R1 | C1-A f0/0   | 10.0.0.2 | R1 f0/0     | 10.0.0.1 | 255.255.255.0 |
| R1–R2   | R1 f1/0     | 20.0.0.1 | R2 f0/0     | 20.0.0.2 | 255.255.255.0 |
| R2–R3   | R2 f1/0     | 30.0.0.1 | R3 f0/0     | 30.0.0.2 | 255.255.255.0 |
| R3–R4   | R3 f1/0     | 40.0.0.1 | R4 f0/0     | 40.0.0.2 | 255.255.255.0 |
| R4–C1-B | R4 f1/0     | 50.0.0.1 | C1-B f0/0   | 50.0.0.2 | 255.255.255.0 |

---
| Router | Loopback Interface | IP Address | Subnet        |
| ------ | ------------------ | ---------- | ------------- |
| R1     | Loopback0          | 1.1.1.1/24 | 255.255.255.0 |
| R2     | Loopback0          | 2.2.2.2/24 | 255.255.255.0 |
| R3     | Loopback0          | 3.3.3.3/24 | 255.255.255.0 |
| R4     | Loopback0          | 4.4.4.4/24 | 255.255.255.0 |


## 🛠️ Step-by-Step Configuration

### 🔌 1. Physical Setup in GNS3
- 🧱 Devices Required:
- Drag and drop:
  - 6 Cisco Routers (e.g., Cisco 7200 or 3725 with appropriate IOS)
  - Ethernet connections between routers
---

### 🔧 R1 Configuration

```bash
R1#conf t
R1(config)#interface fastEthernet 0/0
R1(config-if)#ip address 10.0.0.1 255.255.255.0
R1(config-if)#no shutdown
R1(config-if)#exit
R1(config)#interface fastEthernet 1/0
R1(config-if)#ip address 20.0.0.2 255.255.255.0
R1(config-if)#no shutdown
R1(config-if)#exit
R1(config)#interface loopback 1
R1(config-if)#ip address 1.1.1.1 255.255.255.0
R1(config-if)#exit
```
### 🔧 R2 Configuration (Stub Router)
```bash
R2#conf t
R2(config)#interface fastEthernet 0/0
R2(config-if)#ip address 20.0.0.1 255.255.255.0
R2(config-if)#no shutdown
R2(config-if)#exit
R2(config)#interface fastEthernet 1/0
R2(config-if)#ip address 30.0.0.1 255.255.255.0
R2(config-if)#no shutdown
R2(config-if)#exit
R2(config)#interface loopback 1
R2(config-if)#ip address 2.2.2.2 255.255.255.0
R2(config-if)#exit
```
### 🔧 R3 Configuration 
```bash
R3#conf t
R3(config)#interface fastEthernet 0/0
R3(config-if)#ip address 30.0.0.2 255.255.255.0
R3(config-if)#no shutdown
R3(config-if)#exit
R3(config)#interface fastEthernet 1/0
R3(config-if)#ip address 40.0.0.1 255.255.255.0
R3(config-if)#no shutdown
R3(config-if)#exit
R3(config)#interface loopback 1
R3(config-if)#ip address 3.3.3.3 255.255.255.0
R3(config-if)#exit
```
### 🔧 R4 Configuration
```bash
R4#conf t
R4(config)#interface fastEthernet 0/0
R4(config-if)#ip address 40.0.0.2 255.255.255.0
R4(config-if)#no shutdown
R4(config-if)#exit
R4(config)#interface fastEthernet 1/0
R4(config-if)#ip address 50.0.0.1 255.255.255.0
R4(config-if)#no shutdown
R4(config-if)#exit
R4(config)#interface loopback 1
R4(config-if)#ip address 4.4.4.4 255.255.255.0
R4(config-if)#exit
```
### 🔧 C1-A Configuration
```bash
C1-A#conf t
C1-A(config)#interface fastEthernet 0/0
C1-A(config-if)#ip address 10.0.0.2 255.255.255.0
C1-A(config-if)#no shutdown
C1-A(config-if)#exit
```
### 🔧 C1-B Configuration
```bash
C1-B#conf t
C1-B(config)#interface fastEthernet 0/0
C1-B(config-if)#ip address 50.0.0.2 255.255.255.0
C1-B(config-if)#no shutdown
C1-B(config-if)#exit
```

Now we will configure OSPF as the IGP (Interior Gateway Protocol) among the ISP routers. There will be no OSPF configuration on the client routers.

### 🔧 R1 Configuration

```bash
R1#conf t
R1(config)#router ospf 1
R1(config-router)#network 10.0.0.0 0.0.0.255 area 10
R1(config-router)#network 20.0.0.0 0.0.0.255 area 10
R1(config-router)#network 1.1.1.1 0.0.0.255 area 10
R1(config-router)#passive-interface fastEthernet 0/0
R1(config-router)#exit
```

### 🔧 R2 Configuration

```bash
R2#conf t
R2(config)#router ospf 1
R2(config-router)#network 20.0.0.0 0.0.0.255 area 10
R2(config-router)#network 30.0.0.0 0.0.0.255 area 10
R2(config-router)#network 2.2.2.2 0.0.0.255 area 10
R2(config-router)#exit
```

### 🔧 R3 Configuration

```bash
R3#conf t
R3(config)#router ospf 1
R3(config-router)#network 30.0.0.0 0.0.0.255 area 10
R3(config-router)#network 40.0.0.0 0.0.0.255 area 10
R3(config-router)#network 3.3.3.3 0.0.0.255 area 10
R3(config-if)#exit
```

### 🔧 R4 Configuration

```bash
R4#conf t
R4(config)#router ospf 1
R4(config-router)#network 40.0.0.0 0.0.0.255 area 10
R4(config-router)#network 50.0.0.0 0.0.0.255 area 10
R4(config-router)#network 4.4.4.0 0.0.0.255 area 10
R4(config-router)#passive-interface fastEthernet 1/0
R4(config-if)#exit
```

## ✅ Configuring MPLS on ISP Router:

### 🔧 R1 Configuration

```bash
R1#conf t
R1(config)#mpls ip
R1(config)#interface fastEthernet 1/0
R1(config-if)#mpls ip
```
### 🔧 R2 Configuration

```bash
R2#conf t
R2(config)#mpls ip
R2(config)#int fastEthernet 0/0
R2(config-if)#mpls ip
R2(config-if)#exit
R2(config)#int fastEthernet 1/0
R2(config-if)#mpls ip
```
### 🔧 R3 Configuration

```bash
R3#conf t
R3(config)#mpls ip
R3(config)#int fastEthernet 0/0
R3(config-if)#mpls ip
R3(config-if)#exit
R3(config)#int fastEthernet 1/0
R3(config-if)#mpls ip
```
### 🔧 R4 Configuration

```bash
R4#conf t
R4(config)#mpls ip
R4(config)#interface fastEthernet 0/0
R4(config-if)#mpls ip
```
