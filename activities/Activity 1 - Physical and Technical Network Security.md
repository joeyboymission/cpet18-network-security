# Activity 1: Physical and Technical Network Security

## Overview
This activity involves setting up a network in Cisco Packet Tracer with a focus on physical and technical security. The objectives include:
- Configuring the console port with a login password on switches and routers.
- Configuring the VTY lines for secure remote access (SSH) on switches and routers.
- Designing and mapping IP addresses to VLANs for network connectivity.
- Setting up VLAN 1 for SSH remote connections.
- Testing remote management of network devices.

The network consists of:
- **1 Router**: Router0
- **5 Switches**: Switch0, Switch1, Switch2, Switch3, Switch4
- **3 Servers**: Server0, Server1, Server2 (connected to Switch0)
- **11 PCs**: PC0–PC11 (distributed across Switch1–Switch4)

The topology is centralized by Router0, which connects to five switches, each managing a subnet with end devices (servers or PCs). All devices use VLAN 1 for simplicity, and SSH is configured for secure remote management.

---

## IP Addressing Table
Based on the provided topology, we’ll assign IP addresses to each device in the network. The topology uses five subnets (192.168.1.0/24 to 192.168.5.0/24), one for each switch branch. VLAN 1 is used across all switches for management and connectivity.

| Device      | Interface/Port | IP Address  | Subnet Mask   | Default Gateway | VLAN | Connected To   |
| ----------- | -------------- | ----------- | ------------- | --------------- | ---- | -------------- |
| **Router0** | G0/0           | 192.168.1.1 | 255.255.255.0 | -               | -    | Switch0 (G0/1) |
|             | G1/0           | 192.168.2.1 | 255.255.255.0 | -               | -    | Switch1 (G0/1) |
|             | G2/0           | 192.168.3.1 | 255.255.255.0 | -               | -    | Switch2 (G0/1) |
|             | G3/0           | 192.168.4.1 | 255.255.255.0 | -               | -    | Switch3 (G0/1) |
|             | G4/0           | 192.168.5.1 | 255.255.255.0 | -               | -    | Switch4 (G0/1) |
| **Switch0** | VLAN 1         | 192.168.1.2 | 255.255.255.0 | 192.168.1.1     | 1    | Router0 (G0/0) |
| **Switch1** | VLAN 1         | 192.168.2.2 | 255.255.255.0 | 192.168.2.1     | 1    | Router0 (G1/0) |
| **Switch2** | VLAN 1         | 192.168.3.2 | 255.255.255.0 | 192.168.3.1     | 1    | Router0 (G2/0) |
| **Switch3** | VLAN 1         | 192.168.4.2 | 255.255.255.0 | 192.168.4.1     | 1    | Router0 (G3/0) |
| **Switch4** | VLAN 1         | 192.168.5.2 | 255.255.255.0 | 192.168.5.1     | 1    | Router0 (G4/0) |
| **Server0** | F0             | 192.168.1.3 | 255.255.255.0 | 192.168.1.1     | 1    | Switch0 (F0/1) |
| **Server1** | F0             | 192.168.1.4 | 255.255.255.0 | 192.168.1.1     | 1    | Switch0 (F0/2) |
| **Server2** | F0             | 192.168.1.5 | 255.255.255.0 | 192.168.1.1     | 1    | Switch0 (F0/3) |
| **PC0**     | F0             | 192.168.2.3 | 255.255.255.0 | 192.168.2.1     | 1    | Switch1 (F0/1) |
| **PC1**     | F0             | 192.168.2.4 | 255.255.255.0 | 192.168.2.1     | 1    | Switch1 (F0/2) |
| **PC2**     | F0             | 192.168.2.5 | 255.255.255.0 | 192.168.2.1     | 1    | Switch1 (F0/3) |
| **PC3**     | F0             | 192.168.3.3 | 255.255.255.0 | 192.168.3.1     | 1    | Switch2 (F0/1) |
| **PC4**     | F0             | 192.168.3.4 | 255.255.255.0 | 192.168.3.1     | 1    | Switch2 (F0/2) |
| **PC5**     | F0             | 192.168.3.5 | 255.255.255.0 | 192.168.3.1     | 1    | Switch2 (F0/3) |
| **PC6**     | F0             | 192.168.4.3 | 255.255.255.0 | 192.168.4.1     | 1    | Switch3 (F0/1) |
| **PC7**     | F0             | 192.168.4.4 | 255.255.255.0 | 192.168.4.1     | 1    | Switch3 (F0/2) |
| **PC8**     | F0             | 192.168.4.5 | 255.255.255.0 | 192.168.4.1     | 1    | Switch3 (F0/3) |
| **PC9**     | F0             | 192.168.5.3 | 255.255.255.0 | 192.168.5.1     | 1    | Switch4 (F0/1) |
| **PC10**    | F0             | 192.168.5.4 | 255.255.255.0 | 192.168.5.1     | 1    | Switch4 (F0/2) |
| **PC11**    | F0             | 192.168.5.5 | 255.255.255.0 | 192.168.5.1     | 1    | Switch4 (F0/3) |

