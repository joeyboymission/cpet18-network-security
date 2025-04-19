# Prelim Exam

## Given

A network administrator is assigned to design a network addressing for the TUP-Manila campus network connection.

**IP Address**: 172.30.240.0 /22  
**Mask**: 255.255.252.0

### Instructions

1. Design an addressing scheme for the following network requirements.  
    **TUP-Manila Number of host addresses with internet access computers**
    - CAFA = 24
    - CIE = 25
    - CIT = 63
    - COS = 30
    - COE = 30
    - CLA = 20
    - ADMIN = 300
2. Design the network topology and map the addresses to connect the network devices using only the first and last usable addresses of a subnet.
3. Calculate IP addressing scheme. Show your solution on a sheet of paper to support your answer.
4. Secure the devices physical and network access.
5. Configure the network for internet connectivity.

### Devices

- 1x Router: Router-PT-Empty (TUP-MANILA)
- 1x Cloud: Cloud-PT (INTERNET)
- 7 Switches: 2960-24TT (CAFA, CIE, CIT, COS, COE, CLA, ADMIN)
- 1 Multi Switch: 356024PS (MAIN-SWITCH)

### VLAN Assignments

- **CAFA**: VLAN 50 (VLAN50)
- **CIE**: VLAN 60 (VLAN60)
- **CIT**: VLAN 20 (VLAN20)
- **COS**: VLAN 30 (VLAN30)
- **COE**: VLAN 40 (VLAN40)
- **CLA**: VLAN 70 (VLAN70)
- **ADMIN**: VLAN 10 (VLAN10)

---

## Solution

#### Step 1: Design an Addressing Scheme

We start with the given IP address 172.30.240.0 and subnet mask 255.255.252.0. First, we determine the number of bits available for subnetting and the number of hosts the current network can support.

**Subnet Mask Analysis**:  
The subnet mask 255.255.252.0 is /22 in CIDR notation (since 255 = 8 bits, 255 = 8 bits, 252 = 6 bits; total = 22 bits).  
This leaves 32 - 22 = 10 bits for hosts.  
Number of hosts per subnet = 2^10 - 2 = 1024 - 2 = **1022 hosts**.

1022 hosts is too many to allocate for each required allocation, so we need to subnet further.

**Department Host Requirements (Usable Hosts)**:

- CAFA: 24 hosts
- CIE: 25 hosts
- CIT: 63 hosts
- COS: 30 hosts
- COE: 30 hosts
- CLA: 20 hosts
- ADMIN: 300 hosts

**Department Host Requirements (Total, Including Network and Broadcast)**:  
We add 2 addresses (network and broadcast) to the usable host requirement:

- CAFA: 24 + 2 = 26 total addresses
- CIE: 25 + 2 = 27 total addresses
- CIT: 63 + 2 = 65 total addresses
- COS: 30 + 2 = 32 total addresses
- COE: 30 + 2 = 32 total addresses
- CLA: 20 + 2 = 22 total addresses
- ADMIN: 300 + 2 = 302 total addresses

To accommodate these departments, we subnet the 172.30.240.0/22 network into smaller subnets that can support the total number of addresses for each department.

**Calculate Subnet Sizes**:  
For each department, we allocate a subnet that can support the total number of addresses. The formula for the total number of addresses per subnet is 2^n, where n is the number of host bits. We need to find the smallest n that satisfies each department's total address requirement.

- CAFA: 26 total addresses → 2^5 = 32 addresses (5 bits, /27)
    - Usable: 32 - 2 = 30
- CIE: 27 total addresses → 2^5 = 32 addresses (5 bits, /27)
    - Usable: 32 - 2 = 30
- CIT: 65 total addresses → 2^7 = 128 addresses (7 bits, /25)
    - Usable: 128 - 2 = 126
- COS: 32 total addresses → 2^6 = 64 addresses (6 bits, /26)
    - Usable: 64 - 2 = 62
- COE: 32 total addresses → 2^6 = 64 addresses (6 bits, /26)
    - Usable: 64 - 2 = 62
