# Spanning Tree Protocol (STP) Notes

### Introduction to Spanning Tree Protocol (STP)

The Spanning Tree Protocol is a Layer 2 network protocol designed to prevent loops in Ethernet networks, particularly in environments with redundant paths, such as switched or bridged LANs. Developed by Radia Perlman in 1985, STP ensures a loop-free topology by creating a single active path between any two network nodes, while blocking redundant paths unless they’re needed (e.g., in case of a failure). It’s defined in the IEEE 802.1D standard.

### Why STP Matters in Network Security

In the context of network security, STP plays a critical role by maintaining network stability and availability. Loops in a network can lead to broadcast storms, where frames endlessly circulate, consuming bandwidth and potentially crashing the network—a scenario ripe for exploitation in denial-of-service (DoS) attacks. By preventing loops, STP reduces vulnerabilities that could be leveraged to disrupt services.

### How STP Works (Basic Mechanism)

STP operates through a series of steps to maintain a loop-free topology:

1. **Root Bridge Election**: All switches in the network elect a single "root bridge" based on the lowest Bridge ID, which is a combination of a priority value (configurable) and the switch’s MAC address (unique).
    
2. **Path Cost Calculation**: Each switch determines the least-cost path to the root bridge using link speed and other metrics (e.g., 100 for 10 Mbps, 19 for 100 Mbps).
    
3. **Port Roles and States**:
    
    - **Root Port**: The port on a non-root switch with the best (lowest-cost) path to the root bridge.
        
    - **Designated Port**: The port on a network segment that forwards traffic toward the root bridge.
        
    - **Blocked Port**: Ports placed in a blocking state to prevent loops, which listen for BPDUs but do not forward data.
        
4. **BPDU Exchange**: Switches exchange Bridge Protocol Data Units (BPDUs) to share topology information, elect the root bridge, and detect changes like link failures.
    

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
        Switch(config)# interface range fa0/1 - 24
        Switch(config-if-range)# spanning-tree bpduguard enable
        ```
        
    - **Use Case**: Apply to ports connected to end devices (e.g., PCs, printers), not to ports linked to other switches, to stop rogue devices from influencing STP.
        
2. **Root Guard**:
    
    - **Description**: Prevents a port from becoming the root port. If a superior BPDU (indicating a better Bridge ID) is received, the port enters a "root-inconsistent" state until the issue is resolved.
        
    - **Configuration**:
        
        ```
        Switch(config)# interface fa0/2
        Switch(config-if)# spanning-tree guard root
        ```
        
    - **Use Case**: Use on ports facing downstream switches to ensure the designated root bridge retains control.
        
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
        

**Additional Security Tip**: To ensure the intended switch remains the root bridge, manually configure its bridge priority:

```
Switch(config)# spanning-tree vlan 1 priority 4096
```

This sets a low priority (e.g., 4096 vs. the default 32768), making it more likely to be elected as the root bridge.

**Example: Best Practice for a Port Connected to a PC** For a secure configuration on a port connected to a PC:

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
    

### STP Versions

STP has evolved over time, with various versions enhancing its functionality:

- **STP**: Original IEEE 802.1D standard, providing basic loop prevention with slower convergence (30-50 seconds).
    
- **RSTP**: Rapid STP (IEEE 802.1w), offers faster convergence (a few seconds) by improving port state transitions.
    
- **MST**: Multiple STP (IEEE 802.1s), VLAN-aware, allowing multiple spanning tree instances to optimize traffic across VLANs.
    
- **PVST+**: Cisco proprietary Per-VLAN Spanning Tree Plus, runs a separate STP instance for each VLAN, enhancing flexibility.
    
- **RPVST+**: Cisco’s Rapid PVST+, combines PVST+ with RSTP’s rapid convergence for per-VLAN optimization.
    

### Network Topology (Common to All Scenarios)
- **CIT Gigabit 0/1** connects to **IRTC Gigabit 0/1**
- **CIT Gigabit 0/2** connects to **COS Gigabit 0/1**
- **COS Gigabit 0/2** connects to **IRTC Gigabit 0/2**

**End Devices:**
- **PC0**: Connected to `FastEthernet0/1` on CIT, IP: `192.168.100.2/24`.
- **PC1**: Connected to `FastEthernet0/1` on COS, IP: `192.168.100.3/24`.
- **PC2**: Connected to `FastEthernet0/1` on IRTC, IP: `192.168.100.4/24`.

All PCs are in VLAN 1 (default) and the same subnet (`192.168.100.0/24`), enabling Layer 2 communication.

---

### Scenario 1: Default Configuration

#### Setup
- **Objective**: Demonstrate STP behavior with default settings, introducing `ATTACKER-A` (default priority 32768) without disrupting CIT as the root bridge.
- **Security**: None applied; all switches use default STP settings.
- **Root Bridge**: CIT wins due to the lowest MAC address among default priorities.

#### Step-by-Step Configuration

1. **Enable STP with Default Settings**
   - **Purpose**: Allow STP to operate with its default priority (32768) and elect a root bridge based on MAC addresses.
   - **Configuration** (apply to CIT, COS, IRTC; ATTACKER-A defaults are assumed):
     ```plaintext
     spanning-tree mode ieee
     ```
   - **Note**: This is typically enabled by default on Cisco switches.

2. **Configure Access Ports for PCs**
   - **Purpose**: Connect PCs to switches without additional STP features.
   - **Configuration** (apply to CIT, COS, IRTC):
     ```plaintext
     interface fa0/1
      switchport mode access
      description Connected to PC
     ```

3. **Introduce ATTACKER-A**
   - **Purpose**: Simulate an external switch joining the network with default settings.
   - **Configuration** (on ATTACKER-A, assumed):
     ```plaintext
     interface fa0/1
      switchport mode access
     ```
   - **Connection**: ATTACKER-A’s `Fa0/1` connects to `IRTC Fa0/2`.

#### Complete CLI Configurations

**CIT Configuration:**
```plaintext
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
```plaintext
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
```plaintext
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
```plaintext
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
```plaintext
spanning-tree mode ieee
interface fa0/1
 switchport mode access
 description Connected to PC0
