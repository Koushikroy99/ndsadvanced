<p align="center">
    <img src="./cisco-logo.png" alt="Logo" width="200">
</p>

<h2 align="center"> NDS Advanced GNS3 Projects (Sunny Sir - NDS Lab)</h2>

---

## 📝About this project

Welcome to my Enterprise Switching Lab with VTP, VLANs & Pruning — designed to simulate real-world Layer 2 network designs in a structured, multi-floor enterprise environment.

This GNS3-based lab demonstrates advanced switching concepts like VLAN segmentation, VTP (VLAN Trunking Protocol) deployment in different modes, and VTP Pruning for optimizing broadcast domain efficiency across trunk links.


---
## Networking Labs

### Switch Lab

<p align="center">
    <img src="./Switch.png" alt="Switch Lab">
</p>

<details>
<summary><strong>⚙️ OSPF Configuration - VLAN, Trunk, VTP & Pruning</strong></summary>

<br>

## 🧩 Network Topology:
- **4 Routers** (R1 to R7)
- OSPF divided into **3 areas**:
  - **Area 2:** R1 ↔ R2 ↔ R3
  - **Area 1 (Transit Area):** R3 ↔ R4 ↔ R5
  - **Area 0:** R5 ↔ R6 ↔ R7
- ABRs: **R3** and **R5**

---

## 🌐 Network Plan Summary:

| Switch         | VTP Mode    | VLANs Needed                                               | Trunk Links                                        |
| -------------- | ----------- | ---------------------------------------------------------- | -------------------------------------------------- |
| **SW-1ST-FLR** | **Server**  | 10 (IT), 20 (HR), 30 (Finance), 40 (Sales), 50 (Marketing) | e0 to SW-2ND-FLR <br> e1 to PCs <br> e2 to PCs     |
| **SW-2ND-FLR** | Transparent | Local VLAN 40 (Sales)                                      | e0 trunk to SW-1ST-FLR <br> e2 trunk to SW-3RD-FLR |
| **SW-3RD-FLR** | Transparent | Local VLAN 50 (Marketing)                                  | e0 trunk to SW-2ND-FLR <br> e2 trunk to SW-4TH-FLR |
| **SW-4TH-FLR** | Client      | Learns VLANs from Server                                   | e0 trunk to SW-3RD-FLR <br> e1/e2 access to PCs    |


---

## 🛠️ Step-by-Step Configuration

### 🔌 1. Physical Setup in GNS3
- 🧱 Devices Required:
- Drag and drop:
  4 Cisco Switches (L2 Switch or IOU L2 images or GNS3 Layer 2 Switch)

SW-1ST-FLR (VTP Server)

SW-2ND-FLR (Transparent)

SW-3RD-FLR (Transparent)

SW-4TH-FLR (VTP Client)

6 End Devices (VPCS or Virtual PCs)

IT-PC1, HR-PC1, SALES-PC1, MARK-PC1, IT-PC2, FINANCE-PC1
---

### 🔧 SW-1ST-FLR (VTP Server) Configuration

```bash
Switch(config)# vtp domain NG
Switch(config)# vtp mode server
Switch(config)# vtp password NG123

! Create VLANs
Switch(config)# vlan 10
Switch(config-vlan)# name IT
Switch(config)# vlan 20
Switch(config-vlan)# name HR
Switch(config)# vlan 30
Switch(config-vlan)# name Finance
Switch(config)# vlan 40
Switch(config-vlan)# name Sales
Switch(config)# vlan 50
Switch(config-vlan)# name Marketing

! Trunk configuration
Switch(config)# interface e0
Switch(config-if)# switchport mode trunk
Switch(config-if)# interface e2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20
Switch(config-if)# interface e1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10

```
### 🔧 SW-2ND-FLR (VTP Transparent) Configuration
```bash
Switch(config)# vtp domain NG
Switch(config)# vtp mode transparent
Switch(config)# vtp password NG123

! Locally create VLAN 40
Switch(config)# vlan 40
Switch(config-vlan)# name Sales

! Trunk configuration
Switch(config)# interface e0
Switch(config-if)# switchport mode trunk
Switch(config-if)# interface e2
Switch(config-if)# switchport mode trunk
Switch(config-if)# interface e1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 40

```
### 🔧 SW-3RD-FLR (VTP Transparent) Configuration
```bash
Switch(config)# vtp domain NG
Switch(config)# vtp mode transparent
Switch(config)# vtp password NG123

! Locally create VLAN 50
Switch(config)# vlan 50
Switch(config-vlan)# name Marketing

! Trunk configuration
Switch(config)# interface e0
Switch(config-if)# switchport mode trunk
Switch(config-if)# interface e2
Switch(config-if)# switchport mode trunk
Switch(config-if)# interface e1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 50

```
### 🔧 SW-4TH-FLR (VTP Client) Configuration
```bash
Switch(config)# vtp domain MyCompany
Switch(config)# vtp mode client
Switch(config)# vtp password MyVTPpass

! Trunk configuration
Switch(config)# interface e0
Switch(config-if)# switchport mode trunk
Switch(config-if)# interface e1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# interface e2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 30

```


### 🔧 VTP Pruning Configuration Configuration 
```bash
SW-1ST-FLR(config)# vtp pruning

```
SW-2ND-FLR (Sales, VLAN 40) → doesn’t need traffic for VLAN 10, 20, 30, 50

SW-3RD-FLR (Marketing, VLAN 50) → doesn’t need traffic for VLAN 10, 20, 30, 40

By pruning, these switches won’t get traffic for VLANs they don’t use.

## ✅ FINAL TESTING:

### Verify VTP & VLANs:

show vtp status → Check mode, domain, revision

show vlan brief → Check VLANs on all switches

show interfaces trunk → Verify trunk ports

SW-1ST-FLR# show vtp status