- CLA: 22 total addresses → 2^5 = 32 addresses (5 bits, /27)
    - Usable: 32 - 2 = 30
- ADMIN: 302 total addresses → 2^9 = 512 addresses (9 bits, /23)
    - Usable: 512 - 2 = 510

**Sort Departments by Size (Largest to Smallest)**:  
This ensures efficient allocation of address space:

- ADMIN: 302 total addresses (/23)
- CIT: 65 total addresses (/25)
- COS: 32 total addresses (/26)
- COE: 32 total addresses (/26)
- CAFA: 26 total addresses (/27)
- CIE: 27 total addresses (/27)
- CLA: 22 total addresses (/27)

**Subnet the Network**:  
The base network is 172.30.240.0/22, which spans from 172.30.240.0 to 172.30.243.255 (1024 addresses). We allocate subnets starting from the base address.

- **ADMIN (/23)**: Needs 512 addresses.
    
    - Subnet: 172.30.240.0/23
    - Network Address: 172.30.240.0
    - Broadcast Address: 172.30.241.255
    - Range: 172.30.240.0 to 172.30.241.255
    - Usable: 172.30.240.1 to 172.30.241.254 (510 usable)
    - Next available: 172.30.242.0
- **CIT (/25)**: Needs 128 addresses.
    
    - Subnet: 172.30.242.0/25
    - Network Address: 172.30.242.0
    - Broadcast Address: 172.30.242.127
    - Range: 172.30.242.0 to 172.30.242.127
    - Usable: 172.30.242.1 to 172.30.242.126 (126 usable)
    - Next available: 172.30.242.128
- **COS (/26)**: Needs 64 addresses.
    
    - Subnet: 172.30.242.128/26
    - Network Address: 172.30.242.128
    - Broadcast Address: 172.30.242.191
    - Range: 172.30.242.128 to 172.30.242.191
    - Usable: 172.30.242.129 to 172.30.242.190 (62 usable)
    - Next available: 172.30.242.192
- **COE (/26)**: Needs 64 addresses.
    
    - Subnet: 172.30.242.192/26
    - Network Address: 172.30.242.192
    - Broadcast Address: 172.30.242.255
    - Range: 172.30.242.192 to 172.30.242.255
    - Usable: 172.30.242.193 to 172.30.242.254 (62 usable)
    - Next available: 172.30.243.0
- **CAFA (/27)**: Needs 32 addresses.
    
    - Subnet: 172.30.243.0/27
    - Network Address: 172.30.243.0
    - Broadcast Address: 172.30.243.31
    - Range: 172.30.243.0 to 172.30.243.31
    - Usable: 172.30.243.1 to 172.30.243.30 (30 usable)
    - Next available: 172.30.243.32
- **CIE (/27)**: Needs 32 addresses.
    
    - Subnet: 172.30.243.32/27
    - Network Address: 172.30.243.32
    - Broadcast Address: 172.30.243.63
    - Range: 172.30.243.32 to 172.30.243.63
    - Usable: 172.30.243.33 to 172.30.243.62 (30 usable)
    - Next available: 172.30.243.64
- **CLA (/27)**: Needs 32 addresses.
    
    - Subnet: 172.30.243.64/27
    - Network Address: 172.30.243.64
    - Broadcast Address: 172.30.243.95
    - Range: 172.30.243.64 to 172.30.243.95
    - Usable: 172.30.243.65 to 172.30.243.94 (30 usable)
    - Next available: 172.30.243.96

This addressing scheme fits within the 172.30.240.0/22 range (1024 addresses) and meets all department requirements.

#### Step 2: Design the Network Topology and Map Addresses

We use a **Router-on-a-Stick** configuration, where the router (TUP-MANILA) handles inter-VLAN routing via a single trunk link to the MAIN-SWITCH. The MAIN-SWITCH connects to departmental switches, and each departmental switch connects PCs to their respective VLANs.

