# Subnetting

## Given Information
- **IP Address**:  
    This typically refers to an IP address within the range of usable host addresses in a subnet. It represents a specific device’s address or a reference point for subnet calculations.
- **Subnet Mask**:  
    A binary mask used to delineate the network and host portions of an IP address. It reveals the Network ID (by preserving network bits) and conceals the Host ID (by masking host bits), enabling the division of a network into subnets.

## Objectives
Subnetting problems often require calculating the following components based on the given IP address and subnet mask:
- **Total Possible Addresses (2^n)**:  
    This represents the total number of IP addresses in a subnet, including the network address and broadcast address. Here, n is the number of bits allocated to the host portion of the subnet mask.
- **Usable Addresses (2^n - 2)**:  
    The number of IP addresses that can be assigned to end devices (e.g., computers, printers). It excludes the network and broadcast addresses, with n being the number of host bits.
- **Total Number of Subnets (2^m)**:  
    The total count of subnets created from the original network, where m is the number of bits borrowed from the host portion to define subnet divisions.
- **Network IP Address**:  
    The first address in a subnet, reserved to identify the subnet itself. It cannot be assigned to any end device.
- **Broadcast IP Address**:  
    The last address in a subnet, used for broadcasting to all devices within that subnet. It is not assignable to individual end devices.  
    _(Correction: Your original note incorrectly states it "can be assigned on the end devices," which contradicts standard networking rules.)_
- **Start of Usable IP Address**:  
    The first address in the subnet that can be assigned to an end device, immediately following the network address.
- **Last of Usable IP Address**:  
    The final address in the subnet available for assignment to an end device, immediately preceding the broadcast address.
- **Range of Usable IP Addresses**:  
    The span of assignable addresses within the subnet, encompassing all IPs from the "Start of Usable IP Address" to the "Last of Usable IP Address."
- **Subnet Location of the IP Address**:  
    The specific subnet to which the given IP address belongs, often expressed as an ordinal position (e.g., the 5th subnet) within the set of all subnets derived from the original network.

## Methods and Computation

### Long Method - Using Binary Notation
#### Finding the First IP Address (Network Address)
**Requirements:**
- A given IP address (typically a host address within the subnet)
- The subnet mask associated with the network

**Steps:**
1. **Convert to Binary Notation**:  
    Transform both the given IP address and the subnet mask into their binary equivalents, representing each octet as an 8-bit sequence.
2. **Perform a Bitwise AND Operation**:  
    Align the binary forms of the IP address and subnet mask, then apply the logical AND operation (1 AND 1 = 1; all other combinations = 0) to isolate the network portion. Ensure proper alignment of bits to avoid computational errors.
    - **Binary Representation**:  
        IP Address AND Subnet Mask = Network Address
3. **Convert Back to Dotted Decimal Notation**:  
    Translate the resulting binary network address into its decimal form, separating each 8-bit octet with dots.

---

#### Finding the Last IP Address (Broadcast Address)
**Requirements:**
- The first IP address (network address) in binary notation
- The subnet mask in binary notation

**Steps:**
1. **Obtain the Network Address in Binary**:  
    Use the binary form of the first IP address (network address). If only the decimal form is available, convert it to binary notation first.
2. **Invert the Subnet Mask**:  
    Flip each bit of the subnet mask (1 becomes 0, 0 becomes 1) to create the inverted mask, also known as the wildcard mask. This highlights the host portion.
3. **Perform a Bitwise OR Operation**:  
    Align the binary network address with the inverted subnet mask, then apply the logical OR operation (0 OR 0 = 0; all other combinations = 1) to compute the broadcast address. Ensure precise bit alignment for accurate results.
    - **Binary Representation**:  
        Network Address OR Inverted Subnet Mask = Broadcast Address
4. **Obtain the Broadcast Address in Binary**:  
    The result of the OR operation yields the broadcast address in binary form.
5. **Convert Back to Dotted Decimal Notation**:  
    Translate the binary broadcast address into its decimal equivalent, formatted as four octets separated by dots.


#### Example 1:
**Given:**
- **IP Address**: 172.50.100.121  
- **Subnet Mask**: 255.255.248.0  

---

##### Finding the First IP Address (Network Address)
**Requirements:**  
- Given IP Address: 172.50.100.121  
- Subnet Mask: 255.255.248.0  

**Steps:**

