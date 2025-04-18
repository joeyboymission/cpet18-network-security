# Inter-VLAN Routing

## What is a VLAN?

A Virtual Local Area Network (VLAN) is a logical subdivision of a physical network, grouping devices into isolated domains regardless of their physical location.

**Benefits**:

- **Reduced Broadcast Traffic**: Limits broadcast messages to specific VLANs, improving network efficiency.
- **Enhanced Security**: Isolates sensitive devices or data, reducing unauthorized access risks.
- **Improved Management**: Groups devices by function (e.g., HR, IT) or location for easier administration.

**Challenges**: VLANs create isolated segments, preventing direct communication between them, which can lead to data silos and operational inefficiencies without Inter-VLAN Routing.

## What is Inter-VLAN Routing?

Inter-VLAN Routing is the process of forwarding network traffic between different VLANs, enabling communication across isolated network segments using a Layer 3 device (router or Layer 3 switch).

**Purpose**:

- Facilitates resource sharing (e.g., servers, printers) across departments while maintaining VLAN segmentation.
- Supports enterprise collaboration by allowing controlled communication without compromising network security or integrity.

**Key Concepts**:

- **Layer 3 Requirement**: Unlike Layer 2 switching within a VLAN, Inter-VLAN Routing involves Layer 3 IP-based routing to connect VLANs.
- **Controlled Connectivity**: Bridges VLAN isolation to enable specific, authorized communication, balancing functionality with security.
- **Enterprise Relevance**: Essential for modern networks where departments, guest networks, or IoT devices require both segregation and collaboration.
- **Mechanisms**: Uses routers or Layer 3 switches with VLAN tagging (802.1Q) and subnetting to route traffic between VLANs.

**Importance**:

- Ensures seamless communication for business functions, such as cross-departmental access to shared applications or VoIP systems.
- Supports advanced use cases like IoT integration, guest VLANs, and hybrid cloud environments by routing traffic securely between segmented networks.

## Types of Inter-VLAN Routing

1. **Traditional/Legacy**
2. **Router-on-a-Stick**
3. **Multilayer Switch**

### Traditional/Legacy

Each VLAN connects to a dedicated physical interface on a router, which routes traffic between VLANs. The switch assigns access ports to specific VLANs, and each router interface acts as the VLAN’s gateway.

**Information**:

- Suitable for small networks with few VLANs (e.g., 2–3 VLANs).
- Each interface requires a unique IP address in the VLAN’s subnet.
- Not scalable due to the limited number of physical router interfaces, making it obsolete for modern networks with many VLANs.

**Common CLI Commands**:

```plaintext
! Switch Configuration
configure terminal
vlan 10
 name VLAN10
vlan 20
 name VLAN20
interface fa0/1
 switchport mode access
 switchport access vlan 10
interface fa0/2
 switchport mode access
 switchport access vlan 20

! Router Configuration
configure terminal
interface fa0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
interface fa0/1
 ip address 192.168.20.1 255.255.255.0
 no shutdown
```

**Notes**: Connect fa0/1 (switch) to fa0/0 (router) for VLAN 10, and fa0/2 to fa0/1 for VLAN 20. This method is rarely used today due to scalability limitations.

**Given**:

- **IP Address Range**:
    - Overall Network: 192.168.100.0/25 (255.255.255.128)
    - VLAN 10 (IT): 192.168.100.0/27 (192.168.100.0–192.168.100.31)
    - VLAN 20 (ADMIN): 192.168.100.32/27 (192.168.100.32–192.168.100.63)
    - VLAN 30 (GUEST): 192.168.100.64/27 (192.168.100.64–192.168.100.95)
    - VLAN 50 (IOT): 192.168.100.96/27 (192.168.100.96–192.168.100.127)
- **Subnet Mask**: 255.255.255.224 (/27) for each VLAN
- **Default Gateway (for PCs)**:
    - VLAN 10: 192.168.100.2
    - VLAN 20: 192.168.100.34
    - VLAN 30: 192.168.100.66
    - VLAN 50: 192.168.100.98