**Note**: The topology lists 2 PCs per department, which doesn’t match the host requirements (e.g., ADMIN needs 300 hosts). For this exam, we assume the 2 PCs represent the first and last usable addresses of each subnet as a simplified demonstration.

**Topology Overview**:

**INTERNET (Cloud-PT)**

- From Eth6 to TUP-MANILA Gig 1/0

**TUP-MANILA (Router-PT-Empty)**

- From Gig 0/0 to MAIN-SWITCH Gig 0/1 (Trunk, VLANs 10,20,30,40,50,60,70)
- From Gig 1/0 to INTERNET Eth6

**MAIN-SWITCH (356024PS)**

- From Gig 0/1 to TUP-MANILA Gig 0/0 (Trunk, VLANs 10,20,30,40,50,60,70)
- From Fa 0/1 to CAFA Gig 0/1 (Trunk, VLAN 50)
- From Fa 0/2 to CIE Gig 0/1 (Trunk, VLAN 60)
- From Fa 0/3 to ADMIN Gig 0/1 (Trunk, VLAN 10)
- From Fa 0/4 to CIT Gig 0/1 (Trunk, VLAN 20)
- From Fa 0/5 to COS Gig 0/1 (Trunk, VLAN 30)
- From Fa 0/6 to COE Gig 0/1 (Trunk, VLAN 40)
- From Fa 0/7 to CLA Gig 0/1 (Trunk, VLAN 70)

**CAFA (2960-24TT)**

- From Gig 0/1 to MAIN-SWITCH Fa 0/1 (Trunk, VLAN 50)
- From Fa 0/1 to PC0 Fa0 (Access, VLAN 50)
- From Fa 0/2 to PC1 Fa0 (Access, VLAN 50)

**CIE (2960-24TT)**

- From Gig 0/1 to MAIN-SWITCH Fa 0/2 (Trunk, VLAN 60)
- From Fa 0/1 to PC2 Fa0 (Access, VLAN 60)
- From Fa 0/2 to PC3 Fa0 (Access, VLAN 60)

**ADMIN (2960-24TT)**

- From Gig 0/1 to MAIN-SWITCH Fa 0/3 (Trunk, VLAN 10)
- From Fa 0/1 to PC4 Fa0 (Access, VLAN 10)
- From Fa 0/2 to PC5 Fa0 (Access, VLAN 10)

**CIT (2960-24TT)**

- From Gig 0/1 to MAIN-SWITCH Fa 0/4 (Trunk, VLAN 20)
- From Fa 0/1 to PC6 Fa0 (Access, VLAN 20)
- From Fa 0/2 to PC7 Fa0 (Access, VLAN 20)

**COS (2960-24TT)**

- From Gig 0/1 to MAIN-SWITCH Fa 0/5 (Trunk, VLAN 30)
- From Fa 0/1 to PC8 Fa0 (Access, VLAN 30)
- From Fa 0/2 to PC9 Fa0 (Access, VLAN 30)

**COE (2960-24TT)**

- From Gig 0/1 to MAIN-SWITCH Fa 0/6 (Trunk, VLAN 40)
- From Fa 0/1 to PC10 Fa0 (Access, VLAN 40)
- From Fa 0/2 to PC11 Fa0 (Access, VLAN 40)

**CLA (2960-24TT)**

- From Gig 0/1 to MAIN-SWITCH Fa 0/7 (Trunk, VLAN 70)
- From Fa 0/1 to PC12 Fa0 (Access, VLAN 70)
- From Fa 0/2 to PC13 Fa0 (Access, VLAN 70)

**Address Mapping (First and Last Usable Addresses)**:  
The first usable address in each subnet is assigned to the router sub-interface (default gateway), the second usable address is assigned to the switch for management, and the last usable address is assigned to the second PC in each department. The first PC uses the third usable address to demonstrate the range.

