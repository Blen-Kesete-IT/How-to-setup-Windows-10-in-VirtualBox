# Windows 10 Cybersecurity Lab in VirtualBox

This project documents the setup of a Windows 10 virtual machine in Oracle VirtualBox for use in a small cybersecurity practice lab alongside Kali Linux.

The goal is to build a controlled virtual environment, place both machines on the same isolated VirtualBox NAT Network, configure predictable private addressing, and validate communication between the Windows and Kali systems.

> **Privacy note:** IP addresses and other network identifiers are intentionally redacted from this public repository.

## Lab Overview

| Component | Configuration |
|---|---|
| Hypervisor | Oracle VirtualBox |
| Windows VM | Windows 10 64-bit |
| Kali VM | Kali Linux |
| Virtual Network | VirtualBox NAT Network |
| Addressing | Static private IPv4 configuration (redacted) |

## Network Topology

```text
                     Internet
                        |
                 VirtualBox NAT
                        |
                Private Gateway
                        |
                 Lab NAT Network
                 /             \
                /               \
        Kali Linux VM       Windows 10 VM
        [IP REDACTED]       [IP REDACTED]
```

## Objectives

- Create a Windows 10 virtual machine in VirtualBox.
- Install Windows 10 from an ISO image.
- Attach the Windows VM to the same NAT Network as Kali Linux.
- Configure a static private IPv4 address on Windows.
- Verify Windows-to-Kali connectivity.
- Verify outbound connectivity from Windows.
- Verify Kali-to-Windows connectivity.
- Document the lab without publishing network identifiers.

## Windows 10 VM Configuration

The Windows 10 VM was created with the following general settings:

- **Operating system:** Windows 10 (64-bit)
- **Memory:** at least 4 GB recommended
- **Virtual disk:** dynamically allocated VDI, at least 40 GB
- **Network adapter:** Intel PRO/1000 MT Desktop
- **Network mode:** NAT Network

## Static IPv4 Configuration

Inside Windows 10, the Ethernet adapter was manually configured with a private IPv4 address, subnet mask, gateway, and DNS configuration. Exact values are intentionally omitted from this public repository.

```text
IP address:       [REDACTED]
Subnet mask:      [REDACTED]
Default gateway:  [REDACTED]
DNS:              [REDACTED]
```

## Connectivity Validation

Connectivity was tested from Windows to the Kali VM and from the lab environment to an external destination. The tests completed successfully with **0% packet loss** in the captured validation evidence.

Exact addresses are not published. Screenshots intended for public use should have IP addresses and other network identifiers blurred or covered before upload.

## Troubleshooting: Kali Could Not Ping Windows

During the reverse connectivity test, Kali Linux was initially unable to ping the Windows 10 VM.

The first test showed:

```text
4 packets transmitted, 0 received, 100% packet loss
```

Because Windows could already communicate with Kali, I investigated the Windows host firewall rather than changing the VirtualBox NAT Network.

### Extra Steps I Took

On the Windows 10 VM, I opened **Windows Defender Firewall with Advanced Security** and completed these steps:

1. Selected **Inbound Rules**.
2. Located **File and Printer Sharing (Echo Request - ICMPv4-In)**.
3. Identified the rule associated with the **Private** profile.
4. Right-clicked the rule and selected **Enable Rule**.
5. Kept Windows Defender Firewall enabled instead of disabling it completely.
6. Returned to Kali Linux and repeated the ping test using the Windows VM's private lab address.

The public command is shown with the address redacted:

```bash
ping -c 4 [WINDOWS-IP-REDACTED]
```

### Result

After enabling the appropriate ICMPv4 inbound rule, the Kali-to-Windows connectivity test succeeded:

```text
4 packets transmitted, 4 received, 0% packet loss
```

This confirmed that the VirtualBox network was working and that the failed reverse ping was caused by Windows Defender Firewall blocking inbound ICMP Echo Requests.

### What I Learned

This troubleshooting step demonstrated that successful connectivity in one direction does not guarantee that return traffic will be permitted in the opposite direction. Host-based firewall rules can affect ICMP testing even when two systems are correctly configured on the same virtual network.

It also reinforced the better security practice of enabling only the required firewall rule instead of disabling the firewall entirely.

For more detailed validation notes, see [`docs/network-validation.md`](docs/network-validation.md).

## Evidence

Public screenshots can document:

1. VirtualBox Windows VM overview
2. NAT Network configuration with identifiers redacted
3. Windows static IPv4 configuration with address fields redacted
4. Successful Windows-to-Kali connectivity with addresses redacted
5. Successful outbound connectivity with destination information redacted where required
6. Windows Defender Firewall inbound rule used for troubleshooting
7. Successful Kali-to-Windows connectivity with addresses redacted

## Security and Privacy Notes

This environment is intended for authorized lab and educational use only. Network identifiers are intentionally excluded from the public documentation in accordance with lab privacy requirements.

Before publishing screenshots, review the entire image for IP addresses, usernames, hostnames, MAC addresses, email addresses, tokens, keys, or other information that should not be public.

All security testing should be performed only against systems and networks you own or have explicit authorization to assess.

## Skills Demonstrated

- Virtual machine deployment
- Windows installation and configuration
- VirtualBox networking
- IPv4 configuration
- Static addressing
- ICMP connectivity validation
- Windows Defender Firewall troubleshooting
- Kali Linux and Windows interoperability
- Cybersecurity lab segmentation
- Security-conscious technical documentation
- Information redaction and privacy awareness

## Repository Structure

```text
.
├── README.md
├── docs/
│   ├── lab-setup.md
│   └── network-validation.md
└── screenshots/
    └── README.md
```

## Author

**Blen Cyber**  
Cybersecurity lab and hands-on IT practice

---

> This repository documents a personal training environment. Sensitive network configuration details are intentionally redacted.
