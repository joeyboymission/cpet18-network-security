# Activity 2 - VLAN Configuration in One Access Port

![[Pasted image 20250315184306.png]]

Below is the revised and reformatted version of **Activity 2 - VLAN Configuration in One Access Port**, following the structure and formatting style of **Activity 1** as provided in the example. The content has been adjusted for consistency, clarity, and uniformity while maintaining all the necessary technical details.

---

# Activity 2: VLAN Configuration in One Access Port

## Overview
This activity focuses on setting up a network in Cisco Packet Tracer with VLAN configurations to segment network traffic. The objectives include:
- Designing a network topology with multiple VLANs.
- Configuring VLANs on switches to isolate traffic for different departments.
- Mapping IP addresses to devices within the same subnet but across different VLANs.
- Testing connectivity within and between VLANs to verify isolation.

The network consists of:
- **1 Laptop**: Laptop1 (for console port connection and configuration only)
- **6 Switches**: UITC Main, DS1, DS2, IRTC, COS, CIT
- **47 PCs**: PC0–PC55 (distributed across IRTC, COS, and CIT switches)

The topology is a hierarchical structure with UITC Main as the core switch, connected to distribution switches (DS1, DS2) and access switches (IRTC, COS, CIT). PCs are grouped into VLANs for Accounting (ACTG), Human Resources (HR), and Computer Engineering Technology (CPET), with VLAN 1 used for minimal connectivity.

---

## IP Addressing Table
All devices are in the same subnet (192.168.150.0/24) but segmented into different VLANs. IP addresses are assigned starting from 192.168.150.2 (192.168.150.1 is reserved, assumed as a default gateway, though no router is present in the topology).