- **ADMIN (172.30.240.0/23, VLAN 10)**:
    
    - Possible Addresses: 2^9 = 512
    - Usable Addresses: 512 - 2 = 510
    - Network Address: 172.30.240.0
    - Broadcast Address: 172.30.241.255
    - Range of Usable Addresses: 172.30.240.1 to 172.30.241.254
    - Router Sub-Interface (TUP-MANILA Gig0/0.10): 172.30.240.1
    - Switch (ADMIN): 172.30.240.2
    - MAIN-SWITCH: 172.30.240.3
    - PC4: 172.30.240.4
    - PC5: 172.30.241.254
- **CIT (172.30.242.0/25, VLAN 20)**:
    
    - Possible Addresses: 2^7 = 128
    - Usable Addresses: 128 - 2 = 126
    - Network Address: 172.30.242.0
    - Broadcast Address: 172.30.242.127
    - Range of Usable Addresses: 172.30.242.1 to 172.30.242.126
    - Router Sub-Interface (TUP-MANILA Gig0/0.20): 172.30.242.1
    - Switch (CIT): 172.30.242.2
    - PC6: 172.30.242.3
    - PC7: 172.30.242.126
- **COS (172.30.242.128/26, VLAN 30)**:
    
    - Possible Addresses: 2^6 = 64
    - Usable Addresses: 64 - 2 = 62
    - Network Address: 172.30.242.128
    - Broadcast Address: 172.30.242.191
    - Range of Usable Addresses: 172.30.242.129 to 172.30.242.190
    - Router Sub-Interface (TUP-MANILA Gig0/0.30): 172.30.242.129
    - Switch (COS): 172.30.242.130
    - PC8: 172.30.242.131
    - PC9: 172.30.242.190
- **COE (172.30.242.192/26, VLAN 40)**:
    
    - Possible Addresses: 2^6 = 64
    - Usable Addresses: 64 - 2 = 62
    - Network Address: 172.30.242.192
    - Broadcast Address: 172.30.242.255
    - Range of Usable Addresses: 172.30.242.193 to 172.30.242.254
    - Router Sub-Interface (TUP-MANILA Gig0/0.40): 172.30.242.193
    - Switch (COE): 172.30.242.194
    - PC10: 172.30.242.195
    - PC11: 172.30.242.254
- **CAFA (172.30.243.0/27, VLAN 50)**:
    
    - Possible Addresses: 2^5 = 32
    - Usable Addresses: 32 - 2 = 30
    - Network Address: 172.30.243.0
    - Broadcast Address: 172.30.243.31
    - Range of Usable Addresses: 172.30.243.1 to 172.30.243.30
    - Router Sub-Interface (TUP-MANILA Gig0/0.50): 172.30.243.1
    - Switch (CAFA): 172.30.243.2
    - PC0: 172.30.243.3
    - PC1: 172.30.243.30
- **CIE (172.30.243.32/27, VLAN 60)**:
    
    - Possible Addresses: 2^5 = 32
    - Usable Addresses: 32 - 2 = 30
    - Network Address: 172.30.243.32
    - Broadcast Address: 172.30.243.63
    - Range of Usable Addresses: 172.30.243.33 to 172.30.243.62
    - Router Sub-Interface (TUP-MANILA Gig0/0.60): 172.30.243.33
    - Switch (CIE): 172.30.243.34
    - PC2: 172.30.243.35
    - PC3: 172.30.243.62
- **CLA (172.30.243.64/27, VLAN 70)**:
    
    - Possible Addresses: 2^5 = 32
    - Usable Addresses: 32 - 2 = 30
    - Network Address: 172.30.243.64
    - Broadcast Address: 172.30.243.95
    - Range of Usable Addresses: 172.30.243.65 to 172.30.243.94
    - Router Sub-Interface (TUP-MANILA Gig0/0.70): 172.30.243.65
    - Switch (CLA): 172.30.243.66
    - PC12: 172.30.243.67
    - PC13: 172.30.243.94

The router sub-interfaces act as the default gateway for each VLAN.

#### Step 3: Calculate IP Addressing Scheme

The addressing scheme has been calculated above, with detailed computations for each subnet (possible addresses, usable addresses, network address, broadcast address, and usable range). As requested, this should be transcribed on a sheet of paper using the details provided in Step 1.

