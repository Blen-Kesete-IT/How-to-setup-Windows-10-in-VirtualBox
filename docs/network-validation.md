# Network Validation

This document records the connectivity checks used to validate the Windows 10 and Kali Linux lab network without publishing network identifiers.

> Exact IP addresses, gateway values, DNS addresses, and network ranges are intentionally redacted.

## Addressing

| System | IPv4 Address | Purpose |
|---|---|---|
| VirtualBox NAT gateway | `[REDACTED]` | Default gateway |
| Kali Linux | `[REDACTED]` | Security testing VM |
| Windows 10 | `[REDACTED]` | Windows lab VM |

## Test 1 — Windows to Kali

From Windows Command Prompt, an ICMP echo test was sent to the Kali VM using its private lab address.

```cmd
ping [KALI-IP-REDACTED]
```

The validation evidence showed successful replies and **0% packet loss**, confirming Windows could reach the Kali VM.

## Test 2 — Outbound Connectivity

An ICMP test to an external destination was used to verify outbound connectivity through the VirtualBox NAT Network.

```cmd
ping [EXTERNAL-DESTINATION-REDACTED]
```

The validation evidence showed **0% packet loss**.

## Test 3 — Kali to Windows

From Kali Linux, the reverse connectivity test was performed with:

```bash
ping -c 4 [WINDOWS-IP-REDACTED]
```

### Initial Result

The initial Kali-to-Windows test failed:

```text
4 packets transmitted, 0 received, 100% packet loss
```

Because Windows could already communicate with Kali, this suggested that the virtual network was functioning but inbound ICMP traffic to Windows required additional investigation.

## Troubleshooting Kali-to-Windows Connectivity

### Problem

Kali Linux initially could not receive ICMP Echo Replies from the Windows 10 VM. The test resulted in **100% packet loss**.

### Investigation

I reviewed the Windows Defender Firewall configuration and checked the inbound firewall rules. The ICMP Echo Request rule required for the test was not enabled for the profile I needed.

### Extra Steps Taken

On the Windows 10 VM, I completed the following steps:

1. Opened **Windows Defender Firewall**.
2. Selected **Advanced settings**.
3. Selected **Inbound Rules**.
4. Located **File and Printer Sharing (Echo Request - ICMPv4-In)**.
5. Enabled the appropriate rule for the **Private** profile.
6. Kept Windows Defender Firewall enabled instead of disabling the firewall completely.
7. Returned to Kali Linux and repeated the connectivity test.

```bash
ping -c 4 [WINDOWS-IP-REDACTED]
```

### Successful Result

After enabling the appropriate inbound ICMPv4 rule, the Kali-to-Windows test succeeded:

```text
4 packets transmitted, 4 received, 0% packet loss
```

This confirmed that the VirtualBox network was working and that the original Kali-to-Windows connectivity problem was related to the Windows Defender Firewall inbound ICMP rule.

### What I Learned

This troubleshooting exercise showed that successful communication in one direction does not necessarily mean traffic will be permitted in the opposite direction. Host-based firewall rules can affect connectivity even when both virtual machines are on the same virtual network. It also demonstrated why it is better to enable only the required firewall rule instead of disabling the firewall entirely.

### Additional Diagnostic Commands

Windows:

```cmd
ipconfig
route print
```

Kali Linux:

```bash
ip addr
ip route
```

These commands can be used to verify interface addressing and routing. Their output should be redacted before being published.

## Security and Privacy Note

IP addresses and other network identifiers are intentionally masked in this public repository. Screenshots should also be reviewed and redacted before publication.

## Validation Result

The lab demonstrates successful Windows-to-Kali connectivity, outbound connectivity, and successful Kali-to-Windows communication after troubleshooting the Windows Defender Firewall configuration. The final reverse connectivity test completed with **0% packet loss**.