- **VLAN Groups**:
    - VLAN 10: PC0, PC1 (COS)
    - VLAN 20: PC2, PC3 (CIT)
    - VLAN 30: PC4, PC5, PC8, PC9 (CAFA, CIE)
    - VLAN 50: PC6, PC7, PC10, PC12 (CAFA, CIE)
- **Devices**:
    - 12 PCs (PC0–PC5, PC6–PC8, PC9, PC10, PC12)
    - 1 Router (UITC)
    - 4 Switches (COS, CIT, CAFA, CIE)
- **Total Devices**: 17 (12 PCs + 1 Router + 4 Switches)

**Topology Description**:

- **UITC (Router)**:
    - Gig0/0: IP 192.168.100.2/27, to COS Fa0/1 (access VLAN 10), speed auto, duplex auto
    - Gig1/0: IP 192.168.100.34/27, to CIT Fa0/1 (access VLAN 20), speed 10 Mbps, duplex auto
    - Gig2/0: IP 192.168.100.66/27, to CAFA Fa0/7 (access VLAN 30), speed auto, duplex auto
    - Gig3/0: IP 192.168.100.98/27, to CAFA Fa0/5 (access VLAN 50), speed 1000 Mbps, duplex auto
- **COS (Switch)**:
    - Fa0/1: To UITC Gig0/0 (access VLAN 10)
    - Fa0/2: To PC0 (access VLAN 10)
    - Fa0/3: To PC1 (access VLAN 10)
    - Management: ip default-gateway 192.168.100.2
- **CIT (Switch)**:
    - Fa0/1: To UITC Gig1/0 (access VLAN 20)
    - Fa0/2: To PC2 (access VLAN 20)
    - Fa0/3: To PC3 (access VLAN 20)
    - Management: ip default-gateway 192.168.100.34
- **CAFA (Switch)**:
    - Fa0/1: To PC4 (access VLAN 30)
    - Fa0/2: To PC5 (access VLAN 30)
    - Fa0/3: To PC12 (access VLAN 50)
    - Fa0/4: To PC6 (access VLAN 50)
    - Fa0/5: To UITC Gig3/0 (access VLAN 50)
    - Fa0/6: To CIE Fa0/6 (access VLAN 30)
    - Fa0/7: To UITC Gig2/0 (access VLAN 30)
    - Fa0/8: Available (access VLAN 30)
    - Management: ip default-gateway 192.168.100.98
- **CIE (Switch)**:
    - Fa0/1: To PC8 (access VLAN 30)
    - Fa0/2: To PC9 (access VLAN 30)
    - Fa0/3: To PC10 (access VLAN 50)
    - Fa0/4: Available (access VLAN 50)
    - Fa0/5: Available (access VLAN 50)
    - Fa0/6: To CAFA Fa0/6 (access VLAN 30)
    - Management: ip default-gateway 192.168.100.98

**Sample PC IP Assignments**:

- PC0 (VLAN 10): 192.168.100.1/27, Gateway 192.168.100.2
- PC1 (VLAN 10): 192.168.100.30/27, Gateway 192.168.100.2
- PC2 (VLAN 20): 192.168.100.33/27, Gateway 192.168.100.34
- PC3 (VLAN 20): 192.168.100.62/27, Gateway 192.168.100.34
- PC4 (VLAN 30): 192.168.100.65/27, Gateway 192.168.100.66
- PC5 (VLAN 30): 192.168.100.94/27, Gateway 192.168.100.66
- PC8 (VLAN 30): 192.168.100.67/27, Gateway 192.168.100.66
- PC9 (VLAN 30): 192.168.100.68/27, Gateway 192.168.100.66
- PC6 (VLAN 50): 192.168.100.97/27, Gateway 192.168.100.98
- PC7 (VLAN 50): 192.168.100.126/27, Gateway 192.168.100.98
- PC10 (VLAN 50): 192.168.100.125/27, Gateway 192.168.100.98
- PC12 (VLAN 50): 192.168.100.99/27, Gateway 192.168.100.98

