# Network Validation

This document records the connectivity checks used to validate the Windows 10 and Kali Linux lab network.

## Expected Addressing

| System | IPv4 Address | Purpose |
|---|---|---|
| VirtualBox NAT gateway | `10.0.0.1` | Default gateway |
| Kali Linux | `10.0.0.2` | Security testing VM |
| Windows 10 | `10.0.0.10` | Windows lab target |

Subnet mask: `255.255.255.0` (`/24`)

## Test 1 — Windows to Kali

From Windows Command Prompt:

```cmd
ping 10.0.0.2
```

Expected result:

```text
Reply from 10.0.0.2 ...
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

A successful result confirms that Windows can communicate with the Kali VM over the shared VirtualBox NAT Network.

## Test 2 — Windows to Internet

From Windows Command Prompt:

```cmd
ping 8.8.8.8
```

Expected result:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

This validates outbound IP connectivity through the VirtualBox NAT gateway.

## Test 3 — Kali to Windows

From Kali Linux:

```bash
ping 10.0.0.10
```

A successful response verifies bidirectional communication between the two lab systems.

## Troubleshooting

If Windows can ping Kali but Kali cannot ping Windows, check Windows Defender Firewall. Inbound ICMP echo requests may be blocked even when general network connectivity is working.

Prefer enabling the appropriate inbound rule instead of disabling the firewall entirely:

**Windows Defender Firewall with Advanced Security > Inbound Rules > File and Printer Sharing (Echo Request - ICMPv4-In)**

Also verify:

```cmd
ipconfig
route print
```

and on Kali:

```bash
ip addr
ip route
```

Both systems should be on the same `10.0.0.0/24` subnet and should use the intended VirtualBox NAT Network.

## Validation Result

The provided Windows evidence shows:

- Successful ping from Windows to Kali at `10.0.0.2`
- 4 packets sent, 4 received, 0 lost
- Successful ping from Windows to `8.8.8.8`
- 4 packets sent, 4 received, 0 lost

These results demonstrate working local lab connectivity and outbound Internet connectivity from the Windows VM.
