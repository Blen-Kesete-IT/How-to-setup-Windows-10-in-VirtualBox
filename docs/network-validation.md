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

The validation evidence showed successful replies and **0% packet loss**, confirming communication between the two VMs.

## Test 2 — Outbound Connectivity

An ICMP test to an external destination was used to verify outbound connectivity through the VirtualBox NAT Network.

```cmd
ping [EXTERNAL-DESTINATION-REDACTED]
```

The validation evidence showed **0% packet loss**.

## Test 3 — Kali to Windows

From Kali Linux, a reverse connectivity test can verify bidirectional communication:

```bash
ping [WINDOWS-IP-REDACTED]
```

## Troubleshooting

If Windows can reach Kali but Kali cannot reach Windows, check Windows Defender Firewall. Inbound ICMP echo requests may be blocked even when general network connectivity is working.

Prefer enabling the appropriate inbound rule instead of disabling the firewall entirely:

**Windows Defender Firewall with Advanced Security > Inbound Rules > File and Printer Sharing (Echo Request - ICMPv4-In)**

Useful local troubleshooting commands include:

```cmd
ipconfig
route print
```

and on Kali:

```bash
ip addr
ip route
```

Do not publish the resulting IP addresses, routes, or other identifiers unless permitted by the instructor.

## Validation Result

The lab evidence demonstrates successful VM-to-VM connectivity and outbound network connectivity. Specific addresses are intentionally omitted from this public repository.
