# Activity 3: Subnetting

## Overview
This activity focuses on subnetting and IP address allocation for a network in Cisco Packet Tracer, targeting Class A, B, and C IP ranges. The objectives include:
- Subnetting Class A (10.0.0.0), Class B (172.16.0.0), and Class C (192.168.100.0) networks into smaller subnets.
- Assigning usable host IPs to PCs, excluding network and broadcast addresses.
- Distributing IPs across paired PCs within each subnet.
- Ensuring proper connectivity through a single switch for each class.

The network consists of:  
- **1 Switch**: Switch0 (Cisco 2960) per class  
- **18 PCs**: PC0–PC17 (2 paired per subnet, 8 pairs per class, with some unassigned in Class C due to subnet size constraints)  

The topology is centralized by a single Cisco 2960 switch per class (Class A, B, C), with each switch managing multiple subnets. Each subnet pair contains 2 PCs, and IPs are assigned to ensure no overlap, excluding network and broadcast addresses.

---

## IP Addressing Table
Based on the provided topology, we’ll assign IP addresses to each PC in the network across Class A, B, and C. Each class uses a single switch (Switch0) with 8 subnets, one for each pair of PCs. The subnets are defined by their respective subnet masks, and IPs are assigned to PCs starting from the first usable IP.

### Class A: 10.0.0.0
| Device | Interface/Port | IP Address     | Subnet Mask     | Default Gateway | Subnet (CIDR) | Connected To   |
|--------|----------------|----------------|-----------------|-----------------|---------------|----------------|
| **Switch0** | VLAN 1     | 10.0.0.1       | 255.128.0.0     | -               | 10.0.0.0/9    | -              |
| **PC0**     | F0         | 10.0.0.2       | 255.128.0.0     | 10.0.0.1        | 10.0.0.0/9    | Switch0 (F0/1) |
| **PC1**     | F0         | 10.0.0.3       | 255.128.0.0     | 10.0.0.1        | 10.0.0.0/9    | Switch0 (F0/2) |
| **PC2**     | F0         | 10.128.0.2     | 255.192.0.0     | 10.128.0.1      | 10.128.0.0/10 | Switch0 (F0/3) |
| **PC3**     | F0         | 10.128.0.3     | 255.192.0.0     | 10.128.0.1      | 10.128.0.0/10 | Switch0 (F0/4) |
| **PC4**     | F0         | 10.192.0.2     | 255.224.0.0     | 10.192.0.1      | 10.192.0.0/11 | Switch0 (F0/5) |
| **PC5**     | F0         | 10.192.0.3     | 255.224.0.0     | 10.192.0.1      | 10.192.0.0/11 | Switch0 (F0/6) |
| **PC6**     | F0         | 10.224.0.2     | 255.240.0.0     | 10.224.0.1      | 10.224.0.0/12 | Switch0 (F0/7) |
| **PC7**     | F0         | 10.224.0.3     | 255.240.0.0     | 10.224.0.1      | 10.224.0.0/12 | Switch0 (F0/8) |
| **PC8**     | F0         | 10.240.0.2     | 255.248.0.0     | 10.240.0.1      | 10.240.0.0/13 | Switch0 (F0/9) |
| **PC9**     | F0         | 10.240.0.3     | 255.248.0.0     | 10.240.0.1      | 10.240.0.0/13 | Switch0 (F0/10)|
| **PC10**    | F0         | 10.248.0.2     | 255.252.0.0     | 10.248.0.1      | 10.248.0.0/14 | Switch0 (F0/11)|
| **PC11**    | F0         | 10.248.0.3     | 255.252.0.0     | 10.248.0.1      | 10.248.0.0/14 | Switch0 (F0/12)|
| **PC12**    | F0         | 10.252.0.2     | 255.254.0.0     | 10.252.0.1      | 10.252.0.0/15 | Switch0 (F0/13)|
| **PC13**    | F0         | 10.252.0.3     | 255.254.0.0     | 10.252.0.1      | 10.252.0.0/15 | Switch0 (F0/14)|
| **PC14**    | F0         | 10.254.0.2     | 255.255.0.0     | 10.254.0.1      | 10.254.0.0/16 | Switch0 (F0/15)|
| **PC15**    | F0         | 10.254.0.3     | 255.255.0.0     | 10.254.0.1      | 10.254.0.0/16 | Switch0 (F0/16)|

