# Spanning Tree Protocol (STP) Notes

### Introduction to Spanning Tree Protocol (STP)

#### Overview and Purpose

The Spanning Tree Protocol (STP) is a Layer 2 network protocol designed to prevent loops in Ethernet networks, particularly in environments with redundant paths, such as switched or bridged LANs. Developed by Radia Perlman in 1985, STP ensures a loop-free topology by creating a single active path between any two network nodes while blocking redundant paths unless they’re needed (e.g., in case of a failure). It’s defined in the IEEE 802.1D standard. Redundant paths enhance reliability by providing failover options, but without proper management, they can lead to loops that destabilize the network. STP mitigates this risk, making it a critical tool for maintaining network stability.

#### Why Loops Are Bad

Loops in a network can cause broadcast storms, where Ethernet frames (especially broadcasts) circulate endlessly, overwhelming switches, consuming bandwidth, and potentially leading to network failures. This scenario not only disrupts service but also creates vulnerabilities that could be exploited in denial-of-service (DoS) attacks. STP prevents this by blocking redundant paths while keeping them available as backups, ensuring a balance between stability and redundancy.

#### STP Features

STP offers several key features that enable its functionality:

- **Elects one Root Bridge**: A single switch acts as the central point, anchoring the spanning tree topology.
- **Blocks some links**: Disables redundant paths to prevent loops.
- **Keeps redundant links available**: Blocked links serve as backups, ready to activate if an active link fails.
- **Protocol Standard**: Follows the IEEE 802.1D standard.
- **Default Priority**: Set at 32768, used as the baseline in Root Bridge election.
- **Lowest MAC Address Wins**: If priorities are equal, the switch with the lowest MAC address becomes the Root Bridge, serving as a tiebreaker.

#### How STP Works

STP operates through a series of steps to maintain a loop-free topology:

1. **Root Bridge Election**:
    
    - All switches in the network compete to become the Root Bridge.
    - The switch with the lowest Bridge ID wins, where the Bridge ID is a combination of a priority value (default 32768) and the switch’s MAC address (unique). If priorities are equal, the lowest MAC address prevails. For example, in a topology where all switches have the default priority of 32768, the switch with the lowest MAC address (e.g., Switch 1 with MAC 0001.4256.C1CC) becomes the Root Bridge.
2. **Path Cost Calculation**:
    
    - Each switch determines the least-cost path to the Root Bridge based on link bandwidth. Lower bandwidth links have higher costs (e.g., 100 for 10 Mbps, 19 for 100 Mbps, 4 for 1 Gbps), ensuring STP chooses the most efficient paths.
3. **Port Roles and States**:
    
    - **Root Port (RP)**: The port on a non-root switch with the best (lowest-cost) path to the Root Bridge.
    - **Designated Port (DP)**: The port on a network segment that forwards traffic toward the Root Bridge.
    - **Blocked/Alternate Port**: Ports placed in a blocking state to prevent loops; they listen for BPDUs but do not forward data, remaining as backups.

### Why STP Matters in Network Security

In the context of network security, STP plays a critical role by maintaining network stability and availability. Loops in a network can lead to broadcast storms, where frames endlessly circulate, consuming bandwidth and potentially crashing the network—a scenario ripe for exploitation in denial-of-service (DoS) attacks. By preventing loops, STP reduces vulnerabilities that could be leveraged to disrupt services, ensuring the network remains operational and secure.

### STP in Action

Imagine a network with three switches (S1, S2, S3) connected in a triangle:

- Without STP, a broadcast frame could loop indefinitely between S1, S2, and S3, overwhelming the network.
- With STP enabled, the protocol elects a root bridge (e.g., S1) and blocks one link (say, between S2 and S3), breaking the loop while keeping the network connected via the root bridge.

### STP Security

While STP is essential for preventing loops, it introduces security risks if not properly configured. By default, STP trusts all switches in the network, making it susceptible to manipulation by malicious devices.

#### STP Security Risks

Key vulnerabilities include:

- **Root Bridge Hijacking**: An attacker connects a switch with a lower Bridge ID (e.g., by setting a low priority), making it the new root bridge. This can redirect traffic through the attacker’s switch, enabling data interception or manipulation.
- **Topology Changes**: An attacker can send malicious BPDUs to force frequent topology recalculations, causing network instability and potential downtime.

#### Common Attacks