#### Step 4: Secure the Devices (Physical and Network Access)

- **Physical Security**:
    
    - Place routers, switches, and servers in locked rooms with restricted access.
    - Use security cameras and access logs to monitor entry.
    - Label cables and devices to prevent tampering.
- **Network Security**:
    
    - VLANs are implemented for each department (VLAN 10 for ADMIN, VLAN 20 for CIT, etc.) to segregate traffic.
    - Configure Access Control Lists (ACLs) on the router to restrict inter-VLAN access unless necessary.
    - Use strong passwords for all devices and enable SSH for remote access (disable Telnet).
    - Disable unused ports on switches to prevent unauthorized access.
    - Enable MAC address filtering to limit device connections.

#### Step 5: Configure the Network for Internet Connectivity

- **Router Configuration**:
    
    - Assign the router’s external interface (Gig 1/0) a public IP (not provided, so assume it’s given by the ISP).
    - Configure NAT (Network Address Translation) to map internal private IPs (172.30.240.0/22) to the public IP for internet access.
    - Set up a default route on the router pointing to the ISP gateway (INTERNET).
    - Ensure DHCP is enabled on the router to dynamically assign IPs to hosts in each subnet (though for this exam, we use static IPs for PCs).
- **DNS Configuration**:
    
    - Configure the router to use a public DNS server (e.g., Google DNS: 8.8.8.8) for name resolution.
- **Firewall Rules**:
    
    - Allow outbound traffic for internet access.
    - Block unsolicited inbound traffic to protect internal devices.

#### Step 6: CLI Commands for Setup

Below are the updated CLI commands to set up the network in Cisco Packet Tracer using Router-on-a-Stick, reflecting the new IP ranges.

**TUP-MANILA (Router-PT-Empty)**

```plaintext
enable
configure terminal
hostname TUP-MANILA

! Configure sub-interfaces for each VLAN
interface GigabitEthernet0/0
 no ip address
 no shutdown
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 172.30.240.1 255.255.254.0
 no shutdown
interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 172.30.242.1 255.255.255.128
 no shutdown
interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 172.30.242.129 255.255.255.192
 no shutdown
interface GigabitEthernet0/0.40
 encapsulation dot1Q 40
 ip address 172.30.242.193 255.255.255.192
 no shutdown
interface GigabitEthernet0/0.50
 encapsulation dot1Q 50
 ip address 172.30.243.1 255.255.255.224
 no shutdown
interface GigabitEthernet0/0.60
 encapsulation dot1Q 60
 ip address 172.30.243.33 255.255.255.224
 no shutdown
interface GigabitEthernet0/0.70
 encapsulation dot1Q 70
 ip address 172.30.243.65 255.255.255.224
 no shutdown

! Configure external interface (assume ISP provides IP)
interface GigabitEthernet1/0
 ip address dhcp
 no shutdown

! Configure NAT
ip nat inside source list 1 interface GigabitEthernet1/0 overload
access-list 1 permit 172.30.240.0 0.0.3.255
interface GigabitEthernet0/0.10
 ip nat inside
interface GigabitEthernet0/0.20
 ip nat inside
interface GigabitEthernet0/0.30
 ip nat inside
interface GigabitEthernet0/0.40
 ip nat inside
interface GigabitEthernet0/0.50
 ip nat inside
interface GigabitEthernet0/0.60
 ip nat inside
interface GigabitEthernet0/0.70
 ip nat inside
interface GigabitEthernet1/0
 ip nat outside

! Configure default route (assume ISP gateway is 203.0.113.1)
ip route 0.0.0.0 0.0.0.0 203.0.113.1

! Configure DNS
ip domain-lookup
ip name-server 8.8.8.8

! Enable SSH
ip domain-name tupmanila.local
crypto key generate rsa
 1024
username cisco password cisco
line vty 0 4
 login local
 transport input ssh
exit
```

**MAIN-SWITCH (356024PS)**

