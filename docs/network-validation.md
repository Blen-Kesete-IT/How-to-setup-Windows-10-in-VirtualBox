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

Because Windows could already communicate with Kali, this indicated that the virtual network was functioning but inbound ICMP traffic to Windows required additional investigation.

## Troubleshooting Kali-to-Windows Connectivity

### Problem

Kali Linux could not receive ICMP Echo Replies from the Windows 10 VM. The test resulted in **100% packet loss**.

### Investigation

The Windows Defender Firewall configuration was reviewed. The Windows VM was connected using the **Public** network profile, and inbound ICMP Echo Requests can be blocked by the firewall even when other network connectivity works normally.

### Resolution

On the Windows 10 VM:

1. Open **Windows Defender Firewall**.
2. Select **Advanced settings**.
3. Select **Inbound Rules**.
4. Locate **File and Printer Sharing (Echo Request - ICMPv4-In)**.
5. Enable the rule that applies to the active **Public** network profile.
6. Keep Windows Defender Firewall enabled; only the required ICMP rule should be changed.
7. Return to Kali Linux and repeat the connectivity test.

```bash
ping -c 4 [WINDOWS-IP-REDACTED]
```

A successful test should report received packets and **0% packet loss**.

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

The lab demonstrates the process of testing Windows-to-Kali connectivity, outbound connectivity, and troubleshooting Kali-to-Windows ICMP communication. This troubleshooting exercise also demonstrates why host-based firewall rules must be considered when diagnosing apparent network connectivity failures.