1. **Root Bridge Hijacking**: The attacker introduces a switch with a lower Bridge ID, taking over as the root bridge and altering the network topology.
2. **Topology Change Attacks**: By sending BPDUs with topology change notifications, an attacker triggers repeated STP recalculations, disrupting network performance.

#### How to Secure STP

To harden STP against these threats, several security features can be implemented:

1. **BPDU Guard**:
    
    - **Description**: Blocks ports if BPDUs are received on access ports, preventing unauthorized switches from participating in STP.
        
    - **Configuration**:
        
        ```
        conf t
        interface range fa0/1
        spanning-tree portfast
        spanning-tree bpduguard enable
        end
        ```
        
    - **Use Case**: Apply to ports connected to end devices (e.g., PCs, printers), not to ports linked to other switches, to stop rogue devices from influencing STP.
        
    - **Example Outcome**: In a scenario where Switch 1 (Fa0/4) sends BPDUs normally, Switch 2 (Fa0/4) with PortFast and BPDU Guard enabled receives a BPDU and shuts down into an `err-disabled` state to prevent a potential loop. Only Switch 2 is affected because it has BPDU Guard configured.
        
2. **Root Guard**:
    
    - **Description**: Prevents a port from becoming the root port. If a superior BPDU (indicating a better Bridge ID) is received, the port enters a "root-inconsistent" state until the issue is resolved.
        
    - **Configuration**:
        
        ```
        conf t
        interface range fa0/2 - 3
        spanning-tree guard root
        end
        ```
        
    - **Use Case**: Use on ports facing downstream switches to ensure the designated root bridge retains control.
        
    - **Example Outcome**: On Switch 3 (Fa0/3) and Switch 4 (Fa0/1), Root Guard detects superior BPDUs from devices attempting to claim Root Bridge status. The ports enter a `root-inconsistent` state, blocking them to protect the current Root Bridge (e.g., Switch 1).
        
3. **PortFast**:
    
    - **Description**: Allows ports to bypass the Listening and Learning states, transitioning immediately to Forwarding, which reduces startup time for end devices.
        
    - **Configuration**:
        
        ```
        Switch(config)# interface fa0/3
        Switch(config-if)# spanning-tree portfast
        ```
        
    - **Use Case**: Enable only on access ports connected to end devices (e.g., PCs, printers) to avoid accidental loops if connected to switches.
        
4. **BPDU Filter**:
    
    - **Description**: Suppresses sending and receiving BPDUs on a port, effectively disabling STP communication on that interface.
        
    - **Configuration**:
        
        ```
        Switch(config)# interface fa0/3
        Switch(config-if)# spanning-tree bpdufilter enable
        ```
        
    - **Use Case**: Use cautiously on ports where STP is unnecessary (e.g., end devices), as it can create loops if misapplied to switch-to-switch links.
        
5. **Loop Guard**:
    
    - **Description**: Protects against loops caused by unidirectional link failures (e.g., a fiber optic cable failing in one direction) by placing affected ports into a "loop-inconsistent" state.
        
    - **Configuration**:
        
        ```
        Switch(config)# interface fa0/4
        Switch(config-if)# spanning-tree guard loop
        ```
        
    - **Use Case**: Apply to non-edge links (between switches) where STP must remain active to prevent loops.
        

**Discussion**: These security features enhance STP’s resilience. Root Guard ensures the intended Root Bridge remains in control by blocking superior BPDUs, while BPDU Guard protects access ports from rogue switches, preventing accidental loops or attacks. Together, they make STP more robust in untrusted environments.

**Additional Security Tip**: To ensure the intended switch remains the root bridge, manually configure its bridge priority:

```
Switch(config)# spanning-tree vlan 1 priority 4096
```

This sets a low priority (e.g., 4096 vs. the default 32768), making it more likely to be elected as the root bridge.

**Example: Best Practice for a Port Connected to a PC** For a secure configuration on a port connected to a PC:

```
interface FastEthernet0/1
 description Connected to PC
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
```

- `switchport mode access`: Ensures the port operates as an access port, not a trunk.
- `spanning-tree portfast`: Speeds up connectivity for the PC.
- `spanning-tree bpduguard enable`: Shuts down the port if a BPDU is received, preventing unauthorized switches.

### STP Configuration and Verification

#### Verification Commands

To monitor and troubleshoot STP on Cisco switches, use the following commands:

- `show interface vlan 1`: Displays the MAC address of the switch’s VLAN 1 interface, useful for identifying the Bridge ID.
- `show version`: Shows switch details, including the base MAC address.
- `show spanning-tree`: Reveals the Root Bridge, port roles (RP, DP, Blocked), and STP status—a go-to command for an STP overview.
- `show run`: Displays the full running configuration, letting you see STP settings in context.
- `show interface status`: Lists the status and VLAN assignments of all interfaces.
- `show running-config | section interface FastEthernet0/x`: Zooms in on a specific interface’s config (e.g., FastEthernet0/1).
- `show spanning-tree inconsistentports`: Identifies ports in an inconsistent state, often due to STP protection features like Root Guard.
- `show errdisable recovery`: Shows settings for recovering from the `err-disabled` state (e.g., after BPDU Guard triggers).
- `show cdp neighbors`: Maps directly connected Cisco devices, helping you visualize the topology.

These commands are a toolkit for network admins to monitor STP, troubleshoot issues, and confirm the network matches the intended design.

#### Configuring the Root Bridge

To configure a switch as the Root Bridge, adjust its priority to a value lower than the default (32768):

```
en
conf t
spanning-tree vlan 1 priority 4096
end
wr
```

- **Steps Explained**:
    - `en`: Enters privileged EXEC mode.
    - `conf t`: Enters global configuration mode.
    - `spanning-tree vlan 1 priority 4096`: Sets the priority for VLAN 1 to 4096, a lower value than 32768, increasing the likelihood of becoming the Root Bridge.
    - `end`: Exits configuration mode.
    - `wr`: Saves the configuration to startup-config.
- **Priority Details**: STP priority values must be multiples of 4096 (e.g., 0, 4096, 8192, up to 61440). In this example, the switch with priority 4096 becomes the Root Bridge, overriding the MAC address tiebreaker.

**Discussion**: These tools bridge theory and practice. Commands like `show spanning-tree` offer visibility into the spanning tree, while setting a lower priority allows admins to optimize the topology by choosing a high-performance switch as the root. This control is key for enhancing network performance and ensuring stability.

### STP Versions

STP has evolved over time, with various versions enhancing its functionality:

- **STP**: Original IEEE 802.1D standard, providing basic loop prevention with slower convergence (30-50 seconds).
- **RSTP**: Rapid STP (IEEE 802.1w), offers faster convergence (a few seconds) by improving port state transitions.
- **MST**: Multiple STP (IEEE 802.1s), VLAN-aware, allowing multiple spanning tree instances to optimize traffic across VLANs.
- **PVST+**: Cisco proprietary Per-VLAN Spanning Tree Plus, runs a separate STP instance for each VLAN, enhancing flexibility.
- **RPVST+**: Cisco’s Rapid PVST+, combines PVST+ with RSTP’s rapid convergence for per-VLAN optimization.

### Network Topology (Common to All Scenarios)

#### Topology Overview

The network consists of three core switches forming a triangle, connected to PCs and attacker switches:

- **Switch0 (CIT)**: 2960-24TT, MAC: 0001.4256.C1CC, IP: 192.168.100.1/24
- **Switch1 (COS)**: 2960-24T, MAC: 0030.F222.963C, IP: 192.168.100.11/24
- **Switch2 (IRTC)**: 2960-24T, IP: 192.168.100.12/24

#### Inter-Switch Connections

- **CIT Gigabit 0/1** connects to **COS Gigabit 0/1** (trunk)
- **CIT Gigabit 0/2** connects to **IRTC Gigabit 0/1** (trunk)
- **COS Gigabit 0/2** connects to **IRTC Gigabit 0/2** (trunk)

#### End Devices

- **PC0**: Connected to `FastEthernet0/1` on CIT, IP: 192.168.100.2/24
- **PC1**: Connected to `FastEthernet0/1` on COS, IP: 192.168.100.3/24
- **PC2**: Connected to `FastEthernet0/1` on IRTC, IP: 192.168.100.4/24

All PCs are in VLAN 1 (default) and the same subnet (`192.168.100.0/24`), enabling Layer 2 communication.

#### Attacker Devices (Introduced in Scenarios)

- **ATTACKER-A** (Scenario 1): Connected to IRTC Fa0/2, default priority 32768
- **ATTACKER-B** (Scenario 2): Connected to IRTC Fa0/2, priority 4096
- **ATTACKER-C1** (Scenario 3): Connected to CIT Fa0/2, priority 4096
- **ATTACKER-C2** (Scenario 3): Connected to COS Fa0/2, priority 4096
- **ATTACKER-C3** (Scenario 3): Connected to IRTC Fa0/2, priority 4096