1. **Convert to Binary Notation**:  
   Transform both the IP address and subnet mask into their 32-bit binary representations, with each octet as an 8-bit sequence.

   - **IP Address: 172.50.100.121**  
     - **172**:  
       128 + 32 + 8 + 4 = 172  
       Binary: `10101100`  
       (128=1, 64=0, 32=1, 16=0, 8=1, 4=1, 2=0, 1=0)  
     - **50**:  
       32 + 16 + 2 = 50  
       Binary: `00110010`  
       (32=1, 16=1, 8=0, 4=0, 2=1, 1=0, padded with zeros)  
     - **100**:  
       64 + 32 + 4 = 100  
       Binary: `01100100`  
       (64=1, 32=1, 16=0, 8=0, 4=1, 2=0, 1=0)  
     - **121**:  
       64 + 32 + 16 + 8 + 1 = 121  
       Binary: `01111001`  
       (64=1, 32=1, 16=1, 8=1, 4=0, 2=0, 1=1)  
     - **Full Binary**:  
       `10101100.00110010.01100100.01111001`  
     - **Correction from Original**: Your notes show `01101000` (104) for the third octet, but 100 = `01100100`. This was a typo.

   - **Subnet Mask: 255.255.248.0**  
     - **255**:  
       128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 = 255  
       Binary: `11111111`  
     - **255**:  
       Binary: `11111111`  
     - **248**:  
       128 + 64 + 32 + 16 + 8 = 248  
       Binary: `11111000`  
       (128=1, 64=1, 32=1, 16=1, 8=1, 4=0, 2=0, 1=0)  
     - **0**:  
       Binary: `00000000`  
     - **Full Binary**:  
       `11111111.11111111.11111000.00000000`  
     - **Verification**: Matches your notes (`1111 1111.1111 1111.1111 1000.0000 0000`), correct when spaces are ignored.

2. **Perform a Bitwise AND Operation**:  
   Align the binary IP address and subnet mask, then apply the AND logic (1 AND 1 = 1; all else = 0) to find the network address.

   ```
   IP Address:    10101100.00110010.01100100.01111001
   Subnet Mask:   11111111.11111111.11111000.00000000
   --------------------------------------------
   Result:        10101100.00110010.01100000.00000000
   ```

   - **First Octet**: `10101100 AND 11111111 = 10101100` (all bits preserved)  
   - **Second Octet**: `00110010 AND 11111111 = 00110010` (all bits preserved)  
   - **Third Octet**: `01100100 AND 11111000 = 01100000`  
     - First 5 bits: `01100 AND 11111 = 01100`  
     - Last 3 bits: `100 AND 000 = 000`  
   - **Fourth Octet**: `01111001 AND 00000000 = 00000000` (all zeroed)  
   - **Correction from Original**: Your notes’ IP third octet (`01101000`) is wrong, but the result (`01100000`) aligns with using 100 (`01100100`), suggesting the typo didn’t affect the AND step.

3. **Convert Back to Dotted Decimal Notation**:  
   Translate the binary result into decimal octets.

   - **Binary Result**: `10101100.00110010.01100000.00000000`  
     - **10101100**: 128 + 32 + 8 + 4 = 172  
     - **00110010**: 32 + 16 + 2 = 50  
     - **01100000**: 64 + 32 = 96  
     - **00000000**: 0  
   - **Network Address**: `172.50.96.0`  
   - **Correction from Original**: Your notes say `127.50.96.0` (`127 = 01111111`), a clear error since `10101100 = 172`. The conclusion (`172.50.96.0`) contradicts this, indicating a typo.

**Result**:  
**The First IP Address (Network Address) is 172.50.96.0.**

---

##### Finding the Last IP Address (Broadcast Address)
**Requirements:**  
- First IP Address (Network Address) in Binary: `10101100.00110010.01100000.00000000`  
- Subnet Mask in Binary: `11111111.11111111.11111000.00000000`

**Steps:**

1. **Obtain the Network Address in Binary**:  
   Already calculated: `10101100.00110010.01100000.00000000` (from above).

2. **Invert the Subnet Mask**:  
   Flip each bit of the subnet mask (1 → 0, 0 → 1) to get the wildcard mask.

   - **Subnet Mask**: `11111111.11111111.11111000.00000000`  
   - **Inverted**:  
     - **11111111 → 00000000**  
     - **11111111 → 00000000**  
     - **11111000 → 00000111** (128=0, 64=0, 32=0, 16=0, 8=0, 4=1, 2=1, 1=1)  
     - **00000000 → 11111111**  
   - **Wildcard Mask**: `00000000.00000000.00000111.11111111`

3. **Perform a Bitwise OR Operation**:  
   Align the network address with the inverted subnet mask, then apply the OR logic (0 OR 0 = 0; all else = 1) to find the broadcast address.

   ```
   Network Address:  10101100.00110010.01100000.00000000
   Inverted Mask:    00000000.00000000.00000111.11111111
   -------------------------------------------------
   Result:           10101100.00110010.011 vestigates11111.11111111
   ```

   - **First Octet**: `10101100 OR 00000000 = 10101100` (unchanged)  
   - **Second Octet**: `00110010 OR 00000000 = 00110010` (unchanged)  
   - **Third Octet**: `01100000 OR 00000111 = 01100111`  
     - First 5 bits: `01100 OR 00000 = 01100`  
     - Last 3 bits: `000 OR 111 = 111` (4 + 2 + 1 = 7)  
   - **Fourth Octet**: `00000000 OR 11111111 = 11111111` (255)  

   - **Binary Result**: `10101100.00110010.01100111.11111111`