```plaintext
enable
configure terminal
hostname MAIN-SWITCH

! Create VLANs
vlan 10
 name VLAN10
vlan 20
 name VLAN20
vlan 30
 name VLAN30
vlan 40
 name VLAN40
vlan 50
 name VLAN50
vlan 60
 name VLAN60
vlan 70
 name VLAN70

! Configure management IP in VLAN 1 (using ADMIN subnet)
interface vlan 1
 ip address 172.30.240.3 255.255.254.0
 no shutdown

! Configure default gateway (point to router's ADMIN sub-interface)
ip default-gateway 172.30.240.1

! Configure trunk port to TUP-MANILA
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,40,50,60,70

! Configure trunk ports to departmental switches
interface FastEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 50
interface FastEthernet0/2
 switchport mode trunk
 switchport trunk allowed vlan 60
interface FastEthernet0/3
 switchport mode trunk
 switchport trunk allowed vlan 10
interface FastEthernet0/4
 switchport mode trunk
 switchport trunk allowed vlan 20
interface FastEthernet0/5
 switchport mode trunk
 switchport trunk allowed vlan 30
interface FastEthernet0/6
 switchport mode trunk
 switchport trunk allowed vlan 40
interface FastEthernet0/7
 switchport mode trunk
 switchport trunk allowed vlan 70

! Enable SSH
ip domain-name tupmanila.local
crypto key generate rsa
 1024
username cisco password cisco
line vty 0 4
 login local
 transport input ssh
exit
```

**CAFA Switch (2960-24TT)**

```plaintext
enable
configure terminal
hostname CAFA-SWITCH

! Create VLAN
vlan 50
 name VLAN50

! Configure management IP in VLAN 50
interface vlan 50
 ip address 172.30.243.2 255.255.255.224
 ip default-gateway 172.30.243.1
 no shutdown

! Configure trunk port to MAIN-SWITCH
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 50

! Configure access ports for PCs
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 50
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 50

! Enable SSH
ip domain-name tupmanila.local
crypto key generate rsa
 1024
username cisco password cisco
line vty 0 4
 login local
 transport input ssh
exit
```

**CIE Switch (2960-24TT)**

```plaintext
enable
configure terminal
hostname CIE-SWITCH

! Create VLAN
vlan 60
 name VLAN60

! Configure management IP in VLAN 60
interface vlan 60
 ip address 172.30.243.34 255.255.255.224
 ip default-gateway 172.30.243.33
 no shutdown

! Configure trunk port to MAIN-SWITCH
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 60

! Configure access ports for PCs
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 60
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 60

! Enable SSH
ip domain-name tupmanila.local
crypto key generate rsa
 1024
username cisco password cisco
line vty 0 4
 login local
 transport input ssh
exit
```

**ADMIN Switch (2960-24TT)**

```plaintext
enable
configure terminal
hostname ADMIN-SWITCH

! Create VLAN
vlan 10
 name VLAN10

! Configure management IP in VLAN 10
interface vlan 10
 ip address 172.30.240.2 255.255.254.0
 ip default-gateway 172.30.240.1
 no shutdown

! Configure trunk port to MAIN-SWITCH
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10

! Configure access ports for PCs
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 10

! Enable SSH
ip domain-name tupmanila.local
crypto key generate rsa
 1024
username cisco password cisco
line vty 0 4
 login local
 transport input ssh
exit
```

**CIT Switch (2960-24TT)**

```plaintext
enable
configure terminal
hostname CIT-SWITCH

! Create VLAN
vlan 20
 name VLAN20

! Configure management IP in VLAN 20
interface vlan 20
 ip address 172.30.242.2 255.255.255.128
 ip default-gateway 172.30.242.1
 no shutdown

! Configure trunk port to MAIN-SWITCH
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 20

! Configure access ports for PCs
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 20
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 20

! Enable SSH
ip domain-name tupmanila.local
crypto key generate rsa
 1024
username cisco password cisco
line vty 0 4
 login local
 transport input ssh
exit
```

**COS Switch (2960-24TT)**