### Class B: 172.16.0.0
| Device | Interface/Port | IP Address     | Subnet Mask     | Default Gateway | Subnet (CIDR) | Connected To   |
|--------|----------------|----------------|-----------------|-----------------|---------------|----------------|
| **Switch0** | VLAN 1     | 172.16.0.1     | 255.255.128.0   | -               | 172.16.0.0/17 | -              |
| **PC0**     | F0         | 172.16.0.2     | 255.255.128.0   | 172.16.0.1      | 172.16.0.0/17 | Switch0 (F0/1) |
| **PC1**     | F0         | 172.16.0.3     | 255.255.128.0   | 172.16.0.1      | 172.16.0.0/17 | Switch0 (F0/2) |
| **PC2**     | F0         | 172.16.128.2   | 255.255.192.0   | 172.16.128.1    | 172.16.128.0/18 | Switch0 (F0/3) |
| **PC3**     | F0         | 172.16.128.3   | 255.255.192.0   | 172.16.128.1    | 172.16.128.0/18 | Switch0 (F0/4) |
| **PC4**     | F0         | 172.16.192.2   | 255.255.224.0   | 172.16.192.1    | 172.16.192.0/19 | Switch0 (F0/5) |
| **PC5**     | F0         | 172.16.192.3   | 255.255.224.0   | 172.16.192.1    | 172.16.192.0/19 | Switch0 (F0/6) |
| **PC6**     | F0         | 172.16.224.2   | 255.255.240.0   | 172.16.224.1    | 172.16.224.0/20 | Switch0 (F0/7) |
| **PC7**     | F0         | 172.16.224.3   | 255.255.240.0   | 172.16.224.1    | 172.16.224.0/20 | Switch0 (F0/8) |
| **PC8**     | F0         | 172.16.240.2   | 255.255.248.0   | 172.16.240.1    | 172.16.240.0/21 | Switch0 (F0/9) |
| **PC9**     | F0         | 172.16.240.3   | 255.255.248.0   | 172.16.240.1    | 172.16.240.0/21 | Switch0 (F0/10)|
| **PC10**    | F0         | 172.16.248.2   | 255.255.252.0   | 172.16.248.1    | 172.16.248.0/22 | Switch0 (F0/11)|
| **PC11**    | F0         | 172.16.248.3   | 255.255.252.0   | 172.16.248.1    | 172.16.248.0/22 | Switch0 (F0/12)|
| **PC12**    | F0         | 172.16.252.2   | 255.255.254.0   | 172.16.252.1    | 172.16.252.0/23 | Switch0 (F0/13)|
| **PC13**    | F0         | 172.16.252.3   | 255.255.254.0   | 172.16.252.1    | 172.16.252.0/23 | Switch0 (F0/14)|
| **PC14**    | F0         | 172.16.254.2   | 255.255.255.0   | 172.16.254.1    | 172.16.254.0/24 | Switch0 (F0/15)|
| **PC15**    | F0         | 172.16.254.3   | 255.255.255.0   | 172.16.254.1    | 172.16.254.0/24 | Switch0 (F0/16)|

### Class C: 192.168.100.0
| Device | Interface/Port | IP Address       | Subnet Mask     | Default Gateway   | Subnet (CIDR)   | Connected To   |
|--------|----------------|------------------|-----------------|-------------------|-----------------|----------------|
| **Switch0** | VLAN 1     | 192.168.100.1    | 255.255.255.128 | -                 | 192.168.100.0/25| -              |
| **PC0**     | F0         | 192.168.100.2    | 255.255.255.128 | 192.168.100.1     | 192.168.100.0/25| Switch0 (F0/1) |
| **PC1**     | F0         | 192.168.100.3    | 255.255.255.128 | 192.168.100.1     | 192.168.100.0/25| Switch0 (F0/2) |
| **PC2**     | F0         | 192.168.100.130  | 255.255.255.192 | 192.168.100.129   | 192.168.100.128/26 | Switch0 (F0/3) |
| **PC3**     | F0         | 192.168.100.131  | 255.255.255.192 | 192.168.100.129   | 192.168.100.128/26 | Switch0 (F0/4) |
| **PC4**     | F0         | 192.168.100.194  | 255.255.255.224 | 192.168.100.193   | 192.168.100.192/27 | Switch0 (F0/5) |
| **PC5**     | F0         | 192.168.100.195  | 255.255.255.224 | 192.168.100.193   | 192.168.100.192/27 | Switch0 (F0/6) |
| **PC6**     | F0         | 192.168.100.226  | 255.255.255.240 | 192.168.100.225   | 192.168.100.224/28 | Switch0 (F0/7) |
| **PC7**     | F0         | 192.168.100.227  | 255.255.255.240 | 192.168.100.225   | 192.168.100.224/28 | Switch0 (F0/8) |
| **PC8**     | F0         | 192.168.100.242  | 255.255.255.248 | 192.168.100.241   | 192.168.100.240/29 | Switch0 (F0/9) |
| **PC9**     | F0         | 192.168.100.243  | 255.255.255.248 | 192.168.100.241   | 192.168.100.240/29 | Switch0 (F0/10)|
| **PC10**    | F0         | 192.168.100.249  | 255.255.255.252 | 192.168.100.249   | 192.168.100.248/30 | Switch0 (F0/11)|
| **PC11**    | F0         | 192.168.100.250  | 255.255.255.252 | 192.168.100.249   | 192.168.100.248/30 | Switch0 (F0/12)|
| **PC12**    | F0         | 192.168.100.252  | 255.255.255.254 | 192.168.100.252   | 192.168.100.252/31 | Switch0 (F0/13)|
| **PC13**    | F0         | 192.168.100.253  | 255.255.255.254 | 192.168.100.252   | 192.168.100.252/31 | Switch0 (F0/14)|
| **PC14**    | F0         | 192.168.100.254  | 255.255.255.255 | 192.168.100.254   | 192.168.100.254/32 | Switch0 (F0/15)|

