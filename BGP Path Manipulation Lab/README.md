<p align="center">
    <img src="./cisco-logo.png" alt="Logo" width="200">
</p>

<h2 align="center"> NDS Advanced GNS3 Projects (Sunny Sir - NDS Lab)</h2>

---

## 📝About this project

This project demonstrates BGP path manipulation to influence shortest path selection. It includes configuration of BGP attributes such as AS Path and Local Preference to control route preference and achieve desired routing behavior.


## Networking Labs

### 7. BGP Path Manipulation Lab

<p align="center">
    <img src="./7. BGP Path Manipulation.png" alt="7. BGP Path Manipulation.png">
</p>

<details>
<summary><strong>⚙️ BGP Path Manipulation Configuration</strong></summary>

<br>

## 🧩 Network Topology:
| Router | Loopback   | Interfaces with IPs                                                                      | AS  |
| ------ | ---------- | ---------------------------------------------------------------------------------------- | --- |
| R1     | 1.1.1.1/24 | f0/0 → 10.1.1.10/24<br>f1/0 → 20.1.1.10/24                                               | 100 |
| R2     | 2.2.2.2/24 | f0/0 → 60.1.1.10/24<br>f1/0 → 30.1.1.10/24<br>f1/1 → 10.1.1.20/24<br>f2/0 → 50.1.1.10/24 | 100 |
| R3     | 3.3.3.3/24 | f0/0 → 40.1.1.10/24<br>f1/0 → 30.1.1.20/24<br>f1/1 → 20.1.1.20/24                        | 100 |
| R4     | 4.4.4.4/24 | f0/0 → 60.1.1.20/24<br>f1/0 → 70.1.1.10/24                                               | 200 |
| R5     | 5.5.5.5/24 | f0/0 → 70.1.1.20/24<br>f1/0 → 40.1.1.20/24<br>f1/1 → 50.1.1.20/24                        | 300 |
---

## 🎯 Lab Goal: BGP Path Manipulation (Shortest Path Preference)
Scenario:
To force traffic to prefer R3 → R5 over R2 → R5, add AS-PATH prepending on R2

## 🛠️ Step-by-Step Configuration

### 🔌 1. Physical Setup in GNS3
- 🧱 Devices Required:
- Drag and drop:
  - 5 Cisco Routers (e.g., Cisco 7200 or 3725 with appropriate IOS)
  - Connections between routers
---

### 🔧 R1 (AS 100) Configuration 

```bash
conf t
interface Loopback1
 ip address 1.1.1.1 255.255.255.0
  no shutdown
interface f0/0
 ip address 10.1.1.10 255.255.255.0
  no shutdown
interface f1/0
 ip address 20.1.1.10 255.255.255.0
  no shutdown
router bgp 100
 bgp log-neighbor-changes
 network 1.1.1.0 mask 255.255.255.0
 neighbor 10.1.1.20 remote-as 100
 neighbor 20.1.1.20 remote-as 100

```
### 🔧 R2 (AS 100) Configuration 
```bash
conf t
interface Loopback1
 ip address 2.2.2.2 255.255.255.0
 no shutdown

interface f0/0
 ip address 60.1.1.10 255.255.255.0
 no shutdown

interface f1/0
 ip address 30.1.1.10 255.255.255.0
 no shutdown

interface f1/1
 ip address 10.1.1.20 255.255.255.0
 no shutdown

interface f2/0
 ip address 50.1.1.10 255.255.255.0
 no shutdown

router bgp 100
 bgp log-neighbor-changes
 network 2.2.2.0 mask 255.255.255.0
 neighbor 10.1.1.10 remote-as 100
 neighbor 30.1.1.20 remote-as 100
 neighbor 50.1.1.20 remote-as 300
 neighbor 60.1.1.20 remote-as 200

```
### 🔧 R3 (AS 100) Configuration
```bash
conf t
interface Loopback1
 ip address 3.3.3.3 255.255.255.0
 no shutdown

interface f0/0
 ip address 40.1.1.10 255.255.255.0
 no shutdown

interface f1/0
 ip address 30.1.1.20 255.255.255.0
 no shutdown

interface f1/1
 ip address 20.1.1.20 255.255.255.0
 no shutdown

router bgp 100
 bgp log-neighbor-changes
 network 3.3.3.0 mask 255.255.255.0
 neighbor 20.1.1.10 remote-as 100
 neighbor 30.1.1.10 remote-as 100
 neighbor 40.1.1.20 remote-as 300

```
### 🔧 R4 (AS 200) Configuration
```bash
conf t
interface Loopback1
 ip address 4.4.4.4 255.255.255.0
 no shutdown

interface f0/0
 ip address 60.1.1.20 255.255.255.0
 no shutdown

interface f1/0
 ip address 70.1.1.10 255.255.255.0
 no shutdown

router bgp 200
 bgp log-neighbor-changes
 network 4.4.4.0 mask 255.255.255.0
 neighbor 60.1.1.10 remote-as 100
 neighbor 70.1.1.20 remote-as 300

```
### 🔧 R5 (AS 300) Configuration
```bash
conf t
interface Loopback1
 ip address 5.5.5.5 255.255.255.0
 no shutdown

interface f0/0
 ip address 70.1.1.20 255.255.255.0
 no shutdown

interface f1/0
 ip address 40.1.1.20 255.255.255.0
 no shutdown

interface f1/1
 ip address 50.1.1.20 255.255.255.0
 no shutdown

router bgp 300
 bgp log-neighbor-changes
 network 5.5.5.0 mask 255.255.255.0
 neighbor 40.1.1.10 remote-as 100
 neighbor 50.1.1.10 remote-as 100
 neighbor 70.1.1.10 remote-as 200

```

### 🔧 Path Manipulation (AS-PATH Prepending Example) Configuration
```bash
router bgp 100
 neighbor 50.1.1.20 route-map PREPEND_OUT out
!
route-map PREPEND_OUT permit 10
 set as-path prepend 100 100 100

```
To force traffic to prefer R3 → R5 over R2 → R5, add AS-PATH prepending on R2.
R5 see the path from R2 as longer (AS 100 repeated 3 times), and prefer the shorter path via R3.