```plaintext
enable
configure terminal
hostname COS-SWITCH

! Create VLAN
vlan 30
 name VLAN30

! Configure management IP in VLAN 30
interface vlan 30
 ip address 172.30.242.130 255.255.255.192
 ip default-gateway 172.30.242.129
 no shutdown

! Configure trunk port to MAIN-SWITCH
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 30

! Configure access ports for PCs
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 30
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 30

! Enable SSH
ip domain-name tupmanila.local
crypto key generate rsa
 1024
username cisco password cisco
line vty 0 4
 login local
 transport input ssh
exit
```

**COE Switch (2960-24TT)**

```plaintext
enable
configure terminal
hostname COE-SWITCH

! Create VLAN
vlan 40
 name VLAN40

! Configure management IP in VLAN 40
interface vlan 40
 ip address 172.30.242.194 255.255.255.192
 ip default-gateway 172.30.242.193
 no shutdown

! Configure trunk port to MAIN-SWITCH
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 40

! Configure access ports for PCs
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 40
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 40

! Enable SSH
ip domain-name tupmanila.local
crypto key generate rsa
 1024
username cisco password cisco
line vty 0 4
 login local
 transport input ssh
exit
```

**CLA Switch (2960-24TT)**

```plaintext
enable
configure terminal
hostname CLA-SWITCH

! Create VLAN
vlan 70
 name VLAN70

! Configure management IP in VLAN 70
interface vlan 70
 ip address 172.30.243.66 255.255.255.224
 ip default-gateway 172.30.243.65
 no shutdown

! Configure trunk port to MAIN-SWITCH
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 70

! Configure access ports for PCs
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 70
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 70

! Enable SSH
ip domain-name tupmanila.local
crypto key generate rsa
 1024
username cisco password cisco
line vty 0 4
 login local
 transport input ssh
exit
```

**PC Configurations (Static IPs)**

- **PC0 (CAFA, VLAN 50)**: IP 172.30.243.3, Subnet Mask 255.255.255.224, Gateway 172.30.243.1
- **PC1 (CAFA, VLAN 50)**: IP 172.30.243.30, Subnet Mask 255.255.255.224, Gateway 172.30.243.1
- **PC2 (CIE, VLAN 60)**: IP 172.30.243.35, Subnet Mask 255.255.255.224, Gateway 172.30.243.33
- **PC3 (CIE, VLAN 60)**: IP 172.30.243.62, Subnet Mask 255.255.255.224, Gateway 172.30.243.33
- **PC4 (ADMIN, VLAN 10)**: IP 172.30.240.4, Subnet Mask 255.255.254.0, Gateway 172.30.240.1
- **PC5 (ADMIN, VLAN 10)**: IP 172.30.241.254, Subnet Mask 255.255.254.0, Gateway 172.30.240.1
- **PC6 (CIT, VLAN 20)**: IP 172.30.242.3, Subnet Mask 255.255.255.128, Gateway 172.30.242.1
- **PC7 (CIT, VLAN 20)**: IP 172.30.242.126, Subnet Mask 255.255.255.128, Gateway 172.30.242.1
- **PC8 (COS, VLAN 30)**: IP 172.30.242.131, Subnet Mask 255.255.255.192, Gateway 172.30.242.129
- **PC9 (COS, VLAN 30)**: IP 172.30.242.190, Subnet Mask 255.255.255.192, Gateway 172.30.242.129
- **PC10 (COE, VLAN 40)**: IP 172.30.242.195, Subnet Mask 255.255.255.192, Gateway 172.30.242.193
- **PC11 (COE, VLAN 40)**: IP 172.30.242.254, Subnet Mask 255.255.255.192, Gateway 172.30.242.193
- **PC12 (CLA, VLAN 70)**: IP 172.30.243.67, Subnet Mask 255.255.255.224, Gateway 172.30.243.65
- **PC13 (CLA, VLAN 70)**: IP 172.30.243.94, Subnet Mask 255.255.255.224, Gateway 172.30.243.65

In Cisco Packet Tracer, set these IPs via the PC’s Desktop > IP Configuration tab.