**CLI Configuration**:

**UITC Router Configuration**:

```
configure terminal
interface GigabitEthernet0/0
 ip address 192.168.100.2 255.255.255.224
 no shutdown
interface GigabitEthernet1/0
 ip address 192.168.100.34 255.255.255.224
 speed 10
 no shutdown
interface GigabitEthernet2/0
 ip address 192.168.100.66 255.255.255.224
 no shutdown
interface GigabitEthernet3/0
 ip address 192.168.100.98 255.255.255.224
 speed 1000
 no shutdown
exit
```

**COS Switch Configuration**:

```
configure terminal
vlan 10
 name IT
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 10
interface FastEthernet0/3
 switchport mode access
 switchport access vlan 10
ip default-gateway 192.168.100.2
exit
```

**CIT Switch Configuration**:

```
configure terminal
vlan 20
 name ADMIN
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 20
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 20
interface FastEthernet0/3
 switchport mode access
 switchport access vlan 20
ip default-gateway 192.168.100.34
exit
```

**CAFA Switch Configuration**:

```
configure terminal
vlan 30
 name GUEST
vlan 50
 name IOT
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 30
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 30
interface FastEthernet0/3
 switchport mode access
 switchport access vlan 50
interface FastEthernet0/4
 switchport mode access
 switchport access vlan 50
interface FastEthernet0/5
 switchport mode access
 switchport access vlan 50
interface FastEthernet0/6
 switchport mode access
 switchport access vlan 30
interface FastEthernet0/7
 switchport mode access
 switchport access vlan 30
interface FastEthernet0/8
 switchport mode access
 switchport access vlan 30
ip default-gateway 192.168.100.98
exit
```

**CIE Switch Configuration**:

```
configure terminal
vlan 30
 name GUEST
vlan 50
 name IOT
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 30
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 30
interface FastEthernet0/3
 switchport mode access
 switchport access vlan 50
interface FastEthernet0/4
 switchport mode access
 switchport access vlan 50
interface FastEthernet0/5
 switchport mode access
 switchport access vlan 50
interface FastEthernet0/6
 switchport mode access
 switchport access vlan 30
ip default-gateway 192.168.100.98
exit
```

**Notes**:

- PCs use the router’s interface IP as their default gateway for Inter-VLAN Routing (e.g., 192.168.100.66 for VLAN 30 PCs).
- Switch management gateways (e.g., CAFA’s 192.168.100.98) are for switch access, not PC routing.
- PC9 was added to CIE Fa0/2 (VLAN 30) to complete the topology, as it was missing from the provided mappings.
- Corrected subnet masks to /27 for all PCs to match router interfaces and VLAN subnets.
- Adjusted PC2 and PC3 gateways to 192.168.100.34, fixing typos in the provided mappings.

**Task**: Configure the topology in Cisco Packet Tracer, apply the above configurations, and test connectivity (e.g., ping PC0 to PC4) to verify Inter-VLAN Routing. Use `show vlan brief`, `show ip route`, and `ping` for troubleshooting.

### Router-on-a-Stick

## Given

- **IP Address Range**:
    - Overall Network: 10.10.10.0/24 (255.255.255.0)
    - VLAN 10: 10.10.10.0/26 (10.10.10.0–10.10.10.63)
    - VLAN 20: 10.10.10.64/26 (10.10.10.64–10.10.10.127)
    - VLAN 30: 10.10.10.192/26 (10.10.10.192–10.10.10.255)
- **Subnet Mask**: 255.255.255.192 (/26) for each VLAN
- **Default Gateway (for PCs)**:
    - VLAN 10: 10.10.10.50
    - VLAN 20: 10.10.10.100
    - VLAN 30: 10.10.10.200