| Device      | Interface/Port | IP Address      | Subnet Mask   | Default Gateway | VLAN | Connected To       |
|-------------|----------------|-----------------|---------------|-----------------|------|--------------------|
| **UITC Main** | VLAN 1       | -               | -             | -               | 1    | -                  |
| **DS1**     | VLAN 1         | -               | -             | -               | 1    | -                  |
| **DS2**     | VLAN 1         | -               | -             | -               | 1    | -                  |
| **IRTC**    | VLAN 1         | -               | -             | -               | 1    | -                  |
| **COS**     | VLAN 1         | -               | -             | -               | 1    | -                  |
| **CIT**     | VLAN 1         | -               | -             | -               | 1    | -                  |
| **PC0**     | F0             | 192.168.150.2   | 255.255.255.0 | 192.168.150.1   | 1    | IRTC (Fa0/2)       |
| **PC14**    | F0             | 192.168.150.3   | 255.255.255.0 | 192.168.150.1   | 1    | COS (Fa0/2)        |
| **PC1**     | F0             | 192.168.150.4   | 255.255.255.0 | 192.168.150.1   | 10   | IRTC (Fa0/3)       |
| **PC2**     | F0             | 192.168.150.5   | 255.255.255.0 | 192.168.150.1   | 10   | IRTC (Fa0/4)       |
| **PC3**     | F0             | 192.168.150.6   | 255.255.255.0 | 192.168.150.1   | 10   | IRTC (Fa0/5)       |
| **PC4**     | F0             | 192.168.150.7   | 255.255.255.0 | 192.168.150.1   | 10   | IRTC (Fa0/6)       |
| **PC5**     | F0             | 192.168.150.8   | 255.255.255.0 | 192.168.150.1   | 10   | IRTC (Fa0/7)       |
| **PC8**     | F0             | 192.168.150.9   | 255.255.255.0 | 192.168.150.1   | 10   | COS (Fa0/3)        |
| **PC32**    | F0             | 192.168.150.10  | 255.255.255.0 | 192.168.150.1   | 10   | COS (Fa0/4)        |
| **PC33**    | F0             | 192.168.150.11  | 255.255.255.0 | 192.168.150.1   | 10   | COS (Fa0/5)        |
| **PC35**    | F0             | 192.168.150.12  | 255.255.255.0 | 192.168.150.1   | 10   | COS (Fa0/6)        |
| **PC34**    | F0             | 192.168.150.13  | 255.255.255.0 | 192.168.150.1   | 10   | COS (Fa0/7)        |
| **PC11**    | F0             | 192.168.150.14  | 255.255.255.0 | 192.168.150.1   | 10   | CIT (Fa0/3)        |
| **PC44**    | F0             | 192.168.150.15  | 255.255.255.0 | 192.168.150.1   | 10   | CIT (Fa0/4)        |
| **PC45**    | F0             | 192.168.150.16  | 255.255.255.0 | 192.168.150.1   | 10   | CIT (Fa0/5)        |
| **PC46**    | F0             | 192.168.150.17  | 255.255.255.0 | 192.168.150.1   | 10   | CIT (Fa0/6)        |
| **PC47**    | F0             | 192.168.150.18  | 255.255.255.0 | 192.168.150.1   | 10   | CIT (Fa0/7)        |
| **PC6**     | F0             | 192.168.150.19  | 255.255.255.0 | 192.168.150.1   | 20   | IRTC (Fa0/8)       |
| **PC24**    | F0             | 192.168.150.20  | 255.255.255.0 | 192.168.150.1   | 20   | IRTC (Fa0/9)       |
| **PC26**    | F0             | 192.168.150.21  | 255.255.255.0 | 192.168.150.1   | 20   | IRTC (Fa0/10)      |
| **PC25**    | F0             | 192.168.150.22  | 255.255.255.0 | 192.168.150.1   | 20   | IRTC (Fa0/11)      |
| **PC27**    | F0             | 192.168.150.23  | 255.255.255.0 | 192.168.150.1   | 20   | IRTC (Fa0/12)      |
| **PC9**     | F0             | 192.168.150.24  | 255.255.255.0 | 192.168.150.1   | 20   | COS (Fa0/8)        |
| **PC36**    | F0             | 192.168.150.25  | 255.255.255.0 | 192.168.150.1   | 20   | COS (Fa0/9)        |
| **PC37**    | F0             | 192.168.150.26  | 255.255.255.0 | 192.168.150.1   | 20   | COS (Fa0/10)       |
| **PC39**    | F0             | 192.168.150.27  | 255.255.255.0 | 192.168.150.1   | 20   | COS (Fa0/11)       |
| **PC38**    | F0             | 192.168.150.28  | 255.255.255.0 | 192.168.150.1   | 20   | COS (Fa0/12)       |
| **PC12**    | F0             | 192.168.150.29  | 255.255.255.0 | 192.168.150.1   | 20   | CIT (Fa0/8)        |
| **PC48**    | F0             | 192.168.150.30  | 255.255.255.0 | 192.168.150.1   | 20   | CIT (Fa0/9)        |
| **PC49**    | F0             | 192.168.150.31  | 255.255.255.0 | 192.168.150.1   | 20   | CIT (Fa0/10)       |
| **PC50**    | F0             | 192.168.150.32  | 255.255.255.0 | 192.168.150.1   | 20   | CIT (Fa0/11)       |
| **PC51**    | F0             | 192.168.150.33  | 255.255.255.0 | 192.168.150.1   | 20   | CIT (Fa0/12)       |
| **PC7**     | F0             | 192.168.150.34  | 255.255.255.0 | 192.168.150.1   | 30   | IRTC (Fa0/13)      |
| **PC28**    | F0             | 192.168.150.35  | 255.255.255.0 | 192.168.150.1   | 30   | IRTC (Fa0/14)      |
| **PC29**    | F0             | 192.168.150.36  | 255.255.255.0 | 192.168.150.1   | 30   | IRTC (Fa0/15)      |
| **PC30**    | F0             | 192.168.150.37  | 255.255.255.0 | 192.168.150.1   | 30   | IRTC (Fa0/16)      |
| **PC31**    | F0             | 192.168.150.38  | 255.255.255.0 | 192.168.150.1   | 30   | IRTC (Fa0/17)      |
| **PC10**    | F0             | 192.168.150.39  | 255.255.255.0 | 192.168.150.1   | 30   | COS (Fa0/13)       |
| **PC40**    | F0             | 192.168.150.40  | 255.255.255.0 | 192.168.150.1   | 30   | COS (Fa0/14)       |
| **PC41**    | F0             | 192.168.150.41  | 255.255.255.0 | 192.168.150.1   | 30   | COS (Fa0/15)       |
| **PC42**    | F0             | 192.168.150.42  | 255.255.255.0 | 192.168.150.1   | 30   | COS (Fa0/16)       |
| **PC43**    | F0             | 192.168.150.43  | 255.255.255.0 | 192.168.150.1   | 30   | COS (Fa0/17)       |
| **PC13**    | F0             | 192.168.150.44  | 255.255.255.0 | 192.168.150.1   | 30   | CIT (Fa0/13)       |
| **PC52**    | F0             | 192.168.150.45  | 255.255.255.0 | 192.168.150.1   | 30   | CIT (Fa0/14)       |
| **PC53**    | F0             | 192.168.150.46  | 255.255.255.0 | 192.168.150.1   | 30   | CIT (Fa0/15)       |
| **PC54**    | F0             | 192.168.150.47  | 255.255.255.0 | 192.168.150.1   | 30   | CIT (Fa0/16)       |
| **PC55**    | F0             | 192.168.150.48  | 255.255.255.0 | 192.168.150.1   | 30   | CIT (Fa0/17)       |

