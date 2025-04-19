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

## Traditional/Legacy

Each VLAN connects to a dedicated physical interface on a router, which routes traffic between VLANs. The switch assigns access ports to specific VLANs, and each router interface acts as the VLAN’s gateway.

**Information**:

- Suitable for small networks with few VLANs (e.g., 2–3 VLANs).
- Each interface requires a unique IP address in the VLAN’s subnet.
- Not scalable due to the limited number of physical router interfaces, making it obsolete for modern networks with many VLANs.

**Common CLI Commands**:

```plaintext
! Switch Configuration (e.g., COS)
configure terminal
vlan 10
 name IT
interface fa0/1
 switchport mode access
 switchport access vlan 10
interface fa0/2
 switchport mode access
 switchport access vlan 10
interface fa0/3
 switchport mode access
 switchport access vlan 10

! Router Configuration (UITC)
configure terminal
interface gig0/0
 ip address 192.168.100.2 255.255.255.224
 no shutdown
interface gig1/0
 ip address 192.168.100.34 255.255.255.224
 no shutdown
```

**Notes**: Connect switch ports (e.g., COS Fa0/1) to router interfaces (e.g., UITC Gig0/0) for each VLAN. This method is rarely used today due to scalability limitations.

### Practical Example
![[traditional-topology.png]]
**Given**:

**IP Address Range**:

- Overall Network: 192.168.100.0/25 (255.255.255.128)
- VLAN 10 (IT): 192.168.100.0/27 (192.168.100.0–192.168.100.31)
- VLAN 20 (ADMIN): 192.168.100.32/27 (192.168.100.32–192.168.100.63)
- VLAN 30 (GUEST): 192.168.100.64/27 (192.168.100.64–192.168.100.95)
- VLAN 50 (IOT): 192.168.100.96/27 (192.168.100.96–192.168.100.127)

**Subnet Mask**: 255.255.255.224 (/27) for each VLAN

**Default Gateway (for PCs)**:

- VLAN 10: 192.168.100.2
- VLAN 20: 192.168.100.34
- VLAN 30: 192.168.100.66
- VLAN 50: 192.168.100.98

**VLAN Groups**:

- VLAN 10: PC0, PC1 (COS)
- VLAN 20: PC2, PC3 (CIT)
- VLAN 30: PC4, PC5, PC8, PC9 (CAFA, CIE)
- VLAN 50: PC6, PC7, PC10, PC12 (CAFA, CIE)

**Devices**:

- 12 PCs (PC0–PC5, PC6–PC8, PC9, PC10, PC12)
- 1 Router (UITC)
- 4 Switches (COS, CIT, CAFA, CIE)

**Total Devices**: 17 (12 PCs + 1 Router + 4 Switches)

**Topology Description**:

**UITC (Router)**:

- Gig0/0: IP 192.168.100.2/27, to COS Fa0/1 (access VLAN 10), speed auto, duplex auto
- Gig1/0: IP 192.168.100.34/27, to CIT Fa0/1 (access VLAN 20), speed 10 Mbps, duplex auto
- Gig2/0: IP 192.168.100.66/27, to CAFA Fa0/7 (access VLAN 30), speed auto, duplex auto
- Gig3/0: IP 192.168.100.98/27, to CAFA Fa0/5 (access VLAN 50), speed 1000 Mbps, duplex auto

**COS (Switch)**:

- Fa0/1: To UITC Gig0/0 (access VLAN 10)
- Fa0/2: To PC0 (access VLAN 10)
- Fa0/3: To PC1 (access VLAN 10)
- Management: ip default-gateway 192.168.100.2

**CIT (Switch)**:

- Fa0/1: To UITC Gig1/0 (access VLAN 20)
- Fa0/2: To PC2 (access VLAN 20)
- Fa0/3: To PC3 (access VLAN 20)
- Management: ip default-gateway 192.168.100.34

**CAFA (Switch)**:

- Fa0/1: To PC4 (access VLAN 30)
- Fa0/2: To PC5 (access VLAN 30)
- Fa0/3: To PC12 (access VLAN 50)
- Fa0/4: To PC6 (access VLAN 50)
- Fa0/5: To UITC Gig3/0 (access VLAN 50)
- Fa0/6: To CIE Fa0/6 (access VLAN 30)
- Fa0/7: To UITC Gig2/0 (access VLAN 30)
- Fa0/8: Unassigned
- Management: ip default-gateway 192.168.100.98

**CIE (Switch)**:

- Fa0/1: To PC8 (access VLAN 30)
- Fa0/2: To PC9 (access VLAN 30)
- Fa0/3: To PC10 (access VLAN 50)
- Fa0/4: Unassigned
- Fa0/5: Unassigned
- Fa0/6: To CAFA Fa0/6 (access VLAN 30)
- Management: ip default-gateway 192.168.100.98

**PC IP Assignments**:

- PC0 (VLAN 10, COS): 192.168.100.1/27, Gateway 192.168.100.2
- PC1 (VLAN 10, COS): 192.168.100.30/27, Gateway 192.168.100.2
- PC2 (VLAN 20, CIT): 192.168.100.33/27, Gateway 192.168.100.34
- PC3 (VLAN 20, CIT): 192.168.100.62/27, Gateway 192.168.100.34
- PC4 (VLAN 30, CAFA): 192.168.100.65/27, Gateway 192.168.100.66
- PC5 (VLAN 30, CAFA): 192.168.100.94/27, Gateway 192.168.100.66
- PC8 (VLAN 30, CIE): 192.168.100.67/27, Gateway 192.168.100.66
- PC9 (VLAN 30, CIE): 192.168.100.68/27, Gateway 192.168.100.66
- PC6 (VLAN 50, CAFA): 192.168.100.97/27, Gateway 192.168.100.98
- PC7 (VLAN 50, CAFA): 192.168.100.126/27, Gateway 192.168.100.98
- PC10 (VLAN 50, CIE): 192.168.100.125/27, Gateway 192.168.100.98
- PC12 (VLAN 50, CAFA): 192.168.100.99/27, Gateway 192.168.100.98

**CLI Configuration**:

**UITC Router**:

```plaintext
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

**COS Switch**:

```plaintext
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

**CIT Switch**:

```plaintext
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

**CAFA Switch**:

```plaintext
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
ip default-gateway 192.168.100.98
exit
```

**CIE Switch**:

```plaintext
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
interface FastEthernet0/6
 switchport mode access
 switchport access vlan 30