- **VLAN Groups**:
    - VLAN 10: PC13, PC14 (LIBRARY), PC19, PC20 (CANTEEN), PC25, PC26 (REGISTRAR)
    - VLAN 20: PC15, PC16 (LIBRARY), PC21, PC22 (CANTEEN), PC27, PC28 (REGISTRAR)
    - VLAN 30: PC17, PC18 (LIBRARY), PC23, PC24 (CANTEEN), PC29, PC30 (REGISTRAR)
- **Devices**:
    - 18 PCs (PC13–PC18 on LIBRARY, PC19–PC24 on CANTEEN, PC25–PC30 on REGISTRAR)
    - 1 Router (ADMIN)
    - 3 Switches (LIBRARY, CANTEEN, REGISTRAR)
- **Total Devices**: 22 (18 PCs + 1 Router + 3 Switches)

## Topology Description

- **ADMIN (Router)**:
    - Gig0/0: No IP, trunk to CANTEEN Fa0/1, carrying VLANs 10, 20, 30
    - Subinterfaces:
        - Gig0/0.10: IP 10.10.10.50/26, VLAN 10
        - Gig0/0.20: IP 10.10.10.100/26, VLAN 20
        - Gig0/0.30: IP 10.10.10.200/26, VLAN 30
- **CANTEEN (Switch)**:
    - Fa0/1: Trunk to ADMIN Gig0/0, allowed VLANs 10, 20, 30
    - Fa0/2: Trunk to LIBRARY Fa0/1, allowed VLANs 10, 20, 30
    - Fa0/3: Trunk to REGISTRAR Fa0/1, allowed VLANs 10, 20, 30
    - Fa0/4: To PC19 (access VLAN 10)
    - Fa0/5: To PC20 (access VLAN 10)
    - Fa0/6: To PC21 (access VLAN 20)
    - Fa0/7: To PC22 (access VLAN 20)
    - Fa0/8: To PC23 (access VLAN 30)
    - Fa0/9: To PC24 (access VLAN 30)
- **LIBRARY (Switch)**:
    - Fa0/1: Trunk to CANTEEN Fa0/2, allowed VLANs 10, 20, 30
    - Fa0/2: To PC13 (access VLAN 10)
    - Fa0/3: To PC14 (access VLAN 10)
    - Fa0/4: To PC15 (access VLAN 20)
    - Fa0/5: To PC16 (access VLAN 20)
    - Fa0/6: To PC17 (access VLAN 30)
    - Fa0/7: To PC18 (access VLAN 30)
- **REGISTRAR (Switch)**:
    - Fa0/1: Trunk to CANTEEN Fa0/3, allowed VLANs 10, 20, 30
    - Fa0/2: To PC25 (access VLAN 10)
    - Fa0/3: To PC26 (access VLAN 10)
    - Fa0/4: To PC27 (access VLAN 20)
    - Fa0/5: To PC28 (access VLAN 20)
    - Fa0/6: To PC29 (access VLAN 30)
    - Fa0/7: To PC30 (access VLAN 30)

## Sample PC IP Assignments