---

### Practical Example

### Scenario 1: Default Configuration
![[scenario1-switchportsec.png]]
#### Setup

- **Objective**: Demonstrate STP behavior with default settings, introducing `ATTACKER-A` (default priority 32768) without disrupting CIT as the root bridge.
- **Security**: None applied; all switches use default STP settings.
- **Root Bridge**: CIT wins due to the lowest MAC address (0001.4256.C1CC) among default priorities (32768 + VLAN ID = 32769 for VLAN 1).

#### Step-by-Step Configuration

1. **Enable STP with Default Settings**
    
    - **Purpose**: Allow STP to operate with its default priority (32768) and elect a root bridge based on MAC addresses.
        
    - **Configuration** (apply to CIT, COS, IRTC; ATTACKER-A defaults are assumed):
        
        ```
        spanning-tree mode ieee
        ```
        
    - **Note**: This is typically enabled by default on Cisco switches.
        
2. **Configure Access Ports for PCs**
    
    - **Purpose**: Connect PCs to switches without additional STP features.
        
    - **Configuration** (apply to CIT, COS, IRTC):
        
        ```
        interface fa0/1
         switchport mode access
         description Connected to PC
        ```
        
3. **Introduce ATTACKER-A**
    
    - **Purpose**: Simulate an external switch joining the network with default settings.
        
    - **Configuration** (on ATTACKER-A, assumed):
        
        ```
        interface fa0/1
         switchport mode access
        ```
        
    - **Connection**: ATTACKER-A’s `Fa0/1` connects to `IRTC Fa0/2`.
        

#### Complete CLI Configurations

**CIT Configuration:**

```
CIT> enable
CIT# configure terminal
CIT(config)# spanning-tree mode ieee
CIT(config)# interface fa0/1
CIT(config-if)# switchport mode access
CIT(config-if)# description Connected to PC0
CIT(config-if)# exit
CIT(config)# end
CIT# write memory
```

**COS Configuration:**

```
COS> enable
COS# configure terminal
COS(config)# spanning-tree mode ieee
COS(config)# interface fa0/1
COS(config-if)# switchport mode access
COS(config-if)# description Connected to PC1
COS(config-if)# exit
COS(config)# end
COS# write memory
```

**IRTC Configuration:**

```
IRTC> enable
IRTC# configure terminal
IRTC(config)# spanning-tree mode ieee
IRTC(config)# interface fa0/1
IRTC(config-if)# switchport mode access
IRTC(config-if)# description Connected to PC2
IRTC(config-if)# exit
IRTC(config)# interface fa0/2
IRTC(config-if)# switchport mode access
IRTC(config-if)# description Connected to ATTACKER-A
IRTC(config-if)# exit
IRTC(config)# end
IRTC# write memory
```

**ATTACKER-A Configuration (Assumed):**

```
ATTACKER-A> enable
ATTACKER-A# configure terminal
ATTACKER-A(config)# spanning-tree mode ieee
ATTACKER-A(config)# interface fa0/1
ATTACKER-A(config-if)# switchport mode access
ATTACKER-A(config-if)# exit
ATTACKER-A(config)# end
ATTACKER-A# write memory
```

#### Summarized CLI Commands

**CIT:**

```
spanning-tree mode ieee
interface fa0/1
 switchport mode access
 description Connected to PC0
```

**COS:**

```
spanning-tree mode ieee
interface fa0/1
 switchport mode access
 description Connected to PC1
```

**IRTC:**

```
spanning-tree mode ieee
interface fa0/1
 switchport mode access
 description Connected to PC2
interface fa0/2
 switchport mode access
 description Connected to ATTACKER-A
```

**ATTACKER-A:**

```
spanning-tree mode ieee
interface fa0/1
 switchport mode access
```

#### Behavior

- **STP Topology**: CIT is root (Priority 32769, lowest MAC `0001.4256.C1CC`). IRTC blocks `Gi0/2` (to COS). ATTACKER-A’s `Fa0/1` is its Root Port toward CIT via IRTC.
- **Ping Tests**: PC0 ↔ PC1, PC0 ↔ PC2, PC1 ↔ PC2 succeed via CIT as the central hub.
- **With ATTACKER-A**: Joins without disrupting CIT’s root status; pings remain successful.