ip default-gateway 192.168.100.98
exit
```

**Notes**:

- PCs use the router’s interface IP as their default gateway for Inter-VLAN Routing (e.g., 192.168.100.66 for VLAN 30 PCs).
- Switch management gateways (e.g., CAFA’s 192.168.100.98) are for switch access, not PC routing.
- CAFA Fa0/8 is unassigned, reserved for future use.
- Subnet masks are /27, providing 32 addresses per VLAN (30 usable), sufficient for the assigned PCs.
- Topology assumes direct switch-to-router links, with CAFA and CIE sharing VLAN 30/50 traffic via Fa0/6.

**Task**: Configure the topology in Cisco Packet Tracer, apply the configurations, and test connectivity by pinging from PC0 (VLAN 10) to PC4 (VLAN 30). Use `show vlan brief`, `show ip route`, and `ping` to verify and troubleshoot.

## Router-on-a-Stick

A single router interface uses subinterfaces to route traffic between VLANs, with a trunk link to a switch carrying multiple VLANs using 802.1Q tagging.

**Information**:

- Cost-effective, requiring only one router interface.
- Suitable for small to medium networks (up to ~50 VLANs).
- Performance depends on the trunk link’s bandwidth, which can bottleneck with high traffic.
- Simplifies cabling compared to Traditional but requires careful subinterface configuration.

### Practical Example
![[routeronastick-topology.png]]
**Given**:
**IP Address Range**:
- Overall Network: 10.10.10.0/24 (255.255.255.0)
- VLAN 10: 10.10.10.0/26 (10.10.10.0–10.10.10.63)
- VLAN 20: 10.10.10.64/26 (10.10.10.64–10.10.10.127)
- VLAN 30: 10.10.10.128/26 (10.10.10.128–10.10.10.191)

**Subnet Mask**: 255.255.255.192 (/26) for each VLAN

**Default Gateway (for PCs)**:
- VLAN 10: 10.10.10.50
- VLAN 20: 10.10.10.100
- VLAN 30: 10.10.10.130

**VLAN Groups**:
- VLAN 10: PC13, PC14 (LIBRARY); PC19, PC20 (CANTEEN); PC25, PC26 (REGISTRAR)
- VLAN 20: PC15, PC16 (LIBRARY); PC21, PC22 (CANTEEN); PC27, PC28 (REGISTRAR)
- VLAN 30: PC17, PC18 (LIBRARY); PC23, PC24 (CANTEEN); PC29, PC30 (REGISTRAR)

**Devices**:
- 18 PCs (PC13–PC18 on LIBRARY, PC19–PC24 on CANTEEN, PC25–PC30 on REGISTRAR)
- 1 Router (ADMIN)
- 3 Switches (LIBRARY, CANTEEN, REGISTRAR)

**Total Devices**: 22 (18 PCs + 1 Router + 3 Switches)

**Topology Description**:
**ADMIN (Router)**:
- Gig0/0: No IP, trunk to CANTEEN Fa0/1, carrying VLANs 10, 20, 30
- Subinterfaces:
    - Gig0/0.10: IP 10.10.10.50/26, VLAN 10
    - Gig0/0.20: IP 10.10.10.100/26, VLAN 20
    - Gig0/0.30: IP 10.10.10.130/26, VLAN 30

**CANTEEN (Switch)**:
- Fa0/1: Trunk to ADMIN Gig0/0, allowed VLANs 10, 20, 30
- Fa0/2: Trunk to LIBRARY Fa0/1, allowed VLANs 10, 20, 30
- Fa0/3: Trunk to REGISTRAR Fa0/1, allowed VLANs 10, 20, 30
- Fa0/4: To PC19 (access VLAN 10)
- Fa0/5: To PC20 (access VLAN 10)
- Fa0/6: To PC21 (access VLAN 20)
- Fa0/7: To PC22 (access VLAN 20)
- Fa0/8: To PC23 (access VLAN 30)
- Fa0/9: To PC24 (access VLAN 30)

**LIBRARY (Switch)**:
- Fa0/1: Trunk to CANTEEN Fa0/2, allowed VLANs 10, 20, 30
- Fa0/2: To PC13 (access VLAN 10)
- Fa0/3: To PC14 (access VLAN 10)
- Fa0/4: To PC15 (access VLAN 20)
- Fa0/5: To PC16 (access VLAN 20)
- Fa0/6: To PC17 (access VLAN 30)
- Fa0/7: To PC18 (access VLAN 30)

**REGISTRAR (Switch)**:
- Fa0/1: Trunk to CANTEEN Fa0/3, allowed VLANs 10, 20, 30
- Fa0/2: To PC25 (access VLAN 10)
- Fa0/3: To PC26 (access VLAN 10)
- Fa0/4: To PC27 (access VLAN 20)
- Fa0/5: To PC28 (access VLAN 20)
- Fa0/6: To PC29 (access VLAN 30)
- Fa0/7: To PC30 (access VLAN 30)

**PC IP Assignments**:
- PC13 (VLAN 10, LIBRARY): 10.10.10.1/26, Gateway 10.10.10.50
- PC14 (VLAN 10, LIBRARY): 10.10.10.62/26, Gateway 10.10.10.50
- PC19 (VLAN 10, CANTEEN): 10.10.10.2/26, Gateway 10.10.10.50
- PC20 (VLAN 10, CANTEEN): 10.10.10.61/26, Gateway 10.10.10.50
- PC25 (VLAN 10, REGISTRAR): 10.10.10.3/26, Gateway 10.10.10.50
- PC26 (VLAN 10, REGISTRAR): 10.10.10.60/26, Gateway 10.10.10.50
- PC15 (VLAN 20, LIBRARY): 10.10.10.65/26, Gateway 10.10.10.100
- PC16 (VLAN 20, LIBRARY): 10.10.10.126/26, Gateway 10.10.10.100
- PC21 (VLAN 20, CANTEEN): 10.10.10.66/26, Gateway 10.10.10.100
- PC22 (VLAN 20, CANTEEN): 10.10.10.125/26, Gateway 10.10.10.100
- PC27 (VLAN 20, REGISTRAR): 10.10.10.67/26, Gateway 10.10.10.100
- PC28 (VLAN 20, REGISTRAR): 10.10.10.124/26, Gateway 10.10.10.100
- PC17 (VLAN 30, LIBRARY): 10.10.10.129/26, Gateway 10.10.10.130
- PC18 (VLAN 30, LIBRARY): 10.10.10.190/26, Gateway 10.10.10.130
- PC23 (VLAN 30, CANTEEN): 10.10.10.131/26, Gateway 10.10.10.130
- PC24 (VLAN 30, CANTEEN): 10.10.10.189/26, Gateway 10.10.10.130
- PC29 (VLAN 30, REGISTRAR): 10.10.10.132/26, Gateway 10.10.10.130
- PC30 (VLAN 30, REGISTRAR): 10.10.10.188/26, Gateway 10.10.10.130

**CLI Configuration**:
**ADMIN Router**:
```plaintext
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
 ip address 10.10.10.130 255.255.255.192
 no shutdown