- PC13 (VLAN 10, LIBRARY): 10.10.10.1/26, Gateway 10.10.10.50
- PC14 (VLAN 10, LIBRARY): 10.10.10.62/26, Gateway 10.10.10.50
- PC19 (VLAN 10, CANTEEN): 10.10.10.2/26, Gateway 10.10.10.50
- PC20 (VLAN 10, CANTEEN): socks 10.10.10.61/26, Gateway 10.10.10.50
- PC25 (VLAN 10, REGISTRAR): 10.10.10.3/26, Gateway 10.10.10.50
- PC26 (VLAN 10, REGISTRAR): 10.10.10.60/26, Gateway 10.10.10.50
- PC15 (VLAN 20, LIBRARY): 10.10.10.65/26, Gateway 10.10.10.100
- PC16 (VLAN 20, LIBRARY): 10.10.10.126/26, Gateway 10.10.10.100
- PC21 (VLAN 20, CANTEEN): 10.10.10.66/26, Gateway 10.10.10.100
- PC22 (VLAN 20, CANTEEN): 10.10.10.125/26, Gateway 10.10.10.100
- PC27 (VLAN 20, REGISTRAR): 10.10.10.67/26, Gateway 10.10.10.100
- PC28 (VLAN 20, REGISTRAR): 10.10.10.124/26, Gateway 10.10.10.100
- PC17 (VLAN 30, LIBRARY): 10.10.10.193/26, Gateway 10.10.10.200
- PC18 (VLAN 30, LIBRARY): 10.10.10.254/26, Gateway 10.10.10.200
- PC23 (VLAN 30, CANTEEN): 10.10.10.194/26, Gateway 10.10.10.200
- PC24 (VLAN 30, CANTEEN): 10.10.10.253/26, Gateway 10.10.10.200
- PC29 (VLAN 30, REGISTRAR): 10.10.10.195/26, Gateway 10.10.10.200
- PC30 (VLAN 30, REGISTRAR): 10.10.10.252/26, Gateway 10.10.10.200

## CLI Configuration

### ADMIN Router Configuration

```
configure terminal
interface GigabitEthernet0/0
 no ip address
 duplex auto
 speed auto
 no shutdown
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 10.10.10.50 255.255.255.192
 no shutdown
interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 10.10.10.100 255.255.255.192
 no shutdown
interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 10.10.10.200 255.255.255.192
 no shutdown
exit
```

### CANTEEN Switch Configuration

```
configure terminal
vlan 10
 name VLAN10
vlan 20
 name VLAN20
vlan 30
 name VLAN30
interface FastEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
interface FastEthernet0/2
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
interface FastEthernet0/3
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
interface FastEthernet0/4
 switchport mode access
 switchport access vlan 10
interface FastEthernet0/5
 switchport mode access
 switchport access vlan 10
interface FastEthernet0/6
 switchport mode access
 switchport access vlan 20
interface FastEthernet0/7
 switchport mode access
 switchport access vlan 20
interface FastEthernet0/8
 switchport mode access
 switchport access vlan 30
interface FastEthernet0/9
 switchport mode access
 switchport access vlan 30
ip default-gateway 10.10.10.100
exit
```

### LIBRARY Switch Configuration

```
configure terminal
vlan 10
 name VLAN10
vlan 20
 name VLAN20
vlan 30
 name VLAN30
interface FastEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 10
interface FastEthernet0/3
 switchport mode access
 switchport access vlan 10
interface FastEthernet0/4
 switchport mode access
 switchport access vlan 20
interface FastEthernet0/5
 switchport mode access
 switchport access vlan 20
interface FastEthernet0/6
 switchport mode access
 switchport access vlan 30
interface FastEthernet0/7
 switchport mode access
 switchport access vlan 30
ip default-gateway 10.10.10.50
exit
```

### REGISTRAR Switch Configuration

```
configure terminal
vlan 10
 name VLAN10
vlan 20
 name VLAN20
vlan 30
 name VLAN30
interface FastEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 10
interface FastEthernet0/3
 switchport mode access
 switchport access vlan 10
interface FastEthernet0/4
 switchport mode access
 switchport access vlan 20
interface FastEthernet0/5
 switchport mode access
 switchport access vlan 20
interface FastEthernet0/6
 switchport mode access
 switchport access vlan 30
interface FastEthernet0/7
 switchport mode access
 switchport access vlan 30
ip default-gateway 10.10.10.200
exit
```

## Notes

- The trunk between ADMIN and CANTEEN carries VLANs 10, 20, and 30, enabling the router to route traffic between these VLANs.
- Trunks between CANTEEN and LIBRARY/REGISTRAR allow VLAN traffic to span multiple switches, ensuring PCs in the same VLAN across different switches can communicate at Layer 2.
- PCs use the router’s subinterface IP as their default gateway for Inter-VLAN communication.
- Switch management default gateways are set for switch access purposes.

## Task

