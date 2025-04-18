# Switch Port Security
## Overview

Switchport Security is a Layer 2 security feature on Cisco switches designed to mitigate unauthorized access to a network by restricting which devices can connect to specific switch ports. This feature is particularly useful in environments where physical access to switch ports might be possible, such as shared office spaces or public areas, as it prevents rogue devices from gaining network access.

## Key Features

### MAC Address-Based Control

Switchport Security operates by associating a switch port with one or more MAC addresses. By default, it allows only one MAC address (the first device that connects) when configured with the "sticky" option. This ensures that only the authorized device can communicate through that port. If another device with a different MAC address attempts to connect, the switch will detect a violation.

### Non-Trunking Port Requirement

Switchport Security is applied to access ports, not trunk ports, because trunk ports are typically used to carry traffic for multiple VLANs and are not meant for end-device connections. This ensures the feature is used in the correct context—securing individual device connections.

### Sticky MAC Address Learning

The "sticky" option is a dynamic yet restrictive feature. When enabled, the switch learns the MAC address of the first device that connects to the port and saves it in its running configuration. This MAC address becomes the only one allowed on that port until the configuration is changed. This is a balance between manual configuration (static) and fully dynamic learning, offering both automation and security.

## Configuration Options

### Static MAC Address

This requires an administrator to manually specify the allowed MAC address(es) for a port. While secure, it’s labor-intensive and doesn’t scale well in large networks.

### Sticky/Dynamic MAC Address

The sticky option automates the process by learning the first MAC address dynamically but then locks the port to that address. This is more practical for environments where devices are semi-permanent but may change occasionally.

## Handling Violations

When an unauthorized device (with a different MAC address) attempts to connect, the switch can respond in one of three ways:

### Protect Mode

Silently drops packets from the unauthorized device without notifying the administrator. The port remains active, which might be useful if you don’t want to disrupt legitimate users but still want to block unauthorized access.

### Restrict Mode

Drops packets and logs the violation, providing visibility into potential security issues. This is helpful for monitoring and auditing purposes.

### Shutdown Mode

The most severe action—shuts down the port entirely, requiring manual intervention to re-enable it. This is the default setting and ensures maximum security by completely blocking access until the issue is resolved.

## Practical Implications

Switchport Security is a critical tool for preventing MAC address spoofing and unauthorized network access. For example, in a university lab setting, enabling Switchport Security on lab computers ensures that only registered devices can connect, reducing the risk of students plugging in personal devices that might introduce malware or disrupt the network.

### Considerations

- **Scalability**: In environments with many devices or frequent device changes (e.g., a BYOD setting), sticky MAC addresses might require frequent reconfiguration.
    
- **Violation Handling**: Choosing the right violation mode depends on the network’s security needs. Shutdown mode might be too aggressive in a dynamic environment, while Protect mode might not provide enough visibility into potential threats.
    

## Next Steps for Exploration

- Dive deeper into configuration commands for enabling Switchport Security on a Cisco switch (e.g., using `switchport port-security` commands).
    
- Explore real-world scenarios where Switchport Security has mitigated specific threats.
    
- Discuss how Switchport Security integrates with other Layer 2 security features, like DHCP Snooping or Dynamic ARP Inspection.
    

## Configuration Example

```
interface FastEthernet0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
```

## Benefits

- Enhances network security by controlling access at the port level.
    
- Prevents unauthorized devices from connecting to the network.
    
- Provides flexibility with static and dynamic MAC address options.
    

## Considerations

- Ensure that MAC addresses are correctly configured to avoid accidental lockouts.
    
- Regularly review and update MAC address configurations to reflect changes in the network environment.