### Notes on IP Addressing
- **Subnets**: Each switch branch operates in a separate subnet:
  - Switch0: 192.168.1.0/24
  - Switch1: 192.168.2.0/24
  - Switch2: 192.168.3.0/24
  - Switch3: 192.168.4.0/24
  - Switch4: 192.168.5.0/24
- **VLAN**: All devices use VLAN 1 for connectivity and management.
- **Default Gateway**: The default gateway for each device matches the Router0 interface for its subnet (e.g., 192.168.2.1 for devices on Switch1).
- **IP Range**: Each subnet provides 254 usable addresses (e.g., 192.168.1.2–192.168.1.255 for Switch0’s subnet).

---

## Network Topology Overview
The network is centralized by Router0, which connects to five switches (Switch0–Switch4). Each switch manages a subnet with end devices (servers or PCs).

- **Router0**:
  - Connects to Switch0 (G0/0), Switch1 (G1/0), Switch2 (G2/0), Switch3 (G3/0), and Switch4 (G4/0).
- **Switch0**:
  - Connects to Server0, Server1, Server2.
  - Subnet: 192.168.1.0/24.
- **Switch1**:
  - Connects to PC0, PC1, PC2.
  - Subnet: 192.168.2.0/24.
- **Switch2**:
  - Connects to PC3, PC4, PC5.
  - Subnet: 192.168.3.0/24.
- **Switch3**:
  - Connects to PC6, PC7, PC8.
  - Subnet: 192.168.4.0/24.
- **Switch4**:
  - Connects to PC9, PC10, PC11.
  - Subnet: 192.168.5.0/24.

---

## CLI Configurations

### 1. Router Configuration (Router0)
Router0 acts as the central routing device, connecting all subnets and enabling inter-subnet communication.

#### Basic Setup
```bash
enable
configure terminal
hostname Router0
ip domain-name lab.local
username cisco secret cisco
enable secret cisco
```

#### Interface Configuration
Configure each Gigabit Ethernet interface to connect to the respective switch subnet.
```bash
interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
 exit
interface GigabitEthernet1/0
 ip address 192.168.2.1 255.255.255.0
 no shutdown
 exit
interface GigabitEthernet2/0
 ip address 192.168.3.1 255.255.255.0
 no shutdown
 exit
interface GigabitEthernet3/0
 ip address 192.168.4.1 255.255.255.0
 no shutdown
 exit
interface GigabitEthernet4/0
 ip address 192.168.5.1 255.255.255.0
 no shutdown
 exit
```