4. **Obtain the Broadcast Address in Binary**:  
   The result is: `10101100.00110010.01100111.11111111`

5. **Convert Back to Dotted Decimal Notation**:  
   - **10101100**: 128 + 32 + 8 + 4 = 172  
   - **00110010**: 32 + 16 + 2 = 50  
   - **01100111**: 64 + 32 + 4 + 2 + 1 = 103  
   - **11111111**: 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 = 255  
   - **Broadcast Address**: `172.50.103.255`

**Result**:  
**The Last IP Address (Broadcast Address) is 172.50.103.255.**

Below is a new sample computation (Example 2) for a Class A IP address, using the "Long Method - Using Binary Notation" as outlined in your notes. Class A addresses have a default subnet mask of 255.0.0.0 (/8), where the first octet (1-126) identifies the network, and the remaining three octets are for hosts. For this example, I’ll subnet a Class A address further to demonstrate the method, ensuring the computation is detailed, accurate, and follows the same rigorous steps as Example 1.

---

#### Example 2: Class A Subnetting
**Given:**
- **IP Address**: 10.45.72.19  
  - Class A range: 1.0.0.0 to 126.255.255.255 (first octet 1-126), so 10.x.x.x is Class A.
- **Subnet Mask**: 255.240.0.0 (/12)  
  - This extends the default /8 mask by borrowing 4 bits from the host portion, creating subnets.

---

##### Finding the First IP Address (Network Address)
**Requirements:**
- Given IP Address: 10.45.72.19
- Subnet Mask: 255.240.0.0

**Steps:**

1. **Convert to Binary Notation**:  
   Convert the IP address and subnet mask into their 32-bit binary representations, with each octet as an 8-bit sequence.

   - **IP Address: 10.45.72.19**  
     - **10**:  
       8 + 2 = 10  
       Binary: `00001010`  
       (128=0, 64=0, 32=0, 16=0, 8=1, 4=0, 2=1, 1=0)  
     - **45**:  
       32 + 8 + 4 + 1 = 45  
       Binary: `00101101`  
       (32=1, 16=0, 8=1, 4=1, 2=0, 1=1)  
     - **72**:  
       64 + 8 = 72  
       Binary: `01001000`  
       (64=1, 32=0, 16=0, 8=1, 4=0, 2=0, 1=0)  
     - **19**:  
       16 + 2 + 1 = 19  
       Binary: `00010011`  
       (16=1, 8=0, 4=0, 2=1, 1=1)  
     - **Full Binary**:  
       `00001010.00101101.01001000.00010011`

   - **Subnet Mask: 255.240.0.0**  
     - **255**:  
       128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 = 255  
       Binary: `11111111`  
     - **240**:  
       128 + 64 + 32 + 16 = 240  
       Binary: `11110000`  
       (128=1, 64=1, 32=1, 16=1, 8=0, 4=0, 2=0, 1=0)  
     - **0**:  
       Binary: `00000000`  
     - **0**:  
       Binary: `00000000`  
     - **Full Binary**:  
       `11111111.11110000.00000000.00000000`  
       - /12 means 12 network bits (8 from first octet + 4 from second).

2. **Perform a Bitwise AND Operation**:  
   Align the binary IP address and subnet mask, then apply the AND logic (1 AND 1 = 1; all else = 0) to find the network address.

   ```
   IP Address:    00001010.00101101.01001000.00010011
   Subnet Mask:   11111111.11110000.00000000.00000000
   --------------------------------------------
   Result:        00001010.00100000.00000000.00000000
   ```

   - **First Octet**: `00001010 AND 11111111 = 00001010` (all bits preserved)  
   - **Second Octet**: `00101101 AND 11110000 = 00100000`  
     - First 4 bits: `0010 AND 1111 = 0010`  
     - Last 4 bits: `1101 AND 0000 = 0000`  
   - **Third Octet**: `01001000 AND 00000000 = 00000000` (all zeroed)  
   - **Fourth Octet**: `00010011 AND 00000000 = 00000000` (all zeroed)

3. **Convert Back to Dotted Decimal Notation**:  
   Translate the binary result into decimal octets.

   - **Binary Result**: `00001010.00100000.00000000.00000000`  
     - **00001010**: 8 + 2 = 10  
     - **00100000**: 32 = 32  
     - **00000000**: 0  
     - **00000000**: 0  
   - **Network Address**: `10.32.0.0`

**Result**:  
**The First IP Address (Network Address) is 10.32.0.0.**

---

##### Finding the Last IP Address (Broadcast Address)
**Requirements:**
- First IP Address (Network Address) in Binary: `00001010.00100000.00000000.00000000`
- Subnet Mask in Binary: `11111111.11110000.00000000.00000000`

**Steps:**

1. **Obtain the Network Address in Binary**:  
   Already calculated: `00001010.00100000.00000000.00000000` (from above).