exit
```

**CANTEEN Switch**:
```plaintext
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

**LIBRARY Switch**:
```plaintext
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

**REGISTRAR Switch**:
```plaintext
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
ip default-gateway 10.10.10.130
exit
```

**Notes**:
- The trunk between ADMIN and CANTEEN carries VLANs 10, 20, and 30, enabling the router to route traffic between these VLANs.
- Trunks between CANTEEN and LIBRARY/REGISTRAR allow VLAN traffic to span multiple switches, ensuring PCs in the same VLAN across different switches communicate at Layer 2.
- PCs use the router’s subinterface IP as their default gateway for Inter-VLAN communication.
- Switch management gateways are set for switch access, but PCs rely on subinterface IPs.
- VLAN 30’s subnet corrected to 10.10.10.128/26 to avoid overlap with VLAN 20.

**Task**: Configure the topology in Cisco Packet Tracer, apply the configurations, and test connectivity by pinging from PC13 (VLAN 10) to PC23 (VLAN 30). Use `show vlan brief`, `show ip route`, `show interfaces trunk`, and `ping` to verify and troubleshoot.

## Multilayer Switch

A Layer 3 switch (multilayer switch) performs both Layer 2 switching and Layer 3 routing internally, using Switched Virtual Interfaces (SVIs) to route traffic between VLANs.

**Information**:

- Highly scalable and efficient, ideal for medium to large enterprise networks.
- Eliminates the need for external routers, reducing latency and complexity.
- Supports advanced features like QoS, ACLs, and EtherChannel for increased bandwidth.
- More expensive than Layer 2 switches but often more cost-effective than separate routers and switches.

### Practical Example
![[multilayer-topology.png]]
**Given**:

**IP Address Range**:
- Overall Network: 172.16.150.0/22 (255.255.252.0)
- VLAN 100: 172.16.148.0/22 (172.16.148.0–172.16.151.255)
- VLAN 20: 172.16.152.0/22 (172.16.152.0–172.16.155.255)
- VLAN 300: 172.16.156.0/22 (172.16.156.0–172.16.159.255)
- VLAN 99: 172.16.160.0/22 (172.16.160.0–172.16.163.255)

**Subnet Mask**: 255.255.252.0 (/22) for each VLAN

**Default Gateway (for PCs)**:

- VLAN 100: 172.16.149.100
- VLAN 20: 172.16.153.100
- VLAN 300: 172.16.157.100
- VLAN 99: 172.16.161.100

**VLAN Groups**:

- VLAN 100: PC31, PC32 (FOODTECH); PC39, PC40 (MECHANICAL); PC47, PC48 (ELECTRONICS); PC55, PC56 (ELECTRICAL)
- VLAN 20: PC33, PC34 (FOODTECH); PC41, PC42 (MECHANICAL); PC49, PC50 (ELECTRONICS); PC57, PC58 (ELECTRICAL)
- VLAN 300: PC37, PC38 (FOODTECH); PC45, PC46 (MECHANICAL); PC53, PC54 (ELECTRONICS); PC59, PC60 (ELECTRICAL)
- VLAN 99: PC35, PC36 (FOODTECH); PC43, PC44 (MECHANICAL); PC51, PC52 (ELECTRONICS); PC61, PC62 (ELECTRICAL)

**Devices**:

- 32 PCs (PC31–PC62, distributed across four switches)
- 1 Multilayer Switch (DEAN)
- 4 Access Switches (ELECTRONICS, ELECTRICAL, MECHANICAL, FOODTECH)

**Total Devices**: 37 (32 PCs + 1 Multilayer Switch + 4 Access Switches)

**Topology Description**:

**DEAN (Multilayer Switch)**:

- Fa0/1: Trunk to ELECTRONICS Fa0/1, carrying VLANs 20, 99, 100, 300
- Fa0/2: Trunk to MECHANICAL Fa0/1, carrying VLANs 20, 99, 100, 300
- Fa0/3: Trunk to FOODTECH Fa0/1, carrying VLANs 20, 99, 100, 300
- Fa0/4: Trunk to ELECTRICAL Fa0/1, carrying VLANs 20, 99, 100, 300
- SVIs:
    - VLAN 100: IP 172.16.149.100/22
    - VLAN 20: IP 172.16.153.100/22
    - VLAN 300: IP 172.16.157.100/22
    - VLAN 99: IP 172.16.161.100/22
- Management: VLAN 1 shut down

**ELECTRONICS (Switch)**:

- Fa0/1: Trunk to DEAN Fa0/1, carrying VLANs 20, 99, 100, 300
- Fa0/2: To PC47 (access VLAN 100)
- Fa0/3: To PC48 (access VLAN 100)
- Fa0/4: To PC49 (access VLAN 20)
- Fa0/5: To PC50 (access VLAN 20)
- Fa0/6: To PC53 (access VLAN 300)
- Fa0/7: To PC54 (access VLAN 300)
- Fa0/8: To PC51 (access VLAN 99)
- Fa0/9: To PC52 (access VLAN 99)
- Management: No IP assigned

**ELECTRICAL (Switch)**:

- Fa0/1: Trunk to DEAN Fa0/4, carrying VLANs 20, 99, 100, 300
- Fa0/2: To PC55 (access VLAN 100)
- Fa0/3: To PC56 (access VLAN 100)
- Fa0/4: To PC57 (access VLAN 20)
- Fa0/5: To PC58 (access VLAN 20)
- Fa0/6: To PC59 (access VLAN 300)
- Fa0/7: To PC60 (access VLAN 300)
- Fa0/8: To PC61 (access VLAN 99)
- Fa0/9: To PC62 (access VLAN 99)
- Management: No IP assigned

**MECHANICAL (Switch)**:

- Fa0/1: Trunk to DEAN Fa0/2, carrying VLANs 20, 99, 100, 300
- Fa0/2: To PC39 (access VLAN 100)
- Fa0/3: To PC40 (access VLAN 100)
- Fa0/4: To PC41 (access VLAN 20)
- Fa0/5: To PC42 (access VLAN 20)
- Fa0/6: To PC45 (access VLAN 300)
- Fa0/7: To PC46 (access VLAN 300)
- Fa0/8: To PC43 (access VLAN 99)
- Fa0/9: To PC44 (access VLAN 99)
- Management: No IP assigned

**FOODTECH (Switch)**:

- Fa0/1: Trunk to DEAN Fa0/3, carrying VLANs 20, 99, 100, 300
- Fa0/2: To PC31 (access VLAN 100)
- Fa0/3: To PC32 (access VLAN 100)
- Fa0/4: To PC33 (access VLAN 20)
- Fa0/5: To PC34 (access VLAN 20)
- Fa0/6: To PC37 (access VLAN 300)
- Fa0/7: To PC38 (access VLAN 300)
- Fa0/8: To PC35 (access VLAN 99)
- Fa0/9: To PC36 (access VLAN 99)
- Management: No IP assigned

**PC IP Assignments**:

**VLAN 100**:

- PC31 (FOODTECH): 172.16.148.1/22, Gateway 172.16.149.100
- PC32 (FOODTECH): 172.16.151.254/22, Gateway 172.16.149.100
- PC39 (MECHANICAL): 172.16.150.1/22, Gateway 172.16.149.100
- PC40 (MECHANICAL): 172.16.150.254/22, Gateway 172.16.149.100
- PC47 (ELECTRONICS): 172.16.151.1/22, Gateway 172.16.149.100
- PC48 (ELECTRONICS): 172.16.149.254/22, Gateway 172.16.149.100
- PC55 (ELECTRICAL): 172.16.149.1/22, Gateway 172.16.149.100
- PC56 (ELECTRICAL): 172.16.148.254/22, Gateway 172.16.149.100

**VLAN 20**:

- PC33 (FOODTECH): 172.16.152.1/22, Gateway 172.16.153.100
- PC34 (FOODTECH): 172.16.155.254/22, Gateway 172.16.153.100
- PC41 (MECHANICAL): 172.16.153.1/22, Gateway 172.16.153.100
- PC42 (MECHANICAL): 172.16.154.254/22, Gateway 172.16.153.100
- PC49 (ELECTRONICS): 172.16.154.1/22, Gateway 172.16.153.100
- PC50 (ELECTRONICS): 172.16.153.254/22, Gateway 172.16.153.100
- PC57 (ELECTRICAL): 172.16.155.1/22, Gateway 172.16.153.100
- PC58 (ELECTRICAL): 172.16.152.254/22, Gateway 172.16.153.100

**VLAN 300**:

- PC37 (FOODTECH): 172.16.156.1/22, Gateway 172.16.157.100
- PC38 (FOODTECH): 172.16.159.254/22, Gateway 172.16.157.100
- PC45 (MECHANICAL): 172.16.157.1/22, Gateway 172.16.157.100
- PC46 (MECHANICAL): 172.16.158.254/22, Gateway 172.16.157.100
- PC53 (ELECTRONICS): 172.16.158.1/22, Gateway 172.16.157.100
- PC54 (ELECTRONICS): 172.16.157.254/22, Gateway 172.16.157.100
- PC59 (ELECTRICAL): 172.16.159.1/22, Gateway 172.16.157.100
- PC60 (ELECTRICAL): 172.16.156.254/22, Gateway 172.16.157.100

**VLAN 99**:

- PC35 (FOODTECH): 172.16.160.1/22, Gateway 172.16.161.100
- PC36 (FOODTECH): 172.16.163.254/22, Gateway 172.16.161.100
- PC43 (MECHANICAL): 172.16.161.1/22, Gateway 172.16.161.100
- PC44 (MECHANICAL): 172.16.162.254/22, Gateway 172.16.161.100
- PC51 (ELECTRONICS): 172.16.162.1/22, Gateway 172.16.161.100
- PC52 (ELECTRONICS): 172.16.161.254/22, Gateway 172.16.161.100
- PC61 (ELECTRICAL): 172.16.163.1/22, Gateway 172.16.161.100
- PC62 (ELECTRICAL): 172.16.160.254/22, Gateway 172.16.161.100

**CLI Configuration**:

**DEAN Multilayer Switch**:

```plaintext
configure terminal
hostname DEAN
ip routing
vlan 20
 name VLAN0020