#### Routing Configuration
Set up static routes for inter-subnet routing.
```bash
ip route 192.168.1.0 255.255.255.0 GigabitEthernet0/0
ip route 192.168.2.0 255.255.255.0 GigabitEthernet1/0
ip route 192.168.3.0 255.255.255.0 GigabitEthernet2/0
ip route 192.168.4.0 255.255.255.0 GigabitEthernet3/0
ip route 192.168.5.0 255.255.255.0 GigabitEthernet4/0
```

#### Console and VTY Configuration for Secure Access
Configure console and VTY lines for secure access using SSH.
```bash
line console 0
 login local
 exit
line vty 0 15
 login local
 transport input ssh
 exit
crypto key generate rsa
1024
```

#### Save Configuration
```bash
write memory
```

#### Complete Configuration
```
enable
configure terminal
hostname Router0
ip domain-name lab.local
username cisco secret cisco
enable secret cisco
interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
 exit
interface GigabitEthernet1/0
 ip address 192.168.2.1 255.255.255.0
 no shutdown
 exit
interface GigabitEthernet2/0
 ip address 192.168.3.1 255.255.255.0
 no shutdown
 exit
interface GigabitEthernet3/0
 ip address 192.168.4.1 255.255.255.0
 no shutdown
 exit
interface GigabitEthernet4/0
 ip address 192.168.5.1 255.255.255.0
 no shutdown
 exit
ip route 192.168.1.0 255.255.255.0 GigabitEthernet0/0
ip route 192.168.2.0 255.255.255.0 GigabitEthernet1/0
ip route 192.168.3.0 255.255.255.0 GigabitEthernet2/0
ip route 192.168.4.0 255.255.255.0 GigabitEthernet3/0
ip route 192.168.5.0 255.255.255.0 GigabitEthernet4/0
line console 0
 login local
 exit
line vty 0 15
 login local
 transport input ssh
 exit
crypto key generate rsa
1024
exit
write memory
```

---

### 2. Switch Configurations

#### Switch0 (192.168.1.0/24)
Switch0 connects to Server0, Server1, and Server2.
```bash
enable
configure terminal
hostname Switch0
ip domain-name lab.local
username cisco secret cisco
enable secret cisco

interface vlan 1
 ip address 192.168.1.2 255.255.255.0
 no shutdown
 exit
ip default-gateway 192.168.1.1

interface GigabitEthernet0/1
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit
interface FastEthernet0/3
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit

line console 0
 login local
 exit
line vty 0 15
 login local
 transport input ssh
 exit
crypto key generate rsa
1024

write memory
```

#### Switch1 (192.168.2.0/24)
Switch1 connects to PC0, PC1, and PC2.
```bash
enable
configure terminal
hostname Switch1
ip domain-name lab.local
username cisco secret cisco
enable secret cisco

interface vlan 1
 ip address 192.168.2.2 255.255.255.0
 no shutdown
 exit
ip default-gateway 192.168.2.1

interface GigabitEthernet0/1
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit
interface FastEthernet0/3
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit

line console 0
 login local
 exit
line vty 0 15
 login local
 transport input ssh
 exit
crypto key generate rsa
1024

write memory
```

#### Switch2 (192.168.3.0/24)
Switch2 connects to PC3, PC4, and PC5.
```bash
enable
configure terminal
hostname Switch2
ip domain-name lab.local
username cisco secret cisco
enable secret cisco

interface vlan 1
 ip address 192.168.3.2 255.255.255.0
 no shutdown
 exit
ip default-gateway 192.168.3.1

interface GigabitEthernet0/1
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit
interface FastEthernet0/3
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit

line console 0
 login local
 exit
line vty 0 15
 login local
 transport input ssh
 exit
crypto key generate rsa
1024

write memory
```

#### Switch3 (192.168.4.0/24)
Switch3 connects to PC6, PC7, and PC8.
```bash
enable
configure terminal
hostname Switch3
ip domain-name lab.local
username cisco secret cisco
enable secret cisco

interface vlan 1
 ip address 192.168.4.2 255.255.255.0
 no shutdown
 exit
ip default-gateway 192.168.4.1

interface GigabitEthernet0/1
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit
interface FastEthernet0/3
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit

line console 0
 login local
 exit
line vty 0 15
 login local
 transport input ssh
 exit
crypto key generate rsa
1024

write memory
```