2. **Invert the Subnet Mask**:  
   Flip each bit of the subnet mask (1 → 0, 0 → 1) to create the wildcard mask.

   - **Subnet Mask**: `11111111.11110000.00000000.00000000`  
   - **Inverted**:  
     - **11111111 → 00000000**  
     - **11110000 → 00001111** (8=1, 4=1, 2=1, 1=1)  
     - **00000000 → 11111111**  
     - **00000000 → 11111111**  
   - **Wildcard Mask**: `00000000.00001111.11111111.11111111`

3. **Perform a Bitwise OR Operation**:  
   Align the network address with the inverted subnet mask, then apply the OR logic (0 OR 0 = 0; all else = 1) to find the broadcast address.

   ```
   Network Address:  00001010.00100000.00000000.00000000
   Inverted Mask:    00000000.00001111.11111111.11111111
   -------------------------------------------------
   Result:           00001010.00101111.11111111.11111111
   ```

   - **First Octet**: `00001010 OR 00000000 = 00001010` (unchanged)  
   - **Second Octet**: `00100000 OR 00001111 = 00101111`  
     - First 4 bits: `0010 OR 0000 = 0010`  
     - Last 4 bits: `0000 OR 1111 = 1111` (8 + 4 + 2 + 1 = 15)  
   - **Third Octet**: `00000000 OR 11111111 = 11111111` (255)  
   - **Fourth Octet**: `00000000 OR 11111111 = 11111111` (255)

4. **Obtain the Broadcast Address in Binary**:  
   The result is: `00001010.00101111.11111111.11111111`

5. **Convert Back to Dotted Decimal Notation**:  
   - **00001010**: 8 + 2 = 10  
   - **00101111**: 32 + 8 + 4 + 2 + 1 = 47  
   - **11111111**: 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 = 255  
   - **11111111**: 255  
   - **Broadcast Address**: `10.47.255.255`

**Result**:  
**The Last IP Address (Broadcast Address) is 10.47.255.255.**

---

### Short Method - Using Decimal Notation

#### Finding the First IP Address (Network Address)

**Requirements:**
- A given IP address (typically a host address within the subnet)
- The subnet mask associated with the network

**Rules:**
1. **Mask = 255**: Copy the IP address octet unchanged (all bits are network bits).
2. **Mask = 0**: Set the IP address octet to 0 (all bits are host bits).
3. **Mask ≠ 255 or 0**: Convert the IP address and subnet mask for that specific octet to binary notation, perform a bitwise AND operation, then convert the result back to decimal.

**Steps:**
1. **Analyze Each Octet**: Apply the appropriate rule (1, 2, or 3) to each octet of the IP address based on the corresponding subnet mask octet.
2. **For Rule 3 Octets**:
    - Convert the IP address octet and subnet mask octet to 8-bit binary.
    - Perform the AND operation (1 AND 1 = 1; all else = 0).
    - Convert the binary result back to decimal.
3. **Assemble the Network Address**: Combine the resulting octets into dotted decimal notation.

---

#### Finding the Last IP Address (Broadcast Address)
**Requirements:**
- The network address (First IP) in decimal notation (from above)
- The subnet mask associated with the network

**Rules:**
1. **Mask = 255**: Copy the network address octet unchanged (no host bits to modify).
2. **Mask = 0**: Set the network address octet to 255 (all host bits become 1s).
3. **Mask ≠ 255 or 0**:
    - Convert the network address octet and subnet mask octet to binary.
    - Invert the subnet mask octet (1 → 0, 0 → 1).
    - Perform a bitwise OR operation with the network address octet (0 OR 0 = 0; all else = 1).
    - Convert the binary result back to decimal.

**Steps:**
1. **Analyze Each Octet**: Apply the appropriate rule (1, 2, or 3) to each octet of the network address based on the subnet mask.
2. **For Rule 3 Octets**:
    - Convert the network address octet and subnet mask octet to 8-bit binary.
    - Invert the subnet mask (e.g., 240 = 11110000 → 00001111).
    - Perform the OR operation.
    - Convert the binary result back to decimal.
3. **Assemble the Broadcast Address**: Combine the resulting octets into dotted decimal notation.

---
#### Example 1: Class A Subnetting

**Given:**

- **IP Address**: 10.45.72.19
    - Class A (first octet 1-126, default mask 255.0.0.0), subnetted further.
- **Subnet Mask**: 255.240.0.0 (/12)
    - Borrows 4 bits from the host portion, creating 16 subnets (2^4).

##### Finding the First IP Address (Network Address)

**Requirements:**

- IP Address: 10.45.72.19
- Subnet Mask: 255.240.0.0

**Steps:**

1. **Analyze Each Octet**:
    - **Octet 1**: IP = 10, Mask = 255
        - Rule 1: Copy 10 → 10
    - **Octet 2**: IP = 45, Mask = 240
        - Rule 3: Convert to binary and AND
            - IP: 45 = 00101101 (32 + 8 + 4 + 1)
            - Mask: 240 = 11110000 (128 + 64 + 32 + 16)
            - AND: 00101101 AND 11110000 = 00100000
                - First 4 bits: 0010 AND 1111 = 0010
                - Last 4 bits: 1101 AND 0000 = 0000
            - Result: 32
    - **Octet 3**: IP = 72, Mask = 0
        - Rule 2: Set to 0 → 0
    - **Octet 4**: IP = 19, Mask = 0
        - Rule 2: Set to 0 → 0