#### Verification

- `CIT# show spanning-tree vlan 1`: Root ID matches CIT’s MAC and Priority 32769.

### Scenario 2: Without Security
![[scenario2-switchportsec.png]]
#### Setup

- **Objective**: Show STP vulnerability when `ATTACKER-B` (priority 4096) hijacks the root bridge role.
- **Security**: None applied; default settings remain except for ATTACKER-B’s priority.
- **Root Bridge**: ATTACKER-B takes over due to its lower priority (4096 + VLAN ID = 4097).

#### Step-by-Step Configuration

1. **Enable STP with Default Settings**
    
    - **Purpose**: Maintain default STP behavior on CIT, COS, and IRTC.
        
    - **Configuration** (apply to CIT, COS, IRTC):
        
        ```
        spanning-tree mode ieee
        ```
        
2. **Configure Access Ports for PCs**
    
    - **Purpose**: Connect PCs without security features.
        
    - **Configuration** (apply to CIT, COS, IRTC):
        
        ```
        interface fa0/1
         switchport mode access
         description Connected to PC
        ```
        
3. **Configure ATTACKER-B with Low Priority**
    
    - **Purpose**: Set ATTACKER-B as the root bridge.
        
    - **Configuration** (on ATTACKER-B):
        
        ```
        spanning-tree vlan 1 priority 4096
        interface fa0/1
         switchport mode access
        ```
        
    - **Connection**: ATTACKER-B’s `Fa0/1` connects to `IRTC Fa0/2`.
        

#### Complete CLI Configurations

**CIT Configuration:**

```
CIT> enable
CIT# configure terminal
CIT(config)# spanning-tree mode ieee
CIT(config)# interface fa0/1
CIT(config-if)# switchport mode access
CIT(config-if)# description Connected to PC0
CIT(config-if)# exit
CIT(config)# end
CIT# write memory
```

**COS Configuration:**

```
COS> enable
COS# configure terminal
COS(config)# spanning-tree mode ieee
COS(config)# interface fa0/1
COS(config-if)# switchport mode access
COS(config-if)# description Connected to PC1
COS(config-if)# exit
COS(config)# end
COS# write memory
```

**IRTC Configuration:**

```
IRTC> enable
IRTC# configure terminal
IRTC(config)# spanning-tree mode ieee
IRTC(config)# interface fa0/1
IRTC(config-if)# switchport mode access
IRTC(config-if)# description Connected to PC2
IRTC(config-if)# exit
IRTC(config)# interface fa0/2
IRTC(config-if)# switchport mode access
IRTC(config-if)# description Connected to ATTACKER-B
IRTC(config-if)# exit
IRTC(config)# end
IRTC# write memory
```

**ATTACKER-B Configuration:**

```
ATTACKER-B> enable
ATTACKER-B# configure terminal
ATTACKER-B(config)# spanning-tree mode ieee
ATTACKER-B(config)# spanning-tree vlan 1 priority 4096
ATTACKER-B(config)# interface fa0/1
ATTACKER-B(config-if)# switchport mode access
ATTACKER-B(config-if)# exit
ATTACKER-B(config)# end
ATTACKER-B# write memory
```

#### Summarized CLI Commands

**CIT:**

```
spanning-tree mode ieee
interface fa0/1
 switchport mode access
 description Connected to PC0
```

**COS:**

```
spanning-tree mode ieee
interface fa0/1
 switchport mode access
 description Connected to PC1
```

**IRTC:**

```
spanning-tree mode ieee
interface fa0/1
 switchport mode access
 description Connected to PC2
interface fa0/2
 switchport mode access
 description Connected to ATTACKER-B
```

**ATTACKER-B:**

```
spanning-tree mode ieee
spanning-tree vlan 1 priority 4096
interface fa0/1
 switchport mode access
```

#### Behavior

- **STP Topology**: ATTACKER-B is root (Priority 4097). IRTC’s `Fa0/2` is Root Port, COS blocks `Gi0/1` (to CIT).
- **Ping Tests**: PC0 ↔ PC1, PC0 ↔ PC2, PC1 ↔ PC2 succeed but may route through ATTACKER-B, exposing traffic.
- **With ATTACKER-B**: Hijacks root, altering paths and potentially intercepting traffic.

#### Verification