vlan 99
 name VLAN0099
vlan 100
 name VLAN0100
vlan 300
 name VLAN0300
interface FastEthernet0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
interface FastEthernet0/2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
interface FastEthernet0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
interface FastEthernet0/4
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
interface Vlan1
 no ip address
 shutdown
interface Vlan20
 ip address 172.16.153.100 255.255.252.0
 no shutdown
interface Vlan99
 ip address 172.16.161.100 255.255.252.0
 no shutdown
interface Vlan100
 ip address 172.16.149.100 255.255.252.0
 no shutdown
interface Vlan300
 ip address 172.16.157.100 255.255.252.0
 no shutdown
exit
```

**ELECTRONICS Switch**:

```plaintext
configure terminal
hostname ELECTRONICS
vlan 20
 name VLAN0020
vlan 99
 name VLAN0099
vlan 100
 name VLAN0100
vlan 300
 name VLAN0300
interface FastEthernet0/1
 switchport mode trunk
 no shutdown
interface FastEthernet0/2
 switchport access vlan 100
 switchport mode access
 no shutdown
interface FastEthernet0/3
 switchport access vlan 100
 switchport mode access
 no shutdown
interface FastEthernet0/4
 switchport access vlan 20
 switchport mode access
 no shutdown
interface FastEthernet0/5
 switchport access vlan 20
 switchport mode access
 no shutdown