2. **Assemble the Network Address**:
    - 10.32.0.0

**Result**:  
**The First IP Address (Network Address) is 10.32.0.0.**

##### Finding the Last IP Address (Broadcast Address)

**Requirements:**

- Network Address: 10.32.0.0 (from above)
- Subnet Mask: 255.240.0.0

**Steps:**

1. **Analyze Each Octet**:
    - **Octet 1**: Network = 10, Mask = 255
        - Rule 1: Copy 10 → 10
    - **Octet 2**: Network = 32, Mask = 240
        - Rule 3: Convert to binary, invert mask, and OR
            - Network: 32 = 00100000 (32)
            - Mask: 240 = 11110000
            - Inverted Mask: 00001111 (15 = 8 + 4 + 2 + 1)
            - OR: 00100000 OR 00001111 = 00101111
                - First 4 bits: 0010 OR 0000 = 0010
                - Last 4 bits: 0000 OR 1111 = 1111
            - Result: 47 (32 + 15 = 47)
    - **Octet 3**: Network = 0, Mask = 0
        - Rule 2: Set to 255 → 255
    - **Octet 4**: Network = 0, Mask = 0
        - Rule 2: Set to 255 → 255
2. **Assemble the Broadcast Address**:
    - 10.47.255.255

**Result**:  
**The Last IP Address (Broadcast Address) is 10.47.255.255.**

---

#### Example 2: Class B Subnetting

**Given:**

- **IP Address**: 172.50.100.121
    - Class B (first octet 128-191, default mask 255.255.0.0), subnetted further.
- **Subnet Mask**: 255.255.248.0 (/21)
    - Borrows 5 bits from the host portion, creating 32 subnets (2^5).

##### Finding the First IP Address (Network Address)

**Requirements:**

- IP Address: 172.50.100.121
- Subnet Mask: 255.255.248.0

**Steps:**

1. **Analyze Each Octet**:
    - **Octet 1**: IP = 172, Mask = 255
        - Rule 1: Copy 172 → 172
    - **Octet 2**: IP = 50, Mask = 255
        - Rule 1: Copy 50 → 50
    - **Octet 3**: IP = 100, Mask = 248
        - Rule 3: Convert to binary and AND
            - IP: 100 = 01100100 (64 + 32 + 4)
            - Mask: 248 = 11111000 (128 + 64 + 32 + 16 + 8)
            - AND: 01100100 AND 11111000 = 01100000
                - First 5 bits: 01100 AND 11111 = 01100
                - Last 3 bits: 100 AND 000 = 000
            - Result: 96 (64 + 32)
    - **Octet 4**: IP = 121, Mask = 0
        - Rule 2: Set to 0 → 0
2. **Assemble the Network Address**:
    - 172.50.96.0

**Result**:  
**The First IP Address (Network Address) is 172.50.96.0.**

##### Finding the Last IP Address (Broadcast Address)

**Requirements:**

- Network Address: 172.50.96.0 (from above)
- Subnet Mask: 255.255.248.0

**Steps:**

1. **Analyze Each Octet**:
    - **Octet 1**: Network = 172, Mask = 255
        - Rule 1: Copy 172 → 172
    - **Octet 2**: Network = 50, Mask = 255
        - Rule 1: Copy 50 → 50
    - **Octet 3**: Network = 96, Mask = 248
        - Rule 3: Convert to binary, invert mask, and OR
            - Network: 96 = 01100000 (64 + 32)
            - Mask: 248 = 11111000
            - Inverted Mask: 00000111 (7 = 4 + 2 + 1)
            - OR: 01100000 OR 00000111 = 01100111
                - First 5 bits: 01100 OR 00000 = 01100
                - Last 3 bits: 000 OR 111 = 111
            - Result: 103 (64 + 32 + 4 + 2 + 1)
    - **Octet 4**: Network = 0, Mask = 0
        - Rule 2: Set to 255 → 255
2. **Assemble the Broadcast Address**:
    - 172.50.103.255

**Result**:  
**The Last IP Address (Broadcast Address) is 172.50.103.255.**

### Finding the Total Number of Subnets
The "Total Number of Subnets" is the count of smaller networks created by borrowing bits from the host portion of a default class mask. It depends on how many bits you borrow from the default mask to make the subnet mask.

- **New Example**:  
  - **IP Address**: `172.60.80.15`  
  - **Subnet Mask**: `255.255.224.0` (/19)  
  - **Class**: B (default mask: `255.255.0.0` or /16)

---

#### Shortcut Method for Total Number of Subnets

The shortcut is all about counting borrowed bits—it’s a single calculation once you know the default and subnet masks.