#### Switch4 (192.168.5.0/24)
Switch4 connects to PC9, PC10, and PC11.
```bash
enable
configure terminal
hostname Switch4
ip domain-name lab.local
username cisco secret cisco
enable secret cisco

interface vlan 1
 ip address 192.168.5.2 255.255.255.0
 no shutdown
 exit
ip default-gateway 192.168.5.1

interface GigabitEthernet0/1
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit
interface FastEthernet0/3
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit

line console 0
 login local
 exit
line vty 0 15
 login local
 transport input ssh
 exit
crypto key generate rsa
1024

write memory
```

---

### 3. End Device Configurations (Servers and PCs)
End devices (servers and PCs) are configured with static IP addresses via the Cisco Packet Tracer GUI.

#### Steps to Configure IP Addresses
1. **Click on the device** (e.g., Server0, PC0).
2. Go to the **Desktop** tab.
3. Select **IP Configuration**.
4. Set the **IPv4 Address**, **Subnet Mask**, and **Default Gateway** as per the IP Addressing Table.
5. Close the window to apply the settings.

#### Example Configurations
- **Server0**:
  - IPv4 Address: 192.168.1.3
  - Subnet Mask: 255.255.255.0
  - Default Gateway: 192.168.1.1
- **PC0**:
  - IPv4 Address: 192.168.2.3
  - Subnet Mask: 255.255.255.0
  - Default Gateway: 192.168.2.1
- **PC11**:
  - IPv4 Address: 192.168.5.5
  - Subnet Mask: 255.255.255.0
  - Default Gateway: 192.168.5.1

Repeat this process for all servers (Server0–Server2) and PCs (PC0–PC11) using the IP Addressing Table.

---

## Verification and Testing

### 1. Verify Configurations
Use the following commands to verify the configurations on each device.

#### Router0
```bash
show running-config
show ip interface brief
show ip route
```

#### Switches (Switch0–Switch4)
```bash
show running-config
show vlan brief
show ip interface brief
```

#### PCs and Servers
- Open the **Command Prompt** in the Desktop tab:
  ```cmd
  ipconfig
  ping <default-gateway>
  ping <another-device-ip>
  ```
  Example for PC0:
  ```cmd
  ipconfig
  ping 192.168.2.1  # Ping Router0 G1/0
  ping 192.168.2.2  # Ping Switch1
  ping 192.168.3.3  # Ping PC3 (inter-subnet)
  ```

### 2. Test SSH Remote Access
From a PC (e.g., PC0), test SSH access to a switch or router:
1. Go to the **Desktop** tab of PC0.
2. Open the **Command Prompt**.
3. Use the SSH command:
   ```cmd
   ssh -l cisco 192.168.2.2  # SSH to Switch1
   ```
4. Enter the password: `cisco`.
5. Verify you can access the switch’s CLI remotely.

Repeat for Router0 (e.g., `ssh -l cisco 192.168.2.1`).

---

## Troubleshooting Tips
- **IP Conflicts**: Ensure no duplicate IPs within a subnet (e.g., 192.168.1.3 is unique in 192.168.1.0/24).
- **Interface Status**: Use `show ip interface brief` to confirm interfaces are “up/up”. If not, issue `no shutdown`.
- **Connectivity Issues**:
  - Verify IP addresses, subnet masks, and default gateways.
  - Check VLAN assignments (`show vlan brief` on switches).
  - Ensure routing is configured on Router0 (`show ip route`).
- **SSH Issues**:
  - Confirm `crypto key generate rsa` was run after setting hostname and domain name.
  - Verify `transport input ssh` is set on VTY lines.
  - Check username/password (`cisco/cisco`).