interface FastEthernet0/6
 switchport access vlan 300
 switchport mode access
 no shutdown
interface FastEthernet0/7
 switchport access vlan 300
 switchport mode access
 no shutdown
interface FastEthernet0/8
 switchport access vlan 99
 switchport mode access
 no shutdown
interface FastEthernet0/9
 switchport access vlan 99
 switchport mode access
 no shutdown
exit
```

**ELECTRICAL Switch**:

```plaintext
configure terminal
hostname ELECTRICAL
vlan 20
 name VLAN0020
vlan 99
 name VLAN0099
vlan 100
 name VLAN0100
vlan 300
 name VLAN0300
interface FastEthernet0/1
 switchport mode trunk
 no shutdown
interface FastEthernet0/2
 switchport access vlan 100
 switchport mode access
 no shutdown
interface FastEthernet0/3
 switchport access vlan 100
 switchport mode access
 no shutdown
interface FastEthernet0/4
 switchport access vlan 20
 switchport mode access
 no shutdown
interface FastEthernet0/5
 switchport access vlan 20
 switchport mode access
 no shutdown
interface FastEthernet0/6
 switchport access vlan 300
 switchport mode access
 no shutdown
interface FastEthernet0/7
 switchport access vlan 300
 switchport mode access
 no shutdown
interface FastEthernet0/8
 switchport access vlan 99
 switchport mode access
 no shutdown