Configure the topology in Cisco Packet Tracer, apply the above configurations, and test connectivity between PCs in different VLANs (e.g., ping PC13 to PC23) to verify Inter-VLAN routing. Use `show vlan brief`, `show ip route`, `show interfaces trunk`, and `ping` for troubleshooting.

### Multilayer Switch

A Layer 3 switch (multilayer switch) performs both Layer 2 switching and Layer 3 routing internally, using Switched Virtual Interfaces (SVIs) or routed ports to route traffic between VLANs.

**Information**:

- Highly scalable and efficient, ideal for medium to large enterprise networks.
- Eliminates the need for external routers, reducing latency and complexity.
- Supports advanced features like QoS, ACLs, and EtherChannel for increased bandwidth.
- More expensive than Layer 2 switches but often more cost-effective than separate routers and switches.

**Common CLI Commands (Cisco Packet Tracer)**:

```plaintext
! Layer 3 Switch Configuration
configure terminal
ip routing
vlan 10
 name VLAN10
vlan 20
 name VLAN20
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown
interface vlan 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown
interface fa0/1
 switchport mode access
 switchport access vlan 10
interface fa0/2
 switchport mode access
 switchport access vlan 20
```

**Notes**: Enable IP routing with `ip routing`. SVIs serve as default gateways for each VLAN. Access ports assign devices to VLANs.

**Given**:

- **IP Address Range**:
    - Overall Network: 172.16.0.0/22 (255.255.252.0)
    - VLAN 10 (STAFF): 172.16.0.0/24 (172.16.0.0–172.16.0.255)
    - VLAN 20 (STUDENT): 172.16.1.0/24 (172.16.1.0–172.16.1.255)
    - VLAN 30 (GUEST): 172.16.2.0/24 (172.16.2.0–172.16.2.255)
- **Subnet Mask**: 255.255.255.0 (/24) for each VLAN
- **Default Gateway (for PCs)**:
    - VLAN 10: 172.16.0.1
    - VLAN 20: 172.16.1.1
    - VLAN 30: 172.16.2.1
- **VLAN Groups**:
    - VLAN 10: PC1–PC11 (MLS, ACCESS)
    - VLAN 20: PC12–PC22 (MLS, ACCESS)
    - VLAN 30: PC23–PC32 (MLS, ACCESS)
- **Devices**:
    - 32 PCs (PC1–PC32)
    - 1 Multilayer Switch (MLS)
    - 1 Access Switch (ACCESS)
- **Total Devices**: 34 (32 PCs + 1 Multilayer Switch + 1 Access Switch)

**Topology Description**:

- **MLS (Multilayer Switch)**:
    - Vlan10: IP 172.16.0.1/24, SVI for VLAN 10
    - Vlan20: IP 172.16.1.1/24, SVI for VLAN 20
    - Vlan30: IP 172.16.2.1/24, SVI for VLAN 30
    - Fa0/1–Fa0/5: To PC1–PC5 (access VLAN 10)
    - Fa0/6–Fa0/10: To PC12–PC16 (access VLAN 20)
    - Fa0/11–Fa0/15: To PC23–PC27 (access VLAN 30)
    - Fa0/24: Trunk to ACCESS Fa0/24, allowed VLANs 10,20,30
- **ACCESS (Switch)**:
    - Fa0/1–Fa0/6: To PC6–PC11 (access VLAN 10)
    - Fa0/7–Fa0/12: To PC17–PC22 (access VLAN 20)
    - Fa0/13–Fa0/17: To PC28–PC32 (access VLAN 30)
    - Fa0/24: Trunk to MLS Fa0/24, allowed VLANs 10,20,30
    - Management: ip default-gateway 172.16.0.1

**Sample PC IP Assignments**:

- PC1 (VLAN 10, MLS): 172.16.0.2/24, Gateway 172.16.0.1
- PC6 (VLAN 10, ACCESS): 172.16.0.7/24, Gateway 172.16.0.1
- PC11 (VLAN 10, ACCESS): 172.16.0.12/24, Gateway 172.16.0.1
- PC12 (VLAN 20, MLS): 172.16.1.2/24, Gateway 172.16.1.1
- PC17 (VLAN 20, ACCESS): 172.16.1.7/24, Gateway 172.16.1.1
- PC22 (VLAN 20, ACCESS): 172.16.1.12/24, Gateway 172.16.1.1
- PC23 (VLAN 30, MLS): 172.16.2.2/24, Gateway 172.16.2.1
- PC28 (VLAN 30, ACCESS): 172.16.2.7/24, Gateway 172.16.2.1
- PC32 (VLAN 30, ACCESS): 172.16.2.11/24, Gateway 172.16.2.1

**CLI Configuration**:

**MLS Switch Configuration**:

```
configure terminal
ip routing
vlan 10
 name STAFF
vlan 20
 name STUDENT
vlan 30
 name GUEST
interface vlan 10
 ip address 172.16.0.1 255.255.255.0
 no shutdown
interface vlan 20
 ip address 172.16.1.1 255.255.255.0
 no shutdown
interface vlan 30
 ip address 172.16.2.1 255.255.255.0
 no shutdown
interface range FastEthernet0/1 - 5
 switchport mode access
 switchport access vlan 10
interface range FastEthernet0/6 - 10
 switchport mode access
 switchport access vlan 20
interface range FastEthernet0/11 - 15
 switchport mode access
 switchport access vlan 30
interface FastEthernet0/24
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
exit
```

**ACCESS Switch Configuration**:

```
configure terminal
vlan 10
 name STAFF
vlan 20
 name STUDENT
vlan 30
 name GUEST
interface range FastEthernet0/1 - 6
 switchport mode access
 switchport access vlan 10
interface range FastEthernet0/7 - 12
 switchport mode access
 switchport access vlan 20
interface range FastEthernet0/13 - 17
 switchport mode access
 switchport access vlan 30
interface FastEthernet0/24
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
ip default-gateway 172.16.0.1
exit
```

**Notes**:

- PCs use the SVI IP as their default gateway (e.g., 172.16.0.1 for VLAN 10 PCs).
- IP routing is enabled on MLS to handle Inter-VLAN Routing internally.
- The ACCESS switch is Layer 2, relying on MLS for routing.
- Sample PC IPs are illustrative; adjust for specific device needs.
- Topology assumes a balanced PC distribution; modify port assignments if required.

**Task**: Configure the topology in Cisco Packet Tracer, apply the above configurations, and test connectivity (e.g., ping PC1 to PC23) to verify Inter-VLAN Routing. Use `show vlan brief`, `show ip route`, `show running-config`, and `ping` for troubleshooting.

## Security Considerations

**Access Control**:

- **Issue**: Unrestricted Inter-VLAN communication can undermine VLAN isolation.
- **Solution**: Implement Access Control Lists (ACLs) to restrict traffic (e.g., block HTTP from guest VLAN to corporate VLAN).
- **Note**: Test ACLs thoroughly to prevent blocking legitimate traffic or allowing unauthorized access.

**VLAN Hopping**:

- **Issue**: Attackers exploit VLAN tags (e.g., double-tagging) to access unauthorized VLANs.
- **Mitigation**: Disable Dynamic Trunking Protocol (DTP), avoid default VLANs (e.g., VLAN 1), restrict trunk VLANs, and secure trunk ports.

**Firewalls and Segmentation**:

- Use firewalls to inspect and filter Inter-VLAN traffic.
- Maintain strict segmentation between sensitive (e.g., STAFF) and non-sensitive (e.g., GUEST) VLANs.

**Traffic Monitoring**:

- Use tools like NetFlow, SNMP, or JumpCloud’s Cloud RADIUS for real-time monitoring of Inter-VLAN traffic to detect anomalies.

## Performance Considerations

**Routing vs. Switching**: Layer 3 routing involves more processing than Layer 2 switching, but multilayer switches use hardware-based routing for minimal latency.

