# Activity 6 - Switchport Security
## Static Configuration

### Violation 1: Protect Mode (SW-STATIC-PROTECTED)

**Topology:**
- **Switch:** SW-STATIC-PROTECTED (Cisco 2960)
- **Laptop:** LAPTOP-STATIC-A1 (connected via RS232 to console port for configuration)
- **Devices:**
  - PC-AUTHORIZED-A1 (Fa0/1): IP 10.10.10.2, MAC 0000.0CE1.D0B1, Status: Allowed
  - PC-AUTHORIZED-A2 (Fa0/2): IP 10.10.10.3, MAC 0002.173C.5B2E, Status: Allowed
  - ATTACKER-A1 (can tap Fa0/1 or Fa0/2): IP 10.10.10.9, MAC 0060.5C1E.8220, Status: Protected
- **Network:** 10.10.10.0/24, Subnet 255.255.255.0

**CLI Configuration:**
```plaintext
enable
configure terminal

interface FastEthernet0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation protect
 switchport port-security mac-address 0000.0CE1.D0B1

interface FastEthernet0/2
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation protect
 switchport port-security mac-address 0002.173C.5B2E

end
```

**Explanation:**
- `switchport mode access`: Sets the port to non-trunking access mode.
- `switchport port-security`: Enables port security.
- `switchport port-security maximum 1`: Limits the port to one MAC address.
- `switchport port-security violation protect`: Drops packets from unauthorized MAC addresses silently.
- `switchport port-security mac-address <MAC>`: Statically defines the allowed MAC address.

---

### Violation 2: Restrict Mode (SW-STATIC-RESTRICTED)

**Topology:**
- **Switch:** SW-STATIC-RESTRICTED (Cisco 2960)
- **Laptop:** LAPTOP-STATIC-A2 (connected via RS232 to console port for configuration)
- **Devices:**
  - PC-AUTHORIZED-A3 (Fa0/1): IP 192.168.250.2, MAC 0009.7CC5.9D2B, Status: Allowed
  - PC-AUTHORIZED-A4 (Fa0/2): IP 192.168.250.3, MAC 0090.2169.55E9, Status: Allowed
  - ATTACKER-A2 (can tap Fa0/1 or Fa0/2): IP 192.168.250.9, MAC 0004.9A7A.1E68, Status: Restricted
- **Network:** 192.168.250.0/24, Subnet 255.255.255.0

**CLI Configuration:**
```plaintext
enable
configure terminal

interface FastEthernet0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation restrict
 switchport port-security mac-address 0009.7CC5.9D2B

interface FastEthernet0/2
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation restrict
 switchport port-security mac-address 0090.2169.55E9

end
```

**Explanation:**
- `switchport port-security violation restrict`: Drops packets from unauthorized MAC addresses and logs the violation.

---

### Violation 3: Shutdown Mode (SW-STATIC-SHUTDOWN)

**Topology:**
- **Switch:** SW-STATIC-SHUTDOWN (Cisco 2960)
- **Laptop:** LAPTOP-STATIC-A3 (connected via RS232 to console port for configuration)
- **Devices:**
  - PC-AUTHORIZED-A5 (Fa0/1): IP 172.25.150.2, MAC 0000.0CE1.D0B1, Status: Allowed
  - PC-AUTHORIZED-A6 (Fa0/2): IP 172.25.150.3, MAC 0002.173C.5B2E, Status: Allowed
  - ATTACKER-A3 (can tap Fa0/1 or Fa0/2): IP 172.25.150.9, MAC 0060.5C1E.8220, Status: Shutdown
- **Network:** 172.25.150.0/24, Subnet 255.255.255.0

**CLI Configuration:**
```plaintext
enable
configure terminal

interface FastEthernet0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address 0000.0CE1.D0B1

interface FastEthernet0/2
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address 0002.173C.5B2E

end
```

**Explanation:**
- `switchport port-security violation shutdown`: Shuts down the port when an unauthorized MAC address is detected.

---

## Sticky/Dynamic Configuration

For sticky configurations, the topology mirrors the static setups, but the `switchport port-security mac-address sticky` command replaces static MAC definitions. The switch dynamically learns and secures the first MAC address connected to each port.

### Violation 1: Protect Mode (SW-STICKY-PROTECTED)

**Topology:**
- **Switch:** SW-STICKY-PROTECTED (Cisco 2960)
- **Laptop:** LAPTOP-STICKY-B1 (connected via RS232 to console port for configuration)
- **Devices:**
  - PC-AUTHORIZED-B1 (Fa0/1): IP 10.10.10.2, MAC 0000.0CE1.D0B1, Status: Allowed
  - PC-AUTHORIZED-B2 (Fa0/2): IP 10.10.10.3, MAC 0002.173C.5B2E, Status: Allowed
  - ATTACKER-B1 (can tap Fa0/1 or Fa0/2): IP 10.10.10.9, MAC 0060.5C1E.8220, Status: Protected
- **Network:** 10.10.10.0/24, Subnet 255.255.255.0

**CLI Configuration:**
```plaintext
enable
configure terminal

interface FastEthernet0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation protect
 switchport port-security mac-address sticky

interface FastEthernet0/2
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation protect
 switchport port-security mac-address sticky

end
```