interface FastEthernet0/9
 switchport access vlan 99
 switchport mode access
 no shutdown
exit
```

**MECHANICAL Switch**:

```plaintext
configure terminal
hostname MECHANICAL
vlan 20
 name VLAN0020
vlan 99
 name VLAN0099
vlan 100
 name VLAN0100
vlan 300
 name VLAN0300
interface FastEthernet0/1
 switchport mode trunk
 no shutdown
interface FastEthernet0/2
 switchport access vlan 100
 switchport mode access
 no shutdown
interface FastEthernet0/3
 switchport access vlan 100
 switchport mode access
 no shutdown
interface FastEthernet0/4
 switchport access vlan 20
 switchport mode access
 no shutdown
interface FastEthernet0/5
 switchport access vlan 20
 switchport mode access
 no shutdown
interface FastEthernet0/6
 switchport access vlan 300
 switchport mode access
 no shutdown
interface FastEthernet0/7
 switchport access vlan 300
 switchport mode access
 no shutdown
interface FastEthernet0/8
 switchport access vlan 99
 switchport mode access
 no shutdown
interface FastEthernet0/9
 switchport access vlan 99
 switchport mode access
 no shutdown
exit
```

**FOODTECH Switch**:

```plaintext
configure terminal
hostname FOODTECH
vlan 20
 name VLAN0020
vlan 99
 name VLAN0099
vlan 100
 name VLAN0100
vlan 300
 name VLAN0300
interface FastEthernet0/1
 switchport mode trunk
 no shutdown
interface FastEthernet0/2
 switchport access vlan 100
 switchport mode access
 no shutdown
interface FastEthernet0/3
 switchport access vlan 100
 switchport mode access
 no shutdown
interface FastEthernet0/4
 switchport access vlan 20
 switchport mode access
 no shutdown
interface FastEthernet0/5
 switchport access vlan 20
 switchport mode access
 no shutdown
interface FastEthernet0/6
 switchport access vlan 300
 switchport mode access
 no shutdown
interface FastEthernet0/7
 switchport access vlan 300
 switchport mode access
 no shutdown
interface FastEthernet0/8
 switchport access vlan 99
 switchport mode access
 no shutdown
interface FastEthernet0/9
 switchport access vlan 99
 switchport mode access
 no shutdown