##### Step 1: Identify the Default CIDR
- Determine the IP class and its default mask:
  - Class B (128-191 in first octet): `255.255.0.0` → /16
- For `172.60.80.15`: Default CIDR = /16

##### Step 2: Find the Subnet CIDR
- Look at the subnet mask and count the 1s in its binary form (or use the /n).
- For `255.255.224.0`:
  - Binary: `11111111.11111111.11100000.00000000`
  - Count of 1s: 8 + 8 + 3 = 19 → /19

##### Step 3: Calculate Borrowed Bits
- Formula: **Borrowed Bits (m) = Subnet CIDR - Default CIDR**
- For /19 and /16:
  - m = 19 - 16 = 3

##### Step 4: Calculate Total Subnets
- Formula: **Total Subnets = 2^m**
- For m = 3:
  - Total Subnets = 2³ = 8

##### Result
- Total Number of Subnets: **8**

---

#### Why This Works
- **Borrowed Bits**: The default Class B mask (/16) uses 16 bits for the network. The /19 mask adds 3 more bits (borrowed from the host portion), splitting the original `172.60.0.0/16` network into 8 smaller subnets.
- **Subnet Size**: Each subnet has 2^(32 - 19) = 2¹³ = 8192 addresses.
- **Ranges**: 
  - Subnet 1: `172.60.0.0 - 172.60.31.255`
  - Subnet 2: `172.60.32.0 - 172.60.63.255`
  - …
  - Subnet 8: `172.60.224.0 - 172.60.255.255`
- **Count**: 3 borrowed bits double the number of subnets each time (2³ = 8).

---

#### General Shortcut Formula
For any network and subnet mask:
1. **Find Default CIDR**: Based on IP class (A: /8, B: /16, C: /24).
2. **Find Subnet CIDR**: Count 1s in the subnet mask or use /n.
3. **Borrowed Bits (m)**: Subnet CIDR - Default CIDR.
4. **Total Subnets**: 2^m.

---

#### Another Class B Example
- **IP**: `172.90.10.100`
- **Mask**: `255.255.240.0` (/20)
1. Default CIDR: /16 (Class B)
2. Subnet CIDR: /20 (8 + 8 + 4 = 20)
3. Borrowed Bits: 20 - 16 = 4
4. Total Subnets: 2⁴ = 16
- Result: **16 subnets**
- Check: Each subnet has 2^(32 - 20) = 4096 addresses, and 16 × 4096 = 65,536 (size of `172.90.0.0/16`).

---

#### Practical Use
- **Quick Mental Math**: For /19 in Class B, think “19 - 16 = 3, 2³ = 8.” It’s fast once you know the class!
- **Pattern**: 
  - Borrow 1 bit → 2 subnets
  - Borrow 2 bits → 4 subnets
  - Borrow 3 bits → 8 subnets
  - Borrow 4 bits → 16 subnets
- **No Binary Needed**: If you have the /n values, subtract and power up 2—done!

---

### Finding the Total Number of Possible Addresses
The "Total Number of Possible Addresses" is the full count of IP addresses in a subnet, regardless of whether they’re usable. It’s determined entirely by the subnet mask’s CIDR notation, which tells us how many host bits are available.

- **New Example**:  
  - **IP Address**: `172.20.150.45`  
  - **Subnet Mask**: `255.255.252.0` (/22)  
  - **Class**: B (default mask: `255.255.0.0` or /16)

---

#### Shortcut Method for Total Number of Possible Addresses

The shortcut is super simple: it’s just a single calculation based on the CIDR value. No exclusions, no fuss—just pure address space.

##### Step 1: Identify the CIDR Notation
- Look at the subnet mask and find the number of 1s in its binary form (or use the /n if given).
- For `255.255.252.0`:
  - Binary: `11111111.11111111.11111100.00000000`
  - Count of 1s: 8 + 8 + 6 = 22 → /22

##### Step 2: Calculate Total Addresses
- Formula: **Total Addresses = 2^(32 - CIDR)**
  - 32 is the total bits in an IP address.
  - CIDR is the number of network bits.
  - 32 - CIDR = number of host bits.
- For /22:
  - Host bits = 32 - 22 = 10
  - Total addresses = 2¹⁰ = 1024

##### Result
- Total Number of Possible Addresses: **1024**

---

#### Why This Works
- **Host Bits**: The /22 mask uses 22 bits for the network, leaving 10 bits for hosts. Each host bit doubles the address space (2¹⁰ = 1024).
- **Range**: For `172.20.150.45`:
  - Network Address: `172.20.148.0` (150 & 252 = 148)
  - Broadcast Address: `172.20.151.255` (148 to 151 = 4 × 256 = 1024 addresses)
- **No Subtraction**: Unlike usable addresses, we don’t subtract anything—1024 is the full block, including `172.20.148.0` and `172.20.151.255`.

---

#### General Shortcut Formula
For any subnet mask:
1. **Find CIDR**: Count the 1s in the mask’s binary (or use /n).
2. **Total Addresses**: 2^(32 - CIDR).