### Notes on IP Addressing
- **Subnet**: All devices are in the 192.168.150.0/24 subnet.
- **VLANs**:
  - VLAN 1: Management (minimal use, 2 PCs).
  - VLAN 10: ACTG (Accounting, 15 PCs).
  - VLAN 20: HR (Human Resources, 15 PCs).
  - VLAN 30: CPET (Computer Engineering Technology, 15 PCs).
- **Default Gateway**: Set to 192.168.150.1 (assumed, though no router is present in the topology).
- **IP Range**: Usable IPs are 192.168.150.2 to 192.168.150.254 (192.168.150.1 reserved, 192.168.150.255 is the broadcast address).

---

## Network Topology Overview
The network is a hierarchical structure with UITC Main as the core switch, connected to distribution switches (DS1, DS2) and access switches (IRTC, COS, CIT). PCs are connected to the access switches and grouped into VLANs.

- **UITC Main**:
  - Connects to DS1 (G0/1), DS2 (G0/2), COS (Fa0/1), IRTC (Fa0/2), and CIT (Fa0/3).
- **DS1**:
  - Connects to UITC Main (G0/1) and IRTC (Fa0/1).
- **DS2**:
  - Connects to UITC Main (G0/1) and CIT (Fa0/1).
- **IRTC**:
  - Connects to DS1 (Fa0/1).
  - Connects to PCs: PC0 (VLAN 1), PC1–PC5 (VLAN 10), PC6/PC24–PC27 (VLAN 20), PC7/PC28–PC31 (VLAN 30).
- **COS**:
  - Connects to UITC Main (Fa0/1).
  - Connects to PCs: PC14 (VLAN 1), PC8/PC32–PC35 (VLAN 10), PC9/PC36–PC39 (VLAN 20), PC10/PC40–PC43 (VLAN 30).
- **CIT**:
  - Connects to DS2 (Fa0/1).
  - Connects to PCs: PC11/PC44–PC47 (VLAN 10), PC12/PC48–PC51 (VLAN 20), PC13/PC52–PC55 (VLAN 30).

---

## CLI Configurations

### 1. Switch Configurations

#### UITC Main
UITC Main is the core switch, connecting to distribution and access switches via trunk links.
```bash
enable
configure terminal
hostname UITCMAIN

! Create VLANs
vlan 10
 name ACTG
 exit
vlan 20
 name HR
 exit
vlan 30
 name CPET
 exit

! Configure trunk ports
interface range GigabitEthernet0/1 - 2
 switchport mode trunk
 switchport trunk allowed vlan all
 no shutdown
 exit
interface range FastEthernet0/1 - 3
 switchport mode trunk
 switchport trunk allowed vlan all
 no shutdown
 exit

write memory
```

#### DS1
DS1 is a distribution switch connecting UITC Main to IRTC.
```bash
enable
configure terminal
hostname DS1

! Create VLANs
vlan 10
 name ACTG
 exit
vlan 20
 name HR
 exit
vlan 30
 name CPET
 exit

! Configure trunk ports
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan all
 no shutdown
 exit
interface FastEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan all
 no shutdown
 exit

write memory
```

#### DS2
DS2 is a distribution switch connecting UITC Main to CIT.
```bash
enable
configure terminal
hostname DS2

! Create VLANs
vlan 10
 name ACTG
 exit
vlan 20
 name HR
 exit
vlan 30
 name CPET
 exit

! Configure trunk ports
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan all
 no shutdown
 exit
interface FastEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan all
 no shutdown
 exit

write memory
```

#### IRTC
IRTC is an access switch connecting PCs to the network, with ports assigned to specific VLANs.
```bash
enable
configure terminal
hostname IRTC

! Create VLANs
vlan 10
 name ACTG
 exit
vlan 20
 name HR
 exit
vlan 30
 name CPET
 exit

! Configure trunk port
interface FastEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan all
 no shutdown
 exit

! Configure access ports for VLAN 1
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit

! Configure access ports for VLAN 10
interface range FastEthernet0/3 - 7
 switchport mode access
 switchport access vlan 10
 no shutdown
 exit

! Configure access ports for VLAN 20
interface range FastEthernet0/8 - 12
 switchport mode access
 switchport access vlan 20
 no shutdown
 exit

! Configure access ports for VLAN 30
interface range FastEthernet0/13 - 17
 switchport mode access
 switchport access vlan 30
 no shutdown
 exit

write memory
```