- `IRTC# show spanning-tree vlan 1`: Root ID shows ATTACKER-B’s MAC and Priority 4097.

### Scenario 3: Securing the Network
![[scenario3-switchportsec.png]]
#### Setup

- **Objective**: Prevent `ATTACKER-C1`, `ATTACKER-C2`, and `ATTACKER-C3` (each with priority 4096) from hijacking STP or disrupting the network, while ensuring connectivity between PC0, PC1, and PC2.
- **Security Features**:
    - BPDU Guard on access ports to disable them upon receiving BPDUs from attacker switches.
    - Root Guard on CIT-C’s inter-switch trunk ports (Gig0/1 and Gig0/2) to prevent superior BPDUs from altering the root bridge.
- **Root Bridge**: CIT-C is the root bridge, relying on its default priority (32768 + VLAN ID = 32769) and lowest MAC address (0001.4256.C1CC).

#### Topology Details

- **Connections to Attacker Switches**:
    - ATTACKER-C1 Gig0/1 → CIT-C Fa0/2
    - ATTACKER-C2 Gig0/1 → COS-C Fa0/2
    - ATTACKER-C3 Gig0/1 → IRTC-C Fa0/2
- **Connections to Legitimate PCs**:
    - PC0 Fa0 → CIT-C Fa0/1
    - PC1 Fa0 → COS-C Fa0/1
    - PC2 Fa0 → IRTC-C Fa0/1
- **Inter-Switch Links**:
    - CIT-C Gig0/1 → COS-C Gig0/1 (trunk)
    - CIT-C Gig0/2 → IRTC-C Gig0/1 (trunk)
    - COS-C Gig0/2 → IRTC-C Gig0/2 (trunk)

#### Step-by-Step Configuration

1. **Apply Root Guard on CIT-C Inter-Switch Ports Only**
    
    - **Purpose**: Protect CIT-C against superior BPDUs from external switches claiming root status on trunk ports.
        
    - **Configuration** (apply to CIT-C only):
        
        ```
        interface range gi0/1 - 2
         spanning-tree guard root
        ```
        
2. **Configure Access Ports for Legitimate PCs**
    
    - **Purpose**: Enable fast connectivity with PortFast and protect against unauthorized BPDUs using BPDU Guard.
        
    - **Configuration** (apply to CIT-C Fa0/1, COS-C Fa0/1, IRTC-C Fa0/1):
        
        ```
        interface fa0/1
         switchport mode access
         spanning-tree portfast
         spanning-tree bpduguard enable
         description Connected to PC
        ```
        
3. **Configure Access Ports for Attacker Switches**
    
    - **Purpose**: Prevent attacker switches from participating in STP by shutting down ports upon BPDU receipt.
        
    - **Configuration** (apply to CIT-C Fa0/2, COS-C Fa0/2, IRTC-C Fa0/2):
        
        ```
        interface fa0/2
         switchport mode access
         spanning-tree bpduguard enable
         description Connected to ATTACKER-C
        ```
        
4. **Secure Unused Access Ports**
    
    - **Purpose**: Protect unused ports from unauthorized access or STP manipulation.
        
    - **Configuration** (apply to CIT-C Fa0/3-24, COS-C Fa0/3-24, IRTC-C Fa0/3-24):
        
        ```
        interface range fa0/3 - 24
         switchport mode access
         spanning-tree portfast
         spanning-tree bpduguard enable
        ```
        
5. **Monitor and Verify**
    
    - **Commands**:
        - `show spanning-tree vlan 1`: Verify CIT-C is the root bridge (based on default priority and MAC address).
        - `show interfaces status | include err`: Check for ports in `err-disabled` state due to BPDU Guard.
        - `show spanning-tree inconsistentports`: Ensure no root-inconsistent states on CIT-C’s inter-switch ports.

#### Complete CLI Configurations

**CIT-C Configuration:**