---

#### Another Class B Example
- **IP**: `172.100.10.200`
- **Mask**: `255.255.224.0` (/19)
1. CIDR: /19 (8 + 8 + 3 = 19)
2. Total Addresses: 2^(32 - 19) = 2¹³ = 8192
- Result: **8192 possible addresses**
- Check: Network = `172.100.0.0`, Broadcast = `172.100.31.255` (32 × 256 = 8192).

---

#### Practical Use
- **Quick Mental Math**: For /22, think “32 - 22 = 10, 2¹⁰ = 1024.” It’s instant once you know the CIDR!
- **Pattern**: 
  - /24 → 2⁸ = 256
  - /23 → 2⁹ = 512
  - /22 → 2¹⁰ = 1024
  - /21 → 2¹¹ = 2048
  - /20 → 2¹² = 4096
- **No Binary Needed**: If you have the /n, you skip all conversions—just subtract and power up 2.

---

### Finding the Total Number of Usable Addresses
The "Total Number of Usable Addresses" is the count of IP addresses in a subnet that can be assigned to devices, excluding the network address (first IP) and broadcast address (last IP). For a subnet, this depends on its size, which is determined by the subnet mask.

- **New Example**:  
  - **IP Address**: `172.16.45.78`  
  - **Subnet Mask**: `255.255.240.0` (/20)  
  - **Class**: B (default mask: `255.255.0.0` or /16)

---

#### Shortcut Method for Total Number of Usable Addresses

The shortcut hinges on one key idea: the number of usable addresses is the total number of addresses in the subnet minus 2 (for network and broadcast). We can find this quickly using the CIDR notation.

##### Step 1: Identify the CIDR Notation
- Look at the subnet mask and count the number of 1s in its binary form (or use the /n notation if given).
- For `255.255.240.0`:
  - Binary: `11111111.11111111.11110000.00000000`
  - Count of 1s: 8 + 8 + 4 = 20 → /20

##### Step 2: Calculate Total Addresses
- Total addresses in a subnet = 2^(32 - CIDR), where 32 is the total bits in an IP address.
- For /20:
  - Host bits = 32 - 20 = 12
  - Total addresses = 2¹² = 4096

##### Step 3: Subtract Network and Broadcast
- Usable addresses = Total addresses - 2
- For 4096:
  - Usable = 4096 - 2 = 4094

##### Result
- Total Number of Usable Addresses: **4094**

---

#### Why This Works
- **Subnet Size**: The /20 mask leaves 12 host bits. Each bit doubles the address space (2¹² = 4096), covering all possible IPs in the subnet (e.g., `172.16.32.0` to `172.16.47.255`).
- **Exclusions**: The first IP (`172.16.32.0`, network) and last IP (`172.16.47.255`, broadcast) are reserved, leaving 4094 usable IPs.

Let’s verify with the network address:
- IP: `172.16.45.78`
- Mask: `255.255.240.0`
- AND: `172.16.32.0` (network)
- Broadcast: `172.16.47.255` (32 to 47 in third octet = 16 × 256 = 4096 addresses)
- Usable: `172.16.32.1` to `172.16.47.254` → 4094 IPs.

---

#### General Shortcut Formula
For any subnet mask:
1. **Find CIDR**: Convert mask to /n (count 1s in binary).
2. **Total Addresses**: 2^(32 - CIDR).
3. **Usable Addresses**: Total - 2.

---

#### Another Class B Example
- **IP**: `172.30.200.10`
- **Mask**: `255.255.252.0` (/22)
1. CIDR: /22 (8 + 8 + 6 = 22)
2. Total Addresses: 2^(32 - 22) = 2¹⁰ = 1024
3. Usable: 1024 - 2 = 1022
- Result: **1022 usable addresses**
- Check: Network = `172.30.200.0`, Broadcast = `172.30.203.255` (4 × 256 = 1024 addresses).

---

#### Practical Use
- **Quick Mental Math**: For /20, think “32 - 20 = 12, 2¹² = 4096, minus 2 = 4094.” No need to list addresses!
- **Pattern**: 
  - /24 → 256 - 2 = 254
  - /23 → 512 - 2 = 510
  - /22 → 1024 - 2 = 1022
  - /21 → 2048 - 2 = 2046
  - /20 → 4096 - 2 = 4094


### Finding the Location of the IP Address on what Subnet
The subnet location of an IP address identifies which specific subnet the IP belongs to within a subdivided network. This is expressed as an ordinal position (e.g., the 1st, 2nd, or 5th subnet) based on the order of subnets created by borrowing bits from the host portion of the default class mask, with numbering starting at 1 as per instructional preference.

Yes, there’s definitely a shortcut to find the "Location of the IP Address on what Subnet" (i.e., determining which subnet an IP belongs to within a larger network divided into smaller subnets). This shortcut simplifies the process compared to listing all subnets or doing extensive binary calculations. Let’s break it down step-by-step, using your example (`172.50.100.121` with subnet mask `255.255.248.0` or /21) to illustrate, and then I’ll explain the general method.