#### COS
COS is an access switch for the College of Science, connecting PCs to the network.
```bash
enable
configure terminal
hostname COS

! Create VLANs
vlan 10
 name ACTG
 exit
vlan 20
 name HR
 exit
vlan 30
 name CPET
 exit

! Configure trunk port
interface FastEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan all
 no shutdown
 exit

! Configure access ports for VLAN 1
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit

! Configure access ports for VLAN 10
interface range FastEthernet0/3 - 7
 switchport mode access
 switchport access vlan 10
 no shutdown
 exit

! Configure access ports for VLAN 20
interface range FastEthernet0/8 - 12
 switchport mode access
 switchport access vlan 20
 no shutdown
 exit

! Configure access ports for VLAN 30
interface range FastEthernet0/13 - 17
 switchport mode access
 switchport access vlan 30
 no shutdown
 exit

write memory
```

#### CIT
CIT is an access switch for the College of Industrial Technology, connecting PCs to the network.
```bash
enable
configure terminal
hostname CIT

! Create VLANs
vlan 10
 name ACTG
 exit
vlan 20
 name HR
 exit
vlan 30
 name CPET
 exit

! Configure trunk port
interface FastEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan all
 no shutdown
 exit

! Configure access ports for VLAN 10
interface range FastEthernet0/3 - 7
 switchport mode access
 switchport access vlan 10
 no shutdown
 exit

! Configure access ports for VLAN 20
interface range FastEthernet0/8 - 12
 switchport mode access
 switchport access vlan 20
 no shutdown
 exit

! Configure access ports for VLAN 30
interface range FastEthernet0/13 - 17
 switchport mode access
 switchport access vlan 30
 no shutdown
 exit

write memory
```

---

### 2. End Device Configurations (PCs)
PCs are configured with static IP addresses via the Cisco Packet Tracer GUI.

#### Steps to Configure IP Addresses
1. **Click on the PC** (e.g., PC0).
2. Go to the **Desktop** tab.
3. Select **IP Configuration**.
4. Set the **IPv4 Address**, **Subnet Mask**, and **Default Gateway** as per the IP Addressing Table.
5. Close the window to apply the settings.

#### Example Configurations
- **PC0**:
  - IPv4 Address: 192.168.150.2
  - Subnet Mask: 255.255.255.0
  - Default Gateway: 192.168.150.1
- **PC1**:
  - IPv4 Address: 192.168.150.4
  - Subnet Mask: 255.255.255.0
  - Default Gateway: 192.168.150.1
- **PC55**:
  - IPv4 Address: 192.168.150.48
  - Subnet Mask: 255.255.255.0
  - Default Gateway: 192.168.150.1

Repeat this process for all PCs (PC0–PC55) using the IP Addressing Table.

---

## Verification and Testing

### 1. Verify Configurations
Use the following commands to verify the configurations on each switch.

#### Switches (UITC Main, DS1, DS2, IRTC, COS, CIT)
```bash
show running-config
show vlan brief
show interfaces trunk
show interfaces status
```

#### PCs
- Open the **Command Prompt** in the Desktop tab:
  ```cmd
  ipconfig
  ping <another-device-ip>
  ```
  Example for PC1:
  ```cmd
  ipconfig
  ping 192.168.150.9  # Ping PC8 (same VLAN 10)
  ping 192.168.150.19 # Ping PC6 (different VLAN 20)
  ```

### 2. Test Connectivity
#### Within VLAN Connectivity
- **Test**: Ping between PCs in the same VLAN.
- **Examples**:
  - From PC1 (192.168.150.4) to PC8 (192.168.150.9) in VLAN 10: Should succeed.
  - From PC6 (192.168.150.19) to PC9 (192.168.150.24) in VLAN 20: Should succeed.
  - From PC7 (192.168.150.34) to PC10 (192.168.150.39) in VLAN 30: Should succeed.
- **Expected Result**: Successful ping, as PCs are in the same broadcast domain.

#### Between VLAN Connectivity
- **Test**: Ping between PCs in different VLANs.
- **Examples**:
  - From PC1 (192.168.150.4, VLAN 10) to PC6 (192.168.150.19, VLAN 20): Should fail.
  - From PC7 (192.168.150.34, VLAN 30) to PC0 (192.168.150.2, VLAN 1): Should fail.
- **Expected Result**: Failed ping, as VLANs isolate broadcast domains, and no inter-VLAN routing is configured (no router or Layer 3 switch present).

---

## Troubleshooting Tips
- **VLAN Mismatch**: Ensure PCs are assigned to the correct VLAN (`show vlan brief` on switches).
- **Trunk Issues**: Verify trunk links allow all VLANs (`show interfaces trunk`).
- **Connectivity Issues**:
  - Confirm IP addresses and subnet masks are correct (`ipconfig` on PCs).
  - Check interface status (`show interfaces status` on switches).
  - Ensure access ports are assigned to the correct VLAN.
- **Ping Failures**:
  - Within VLAN: Check for physical connectivity or VLAN misconfiguration.
  - Between VLANs: Expected to fail without inter-VLAN routing (normal behavior in this topology).