```
CIT-C> enable
CIT-C# configure terminal
CIT-C(config)# interface range gi0/1 - 2
CIT-C(config-if-range)# switchport mode trunk
CIT-C(config-if-range)# switchport trunk allowed vlan 1
CIT-C(config-if-range)# spanning-tree guard root
CIT-C(config-if-range)# exit
CIT-C(config)# interface fa0/1
CIT-C(config-if)# switchport mode access
CIT-C(config-if)# spanning-tree portfast
CIT-C(config-if)# spanning-tree bpduguard enable
CIT-C(config-if)# description Connected to PC0
CIT-C(config-if)# exit
CIT-C(config)# interface fa0/2
CIT-C(config-if)# switchport mode access
CIT-C(config-if)# spanning-tree bpduguard enable
CIT-C(config-if)# description Connected to ATTACKER-C1
CIT-C(config-if)# exit
CIT-C(config)# interface range fa0/3 - 24
CIT-C(config-if-range)# switchport mode access
CIT-C(config-if-range)# spanning-tree portfast
CIT-C(config-if-range)# spanning-tree bpduguard enable
CIT-C(config-if-range)# exit
CIT-C(config)# end
CIT-C# write memory
```

**COS-C Configuration:**

```
COS-C> enable
COS-C# configure terminal
COS-C(config)# interface range gi0/1 - 2
COS-C(config-if-range)# switchport mode trunk
COS-C(config-if-range)# switchport trunk allowed vlan 1
COS-C(config-if-range)# exit
COS-C(config)# interface fa0/1
COS-C(config-if)# switchport mode access
COS-C(config-if)# spanning-tree portfast
COS-C(config-if)# spanning-tree bpduguard enable
COS-C(config-if)# description Connected to PC1
COS-C(config-if)# exit
COS-C(config)# interface fa0/2
COS-C(config-if)# switchport mode access
COS-C(config-if)# spanning-tree bpduguard enable
COS-C(config-if)# description Connected to ATTACKER-C2
COS-C(config-if)# exit
COS-C(config)# interface range fa0/3 - 24
COS-C(config-if-range)# switchport mode access
COS-C(config-if-range)# spanning-tree portfast
COS-C(config-if-range)# spanning-tree bpduguard enable
COS-C(config-if-range)# exit
COS-C(config)# end
COS-C# write memory
```

**IRTC-C Configuration:**

```
IRTC-C> enable
IRTC-C# configure terminal
IRTC-C(config)# interface range gi0/1 - 2
IRTC-C(config-if-range)# switchport mode trunk
IRTC-C(config-if-range)# switchport trunk allowed vlan 1
IRTC-C(config-if-range)# exit
IRTC-C(config)# interface fa0/1
IRTC-C(config-if)# switchport mode access
IRTC-C(config-if)# spanning-tree portfast
IRTC-C(config-if)# spanning-tree bpduguard enable
IRTC-C(config-if)# description Connected to PC2
IRTC-C(config-if)# exit
IRTC-C(config)# interface fa0/2
IRTC-C(config-if)# switchport mode access
IRTC-C(config-if)# spanning-tree bpduguard enable
IRTC-C(config-if)# description Connected to ATTACKER-C3
IRTC-C(config-if)# exit
IRTC-C(config)# interface range fa0/3 - 24
IRTC-C(config-if-range)# switchport mode access
IRTC-C(config-if-range)# spanning-tree portfast
IRTC-C(config-if-range)# spanning-tree bpduguard enable
IRTC-C(config-if-range)# exit
IRTC-C(config)# end
IRTC-C# write memory
```

#### Summarized CLI Commands

**CIT-C:**

```
interface range gi0/1 - 2
 switchport mode trunk
 switchport trunk allowed vlan 1
 spanning-tree guard root
interface fa0/1
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 description Connected to PC0
interface fa0/2
 switchport mode access
 spanning-tree bpduguard enable
 description Connected to ATTACKER-C1
interface range fa0/3 - 24
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
```

**COS-C:**

```
interface range gi0/1 - 2
 switchport mode trunk
 switchport trunk allowed vlan 1
interface fa0/1
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 description Connected to PC1
interface fa0/2
 switchport mode access
 spanning-tree bpduguard enable
 description Connected to ATTACKER-C2
interface range fa0/3 - 24
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
```

**IRTC-C:**

```
interface range gi0/1 - 2
 switchport mode trunk
 switchport trunk allowed vlan 1
interface fa0/1
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 description Connected to PC2
interface fa0/2
 switchport mode access
 spanning-tree bpduguard enable
 description Connected to ATTACKER-C3
interface range fa0/3 - 24
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
```

#### Behavior

- **Without Attacker Switches**:
    - CIT-C is the root bridge (Priority 32769, lowest MAC 0001.4256.C1CC).
    - Pings succeed: PC0 ↔ PC1, PC0 ↔ PC2, PC1 ↔ PC2.