```

**COS:**
```plaintext
spanning-tree mode ieee
interface fa0/1
 switchport mode access
 description Connected to PC1
```

**IRTC:**
```plaintext
spanning-tree mode ieee
interface fa0/1
 switchport mode access
 description Connected to PC2
interface fa0/2
 switchport mode access
 description Connected to ATTACKER-A
```

**ATTACKER-A:**
```plaintext
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

---

### Scenario 2: Without Security

#### Setup
- **Objective**: Show STP vulnerability when `ATTACKER-B` (priority 4096) hijacks the root bridge role.
- **Security**: None applied; default settings remain except for ATTACKER-B’s priority.
- **Root Bridge**: ATTACKER-B takes over due to its lower priority.

#### Step-by-Step Configuration

1. **Enable STP with Default Settings**
   - **Purpose**: Maintain default STP behavior on CIT, COS, and IRTC.
   - **Configuration** (apply to CIT, COS, IRTC):
     ```plaintext
     spanning-tree mode ieee
     ```

2. **Configure Access Ports for PCs**
   - **Purpose**: Connect PCs without security features.
   - **Configuration** (apply to CIT, COS, IRTC):
     ```plaintext
     interface fa0/1
      switchport mode access
      description Connected to PC
     ```

3. **Configure ATTACKER-B with Low Priority**
   - **Purpose**: Set ATTACKER-B as the root bridge.
   - **Configuration** (on ATTACKER-B):
     ```plaintext
     spanning-tree vlan 1 priority 4096
     interface fa0/1
      switchport mode access
     ```
   - **Connection**: ATTACKER-B’s `Fa0/1` connects to `IRTC Fa0/2`.

#### Complete CLI Configurations

**CIT Configuration:**
```plaintext
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
```plaintext
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
```plaintext
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
```plaintext
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
```plaintext
spanning-tree mode ieee
interface fa0/1
 switchport mode access
 description Connected to PC0
```

**COS:**
```plaintext
spanning-tree mode ieee
interface fa0/1
 switchport mode access
 description Connected to PC1
```

**IRTC:**
```plaintext
spanning-tree mode ieee
interface fa0/1
 switchport mode access
 description Connected to PC2
interface fa0/2
 switchport mode access
 description Connected to ATTACKER-B
```