---

#### Understanding the Problem
You want to know which subnet number (e.g., Subnet 1, Subnet 13, etc.) contains a given IP address when a larger network (like a Class B `172.50.0.0/16`) is subnetted (e.g., into /21 subnets). The "location" is essentially the subnet’s position in the sequence of all possible subnets.

In your example:
- **IP Address**: `172.50.100.121`
- **Subnet Mask**: `255.255.248.0` (/21)
- **Base Network**: `172.50.0.0/16` (Class B default)
- **Number of Subnets**: 32 (from 2⁵, borrowing 5 bits)
- **Subnet Size**: 2048 addresses (2^(32 - 21))
- **Increment**: 8 (in the third octet)

The program output identifies this IP as being in "Subnet 13" (`172.50.96.0 - 172.50.103.255`). The shortcut finds this number quickly.

---

#### The Shortcut Method

The shortcut relies on two key pieces of information:
1. **The network address of the specific subnet** containing the IP (found via bitwise AND).
2. **The increment** between subnets (determined by the subnet mask).

Here’s how it works:

##### Step 1: Find the Subnet’s Network Address
- Use the formula: **Network Address = IP Address AND Subnet Mask**
  - IP: `172.50.100.121`
  - Mask: `255.255.248.0`
  - Octet-by-octet:
    - `172 & 255 = 172`
    - `50 & 255 = 50`
    - `100 & 248 = 96` (100 = `01100100`, 248 = `11111000`, result = `01100000` = 96)
    - `121 & 0 = 0`
  - Network Address: `172.50.96.0`

This tells us the IP `172.50.100.121` belongs to the subnet starting at `172.50.96.0`.

##### Step 2: Determine the Increment
- The increment is the size of each subnet in the affected octet (where bits are borrowed).
- Subnet mask: `255.255.248.0` (/21)
- Default Class B mask: `255.255.0.0` (/16)
- Borrowed bits: 21 - 16 = 5
- In the third octet:
  - Default: 0 bits (all host bits)
  - New mask: `248` (`11111000`) → 3 borrowed bits
  - Remaining host bits in third octet: 8 - 3 = 5
  - Increment = 2^(8 - borrowed bits in octet) = 2⁵ = 8
- Each subnet steps by 8 in the third octet (e.g., `172.50.0.0`, `172.50.8.0`, `172.50.16.0`, …).

##### Step 3: Calculate the Subnet Location
- Formula: **Subnet Location = (Value of Affected Octet in Network Address ÷ Increment) + 1**
  - Affected octet: 3rd (where borrowing occurs)
  - Network Address: `172.50.96.0`
  - Third octet value: 96
  - Increment: 8
  - Calculation: `(96 ÷ 8) + 1 = 12 + 1 = 13`
- Result: The IP is in **Subnet 13**.

##### Why Add 1?
- Subnets are numbered starting from 1 (not 0). The division gives the zero-based position (12th subnet after `172.50.0.0`), so adding 1 adjusts to human-readable numbering (13th subnet).

---

#### Why This Works
- **Base Network**: `172.50.0.0` is Subnet 1.
- **Increment**: Each subnet increases by 8 in the third octet:
  - Subnet 1: `172.50.0.0`
  - Subnet 2: `172.50.8.0`
  - Subnet 3: `172.50.16.0`
  - …
  - Subnet 13: `172.50.96.0` (96 ÷ 8 = 12, +1 = 13)
- The division measures how many "steps" of 8 you take from the base (`172.50.0.0`) to reach the subnet’s network address (`172.50.96.0`).

---

#### General Shortcut Formula
For any IP and subnet mask:
1. **Find the Network Address**: IP AND Subnet Mask.
2. **Identify the Affected Octet**: The octet where bits are borrowed (e.g., 3rd octet for /21 in a Class B).
3. **Calculate the Increment**: 
   - Borrowed bits in the affected octet = Number of 1s in that octet’s mask binary - Default mask bits in that octet.
   - Increment = 2^(8 - borrowed bits in affected octet).
4. **Subnet Location**: (Affected octet value in network address ÷ Increment) + 1.

---

#### Another Example
- **IP**: `192.168.1.130`
- **Mask**: `255.255.255.192` (/26)
- **Class**: C (/24 default)
- **Step 1**: Network Address = `192.168.1.128`
  - `130 & 192 = 128` (130 = `10000010`, 192 = `11000000`, result = `10000000` = 128)
- **Step 2**: Increment
  - Borrowed bits: 26 - 24 = 2
  - 4th octet mask: `192` (`11000000`) → 2 bits borrowed
  - Increment = 2^(8 - 2) = 2⁶ = 64
- **Step 3**: Location = `(128 ÷ 64) + 1 = 2 + 1 = 3`
- **Result**: Subnet 3 (`192.168.1.128 - 192.168.1.191`)

---