**Explanation:**
- `switchport port-security mac-address sticky`: Dynamically learns and secures the first MAC address (e.g., 0000.0CE1.D0B1 on Fa0/1).

---

### Violation 2: Restrict Mode (SW-STICKY-RESTRICTED)

**Topology:**
- **Switch:** SW-STICKY-RESTRICTED (Cisco 2960)
- **Laptop:** LAPTOP-STICKY-B2 (connected via RS232 to console port for configuration)
- **Devices:**
  - PC-AUTHORIZED-B3 (Fa0/1): IP 192.168.250.2, MAC 0009.7CC5.9D2B, Status: Allowed
  - PC-AUTHORIZED-B4 (Fa0/2): IP 192.168.250.3, MAC 0090.2169.55E9, Status: Allowed
  - ATTACKER-B2 (can tap Fa0/1 or Fa0/2): IP 192.168.250.9, MAC 0004.9A7A.1E68, Status: Restricted
- **Network:** 192.168.250.0/24, Subnet 255.255.255.0

**CLI Configuration:**
```plaintext
enable
configure terminal

interface FastEthernet0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation restrict
 switchport port-security mac-address sticky

interface FastEthernet0/2
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation restrict
 switchport port-security mac-address sticky

end
```

---

### Violation 3: Shutdown Mode (SW-STICKY-SHUTDOWN)

**Topology:**
- **Switch:** SW-STICKY-SHUTDOWN (Cisco 2960)
- **Laptop:** LAPTOP-STICKY-B3 (connected via RS232 to console port for configuration)
- **Devices:**
  - PC-AUTHORIZED-B5 (Fa0/1): IP 172.25.150.2, MAC 0000.0CE1.D0B1, Status: Allowed
  - PC-AUTHORIZED-B6 (Fa0/2): IP 172.25.150.3, MAC 0002.173C.5B2E, Status: Allowed
  - ATTACKER-B3 (can tap Fa0/1 or Fa0/2): IP 172.25.150.9, MAC 0060.5C1E.8220, Status: Shutdown
- **Network:** 172.25.150.0/24, Subnet 255.255.255.0

**CLI Configuration:**
```plaintext
enable
configure terminal

interface FastEthernet0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky

interface FastEthernet0/2
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky

end
```

---

## Testing and Observing Violations

### Step-by-Step Instructions

1. **Connect Authorized Devices:**
   - For each switch (e.g., SW-STATIC-PROTECTED or SW-STICKY-PROTECTED), connect the authorized PCs to their respective ports:
     - Fa0/1: PC-AUTHORIZED-A1/B1 (MAC 0000.0CE1.D0B1)
     - Fa0/2: PC-AUTHORIZED-A2/B2 (MAC 0002.173C.5B2E)
   - For sticky configurations, the switch learns these MAC addresses dynamically.

2. **Test with Unauthorized Devices:**
   - Connect the attacker PC to either Fa0/1 or Fa0/2 and observe the behavior:
     - **Protect Mode (SW-STATIC-PROTECTED / SW-STICKY-PROTECTED):**
       - Connect ATTACKER-A1/B1 (MAC 0060.5C1E.8220).
       - **Expected Result:** Packets are dropped silently; no log or port shutdown.
     - **Restrict Mode (SW-STATIC-RESTRICTED / SW-STICKY-RESTRICTED):**
       - Connect ATTACKER-A2/B2 (MAC 0004.9A7A.1E68).
       - **Expected Result:** Packets are dropped, and a log entry is generated.
     - **Shutdown Mode (SW-STATIC-SHUTDOWN / SW-STICKY-SHUTDOWN):**
       - Connect ATTACKER-A3/B3 (MAC 0060.5C1E.8220).
       - **Expected Result:** Port shuts down and enters an error-disabled state.

3. **Monitor Switchport Security Status:**
   - Use these commands to observe the security status:
     ```plaintext
     show port-security interface FastEthernet0/1
     show port-security interface FastEthernet0/2
     show port-security
     ```
   - **What to Look For:**
     - **Secure MAC Addresses:** Static (e.g., 0000.0CE1.D0B1) or learned via sticky.
     - **Violation Counts:** Increases with each unauthorized attempt.
     - **Port Status:** "Secure-up" (normal), or "err-disabled" (shutdown mode).

4. **Recover from Shutdown Mode (if applicable):**
   - For ports in error-disabled state (Shutdown Mode):
     ```plaintext
     enable
     configure terminal
     interface FastEthernet0/1
      shutdown
      no shutdown
     end
     ```

---

## Expected Observations

- **Protect Mode:**
  - Traffic from ATTACKER-A1/B1 is silently dropped.
  - `show port-security` shows a violation count increase but no port status change.

- **Restrict Mode:**
  - Traffic from ATTACKER-A2/B2 is dropped, with log messages (e.g., `%PORT_SECURITY-2-PSECURE_VIOLATION`).
  - `show port-security` shows violation count and logged events.

- **Shutdown Mode:**
  - Port shuts down upon ATTACKER-A3/B3 connection.
  - `show port-security interface FastEthernet0/1` shows "err-disabled" status.

This activity provides a complete framework to configure, test, and monitor switchport security, meeting all specified requirements.