**Scalability**:

- Traditional/Legacy: Limited by physical interfaces.
- Router-on-a-Stick: Limited by trunk link bandwidth (moderate scalability).
- Multilayer Switch: Excellent scalability for large networks.

**Traffic Load**: High Inter-VLAN traffic (e.g., VoIP, video streaming) may require QoS policies to prioritize critical data and EtherChannel to increase trunk bandwidth.

## Real-World Applications

- **Departmental Collaboration**: Segregates departments (e.g., STAFF, STUDENT) while allowing access to shared resources like file servers or CRM systems.
- **Guest VLANs**: Isolates guest users from internal networks while providing controlled internet access.
- **IoT Integration**: Enables secure communication between IoT devices and corporate networks while maintaining isolation.
- **Hybrid Cloud**: Routes traffic between on-premises VLANs and cloud resources for seamless hybrid infrastructure.
- **SDN Integration**: Supports dynamic VLAN configurations in software-defined networks (SDN) for optimized routing.

## Troubleshooting Inter-VLAN Routing

**Common Issues**:

- **Misconfigured VLAN IDs**: Ensure consistent VLAN IDs across switches, routers, and subinterfaces.
- **Routing Table Errors**: Verify correct routes to all VLAN subnets on the router or Layer 3 switch.
- **IP Address Misassignments**: Avoid overlapping IP ranges by assigning unique subnets to each VLAN.

**Diagnostic Tools**:

- **Ping**: Test connectivity between VLANs.
- **Traceroute**: Identify packet drop points.
- **ARP Tables**: Diagnose address resolution issues.
- **Show Commands** (Cisco): Use `show vlan`, `show ip route`, or `show interfaces` to verify configurations.

**Proactive Measures**: Conduct periodic audits and use monitoring tools to prevent issues.

## Comparing Routing Methods

|**Criteria**|**Traditional/Legacy**|**Router-on-a-Stick**|**Multilayer Switch**|
|---|---|---|---|
|**Cost**|Low|Low|High|
|**Performance**|Low (CPU-based)|Moderate (CPU-based)|High (hardware-based)|
|**Scalability**|Poor|Moderate (~50 VLANs)|Excellent|
|**Complexity**|Simple|Simple|Moderate|
|**Best Use Case**|Small networks|Small-medium networks|Medium-large networks|

## Best Practices

- Use multilayer switches for large networks to maximize performance and scalability.
- Simplify configurations to streamline troubleshooting and maintenance.
- Implement and test ACLs to balance security and functionality.
- Assign unused ports to a dummy VLAN and avoid default VLANs (e.g., VLAN 1).
- Use 802.1Q tagging and unique subnets for each VLAN to ensure proper traffic routing.
- Regularly monitor traffic with tools like JumpCloud’s Cloud RADIUS or Cisco’s NetFlow.
- Document VLAN and routing configurations for consistency and future reference.
- Integrate with modern solutions like SDN or JumpCloud’s Cloud RADIUS for dynamic and secure management.

## Summary

Inter-VLAN Routing enables controlled communication between VLANs, bridging isolated network segments to support enterprise collaboration and resource sharing. It operates at Layer 3, using routers or multilayer switches with 802.1Q tagging and subnetting. The three methods—Traditional/Legacy, Router-on-a-Stick, and Multilayer Switch—suit different network sizes:

- **Traditional/Legacy**: Simple but outdated, limited by physical interfaces.
- **Router-on-a-Stick**: Cost-effective for small to medium networks but prone to bottlenecks.
- **Multilayer Switch**: Scalable and efficient for large enterprises.

Security is paramount, requiring ACLs, firewalls, secure trunk configurations, and monitoring to mitigate risks like VLAN hopping. Applications range from departmental collaboration to IoT and hybrid cloud integration. By following best practices and leveraging tools like JumpCloud’s Cloud RADIUS, Inter-VLAN Routing can be implemented securely and efficiently to meet modern network demands.