- **With Attacker Switches**:
    - **Access Ports** (e.g., CIT-C Fa0/2, COS-C Fa0/2, IRTC-C Fa0/2): BPDU Guard triggers `err-disabled` state when attacker switches (priority 4096) send BPDUs, isolating them while legitimate PC connectivity remains intact.
    - **Inter-Switch Ports on CIT-C** (Gig0/1-2): Root Guard prevents superior BPDUs from ATTACKER-C switches from altering the root bridge, maintaining CIT-C as root.

#### Verification

- `CIT-C# show spanning-tree vlan 1`: Confirms CIT-C is the root bridge (Priority 32769, MAC 0001.4256.C1CC).
- `CIT-C# show interfaces status | include err`: Displays ports in `err-disabled` state (e.g., Fa0/2 if ATTACKER-C1 sends BPDUs).
- `CIT-C# show spanning-tree inconsistentports`: Verifies no root-inconsistent states on CIT-C’s inter-switch ports.

### Summary of All Scenarios

- **Scenario 1**: Default settings; CIT is root, ATTACKER-A joins harmlessly, pings succeed.
- **Scenario 2**: No security; ATTACKER-B hijacks root, pings succeed but traffic is vulnerable.
- **Scenario 3**: Secured with BPDU Guard and Root Guard; CIT remains root, ATTACKER-C switches are blocked, pings succeed safely.

### Overall Discussion

STP ensures stability in Ethernet networks by preventing loops while maintaining redundancy. It achieves this by electing a Root Bridge, assigning port roles, and blocking redundant paths, balancing reliability with loop prevention. Verification commands like `show spanning-tree` provide transparency into the spanning tree, allowing admins to monitor and troubleshoot effectively. Security features such as Root Guard and BPDU Guard make STP robust against misconfigurations or attacks by protecting the Root Bridge and securing access ports. Together, these tools and features equip network professionals to deploy, monitor, and secure STP effectively, ensuring stable, efficient, and secure networks.

### Q&A Section

This section addresses common questions about STP, covering its purpose, operation, security, configuration, and verification.

1. **What is the primary purpose of STP?**
    
    STP prevents loops in Ethernet networks with redundant paths by creating a loop-free topology, ensuring network stability and preventing broadcast storms.
    
2. **How does STP elect the Root Bridge?**
    
    STP elects the Root Bridge based on the lowest Bridge ID, which combines a priority value (default 32768) and the switch’s MAC address. If priorities are equal, the switch with the lowest MAC address wins.
    
3. **What happens if a network has loops and STP is not enabled?**
    
    Without STP, loops can cause broadcast storms, where frames circulate endlessly, overwhelming switches and leading to network failures, potentially exploited in DoS attacks.
    
4. **What is the role of a Blocked Port in STP?**
    
    A Blocked (or Alternate) Port prevents loops by not forwarding data traffic but listens for BPDUs and remains a backup, ready to activate if an active link fails.
    
5. **How can you secure STP against Root Bridge hijacking?**
    
    Use Root Guard on inter-switch ports to prevent them from becoming Root Ports if they receive superior BPDUs, ensuring the intended Root Bridge retains control.
    
6. **What does BPDU Guard do, and where should it be applied?**
    
    BPDU Guard shuts down a port if it receives a BPDU, preventing unauthorized switches from participating in STP. It should be applied to access ports connected to end devices like PCs or printers.
    
7. **How do you configure a switch to be the Root Bridge for VLAN 1?**
    
    Set a lower priority than the default (32768) using:
    
    ```
    en
    conf t
    spanning-tree vlan 1 priority 4096
    end
    wr
    ```
    
    Priority values must be multiples of 4096.
    
8. **What command can you use to verify the Root Bridge in an STP topology?**
    
    Use `show spanning-tree` to display the Root Bridge, port roles, and STP status, confirming the Root Bridge’s identity and priority.
    
9. **What happens if a port with BPDU Guard enabled receives a BPDU?**
    
    The port shuts down and enters an `err-disabled` state, as seen in the example where Switch 2 (Fa0/4) shuts down after receiving a BPDU from Switch 1.
    
10. **Why should PortFast only be enabled on access ports?**
    
    PortFast skips STP’s Listening and Learning states, immediately forwarding traffic. Enabling it on switch-to-switch links can create loops if BPDUs are not properly managed, so it’s safest on ports connected to end devices.