exit
```

**Notes**:

- DEAN handles Inter-VLAN routing internally via SVIs, eliminating the need for an external router.
- Access switches operate at Layer 2, relying on DEAN for routing.
- The /22 subnet (1024 addresses, 1022 usable) supports all 32 PCs across VLANs, with logical blocks (e.g., 172.16.148.0/22 for VLAN 100) for clarity, though IPs can be assigned flexibly within 172.16.148.0–172.16.151.255.
- VLAN 89 was a topology mislabeling for VLAN 99, confirmed by running-configs.
- No management IPs on access switches; DEAN’s SVIs handle network access.
- Spanning-tree mode is PVST, ensuring a loop-free topology per VLAN.
- CLI commands include `no shutdown` for interfaces, aligning with Packet Tracer best practices.

**Task**: Configure the topology in Cisco Packet Tracer, apply the VLAN and IP configurations, ensuring trunks carry VLANs 20, 99, 100, 300. Assign IPs to PCs and set gateways to DEAN’s SVI IPs. Test Inter-VLAN routing by pinging from PC31 (VLAN 100) to PC37 (VLAN 300). Use `show vlan brief`, `show ip interface brief`, `show running-config`, and `ping` to verify and troubleshoot.

## Security Considerations

**Access Control**:

- **Issue**: Unrestricted Inter-VLAN communication can undermine VLAN isolation.
    
- **Solution**: Use Access Control Lists (ACLs) to restrict traffic. For example, in the Multilayer Switch topology, block non-management VLANs from accessing VLAN 99 (management):
    
    ```plaintext
    access-list 101 deny ip 172.16.148.0 0.0.3.255 172.16.160.0 0.0.3.255
    access-list 101 permit ip any any
    interface Vlan100
     ip access-group 101 in
    ```
    
- **Note**: Test ACLs to ensure legitimate traffic (e.g., VLAN 100 to VLAN 20) is not blocked while securing VLAN 99.
    

**VLAN Hopping**:

- **Issue**: Attackers may use double-tagging to access unauthorized VLANs.
- **Mitigation**: Disable DTP (`switchport nonegotiate`), avoid VLAN 1, restrict trunk VLANs (e.g., `switchport trunk allowed vlan 20,99,100,300`), and secure trunk ports.

**Firewalls and Segmentation**:

- Deploy firewalls to inspect Inter-VLAN traffic, especially for sensitive VLANs (e.g., VLAN 99 in Multilayer Switch).
- Maintain strict segmentation between VLANs (e.g., VLAN 300 for IoT vs. VLAN 100 for corporate).

**Traffic Monitoring**:

- Use NetFlow, SNMP, or similar tools to monitor Inter-VLAN traffic for anomalies, ensuring secure communication across VLANs.

## Performance Considerations

**Routing vs. Switching**: Layer 3 routing involves more processing than Layer 2 switching, but multilayer switches use hardware-based routing for minimal latency.

**Scalability**:

- Traditional/Legacy: Limited by physical interfaces (e.g., 4 VLANs max in UITC topology).
- Router-on-a-Stick: Limited by trunk bandwidth (moderate for 3 VLANs in CANTEEN setup).
- Multilayer Switch: Excellent scalability for large networks (e.g., 32 PCs across 4 VLANs in DEAN topology).

**Traffic Load**: High Inter-VLAN traffic (e.g., VoIP, video) may require QoS policies to prioritize critical data and EtherChannel to increase trunk bandwidth.

## Real-World Applications

- **Departmental Collaboration**: Segregates departments (e.g., VLAN 100 for corporate, VLAN 300 for IoT) while allowing access to shared resources.
- **Guest VLANs**: Isolates guests (e.g., VLAN 30 in Traditional) with controlled internet access.
- **IoT Integration**: Secures IoT devices (e.g., VLAN 50 in Traditional, VLAN 300 in Multilayer) while enabling corporate access.
- **Hybrid Cloud**: Routes traffic between on-premises VLANs and cloud resources.
- **SDN Integration**: Supports dynamic VLAN configurations in software-defined networks.

## Troubleshooting Inter-VLAN Routing

**Common Issues**:

- **Misconfigured VLAN IDs**: Ensure VLAN IDs match across devices (e.g., VLAN 100 on DEAN and ELECTRONICS).
- **Routing Table Errors**: Verify routes for all VLAN subnets (`show ip route`).
- **IP Address Misassignments**: Avoid overlaps (e.g., corrected VLAN 30 in Router-on-a-Stick to 10.10.10.128/26).

**Diagnostic Tools**:

- **Ping**: Test connectivity (e.g., PC0 to PC4, PC13 to PC23, PC31 to PC37).
- **Traceroute**: Identify packet drop points.
- **Show Commands**: Use `show vlan brief`, `show ip route`, `show interfaces trunk`, `show ip interface brief`.

**Proactive Measures**: Audit configurations regularly and monitor traffic to prevent issues.

## Comparing Routing Methods

|**Criteria**|**Traditional/Legacy**|**Router-on-a-Stick**|**Multilayer Switch**|
|---|---|---|---|
|**Cost**|Low|Low|High|
|**Performance**|Low (CPU-based)|Moderate (CPU-based)|High (hardware-based)|
|**Scalability**|Poor (17 devices)|Moderate (22 devices)|Excellent (37 devices)|
|**Complexity**|Simple|Simple|Moderate|
|**Best Use Case**|Small networks|Small-medium networks|Medium-large networks|

## Best Practices

- Use multilayer switches for large networks to maximize performance and scalability.
- Simplify configurations to streamline troubleshooting (e.g., consistent VLAN names).
- Implement and test ACLs to balance security and functionality.
- Assign unused ports to a dummy VLAN and avoid VLAN 1.
- Use 802.1Q tagging and unique subnets for each VLAN.
- Monitor traffic with tools like NetFlow or SNMP.
- Document configurations for consistency and reference.
- Integrate with modern solutions like SDN for dynamic management.

## Summary

Inter-VLAN Routing enables controlled communication between VLANs, bridging isolated segments for enterprise collaboration. It operates at Layer 3 using routers or multilayer switches with 802.1Q tagging and subnetting. The three methods suit different needs:

- **Traditional/Legacy**: Simple but outdated, limited by interfaces.
- **Router-on-a-Stick**: Cost-effective but bandwidth-limited.
- **Multilayer Switch**: Scalable and efficient for large networks.

Security requires ACLs, secure trunks, and monitoring to mitigate risks like VLAN hopping. Applications include departmental collaboration, guest VLANs, IoT, and hybrid cloud integration. Following best practices ensures secure and efficient Inter-VLAN Routing.