### Notes on IP Addressing
- **Subnets**: Each class is divided into 8 subnets, one for each pair of PCs:
  - **Class A**: /9 to /16 (e.g., 10.0.0.0/9, 10.128.0.0/10, etc.)
  - **Class B**: /17 to /24 (e.g., 172.16.0.0/17, 172.16.128.0/18, etc.)
  - **Class C**: /25 to /32 (e.g., 192.168.100.0/25, 192.168.100.128/26, etc.)
- **Default Gateway**: The default gateway for each PC matches the Switch0 VLAN 1 IP for its subnet (e.g., 10.0.0.1 for Class A Pair 1).
- **IP Range**: Each subnet provides a range of usable addresses, excluding network and broadcast addresses (e.g., 192.168.100.1–192.168.100.126 for Class C Pair 1).
- **Class C Limitation**: Subnets /30, /31, and /32 in Class C have limited usable IPs (2, 2, and 1 respectively), so PC15 in Pair 8 is unassigned.

---

## Network Topology Overview
The network is centralized by a single Cisco 2960 switch (Switch0) per class (Class A, B, C). Each switch manages 8 subnets, with each subnet containing a pair of PCs.

### Class A: 10.0.0.0
- **Switch0**:
  - Connects to PC0–PC15 (8 pairs, 16 PCs total).
  - Subnets: 10.0.0.0/9, 10.128.0.0/10, ..., 10.254.0.0/16.
  - PC16 and PC17 are not used (only 16 PCs assigned).

### Class B: 172.16.0.0
- **Switch0**:
  - Connects to PC0–PC15 (8 pairs, 16 PCs total).
  - Subnets: 172.16.0.0/17, 172.16.128.0/18, ..., 172.16.254.0/24.
  - PC16 and PC17 are not used (only 16 PCs assigned).

### Class C: 192.168.100.0
- **Switch0**:
  - Connects to PC0–PC14 (8 pairs, but Pair 8 has only 1 PC due to /32).
  - Subnets: 192.168.100.0/25, 192.168.100.128/26, ..., 192.168.100.254/32.
  - PC15 is unassigned in Pair 8 (/32 has only 1 usable IP); PC16 and PC17 are not used.

---

## Additional Notes
- **Network Topology**: All PCs in each class (A, B, C) are connected to a single Cisco 2960 switch. Each pair of PCs operates within its own subnet, ensuring no IP overlap.
- **IP Allocation**: IPs are assigned sequentially starting from the first usable IP (e.g., 10.0.0.2 for PC0 in Class A Pair 1). Network and broadcast addresses are excluded.
- **Pitfalls**: Avoid assigning network or broadcast addresses to hosts (e.g., 192.168.100.127 in Class C Pair 1 is a broadcast address and cannot be used).
- **Hardware Consideration**: The Cisco 2960 switch supports VLANs, which could be used to logically separate these subnets if needed, even though they’re on the same physical switch. In this setup, VLAN 1 is used for simplicity, but separate VLANs per subnet could enhance security and traffic management.
- **Class C Subnet Constraints**: Subnets /30, /31, and /32 are typically used for point-to-point links rather than PC pairs due to their small size (2, 2, and 1 usable IPs). In a real-world scenario, larger subnets might be preferred for PC pairs.

---