**ATTACKER-B:**
```plaintext
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

---

### Scenario 3: Securing the Network

I understand the need to revise the Scenario 3 notes again based on your clarification. The key updates are:

1. **Remove `spanning-tree guard root` from COS-C and IRTC-C Gigabit interfaces**: Only CIT-C should have Root Guard on its Gigabit interfaces (Gig0/1 and Gig0/2).
2. **Remove `spanning-tree vlan 1 priority 0` from CIT-C**: This command should not be implemented, meaning CIT-C will rely on its default priority (32768 + VLAN ID) or a manually set priority if specified elsewhere, but we’ll exclude this command as per your instruction.

I’ll update the configurations accordingly, ensuring CIT-C remains the root bridge through other means (e.g., its default priority or MAC address being the lowest in the topology, as is often the case in such scenarios), and that the attacker switches (ATTACKER-C1, ATTACKER-C2, ATTACKER-C3) are isolated using BPDU Guard. The topology remains the same as previously described, with the specified port connections for PCs and attacker switches.

---

### Scenario 3: Securing the Network

#### Setup
- **Objective**: Prevent `ATTACKER-C1`, `ATTACKER-C2`, and `ATTACKER-C3` (each with priority 4096) from hijacking Spanning Tree Protocol (STP) or disrupting the network, while ensuring connectivity between PC0, PC1, and PC2.
- **Security Features**:
  - BPDU Guard on access ports to disable them upon receiving BPDUs from attacker switches.
  - Root Guard on CIT-C’s inter-switch trunk ports (Gig0/1 and Gig0/2) to prevent superior BPDUs from altering the root bridge.
- **Root Bridge**: CIT-C is the root bridge

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

---

#### Step-by-Step Configuration

1. **Apply Root Guard on CIT-C Inter-Switch Ports Only**
   - **Purpose**: Protect CIT-C against superior BPDUs from external switches claiming root status on trunk ports.
   - **Configuration** (apply to CIT-C only):
     ```plaintext
     interface range gi0/1 - 2
      spanning-tree guard root
     ```

2. **Configure Access Ports for Legitimate PCs**
   - **Purpose**: Enable fast connectivity with PortFast and protect against unauthorized BPDUs using BPDU Guard.
   - **Configuration** (apply to CIT-C Fa0/1, COS-C Fa0/1, IRTC-C Fa0/1):
     ```plaintext
     interface fa0/1
      switchport mode access
      spanning-tree portfast
      spanning-tree bpduguard enable
      description Connected to PC
     ```

3. **Configure Access Ports for Attacker Switches**
   - **Purpose**: Prevent attacker switches from participating in STP by shutting down ports upon BPDU receipt.
   - **Configuration** (apply to CIT-C Fa0/2, COS-C Fa0/2, IRTC-C Fa0/2):
     ```plaintext
     interface fa0/2
      switchport mode access
      spanning-tree bpduguard enable
      description Connected to ATTACKER-C
     ```

4. **Secure Unused Access Ports**
   - **Purpose**: Protect unused ports from unauthorized access or STP manipulation.
   - **Configuration** (apply to CIT-C Fa0/3-24, COS-C Fa0/3-24, IRTC-C Fa0/3-24):
     ```plaintext
     interface range fa0/3 - 24
      switchport mode access
      spanning-tree portfast
      spanning-tree bpduguard enable
     ```

5. **Monitor and Verify**
   - **Commands**:
     - `show spanning-tree vlan 1`: Verify CIT-C is the root bridge (based on default priority or MAC address).
     - `show interfaces status | include err`: Check for ports in `err-disabled` state due to BPDU Guard.
     - `show spanning-tree inconsistentports`: Ensure no root-inconsistent states on CIT-C’s inter-switch ports.

---

#### Complete CLI Configurations

**CIT-C Configuration:**
```plaintext
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
```plaintext
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
```plaintext
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

---

#### Summarized CLI Commands

**CIT-C:**
```plaintext
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
```plaintext
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
```plaintext
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

---

#### Behavior
- **Without Attacker Switches**:
  - CIT-C is the root bridge (assumed via default priority 32768 + VLAN ID or lowest MAC address).
  - Pings succeed: PC0 ↔ PC1, PC0 ↔ PC2, PC1 ↔ PC2.
- **With Attacker Switches**:
  - **Access Ports** (e.g., CIT-C Fa0/2, COS-C Fa0/2, IRTC-C Fa0/2): BPDU Guard triggers `err-disabled` if attacker switches send BPDUs, isolating them while legitimate PC connectivity remains intact.
  - **Inter-Switch Ports on CIT-C** (Gig0/1-2): Root Guard prevents superior BPDUs from altering the root bridge, maintaining CIT-C as root.

---

#### Verification
- `CIT-C# show spanning-tree vlan 1`: Confirms CIT-C is the root bridge (based on default priority or MAC address).
- `CIT-C# show interfaces status | include err`: Displays any ports in `err-disabled` state (e.g., Fa0/2 if ATTACKER-C1 sends BPDUs).
- `CIT-C# show spanning-tree inconsistentports`: Verifies no root-inconsistent states on CIT-C’s inter-switch ports.


---

### Summary of All Scenarios
- **Scenario 1**: Default settings; CIT is root, ATTACKER-A joins harmlessly, pings succeed.
- **Scenario 2**: No security; ATTACKER-B hijacks root, pings succeed but traffic is vulnerable.
- **Scenario 3**: Secured with BPDU Guard and Root Guard; CIT remains root, ATTACKER-C is blocked, pings succeed safely.
