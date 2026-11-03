# Windows 10 Cybersecurity Lab in VirtualBox

This project documents the setup of a Windows 10 virtual machine in Oracle VirtualBox for use in a small cybersecurity practice lab alongside Kali Linux.

The goal of the lab is to build a controlled virtual environment, place both machines on the same isolated VirtualBox NAT Network, configure predictable IP addressing, and validate communication between the Windows and Kali systems.

## Lab Overview

| Component | Configuration |
|---|---|
| Hypervisor | Oracle VirtualBox |
| Windows VM | Windows 10 64-bit |
| Kali VM | Kali Linux |
| Virtual Network | VirtualBox NAT Network (`NatNetwork`) |
| Windows IP | `10.0.0.10/24` |
| Kali IP | `10.0.0.2/24` |
| Default Gateway | `10.0.0.1` |
| DNS | `8.8.8.8` |

## Network Topology

```text
                     Internet
                        |
                 VirtualBox NAT
                        |
              Gateway: 10.0.0.1
                        |
              NatNetwork (10.0.0.0/24)
                 /                 \
                /                   \
        Kali Linux VM          Windows 10 VM
          10.0.0.2               10.0.0.10
```

## Objectives

- Create a Windows 10 virtual machine in VirtualBox.
- Install Windows 10 from an ISO image.
- Attach the Windows VM to the same NAT Network as Kali Linux.
- Configure a static IPv4 address on Windows.
- Verify Windows-to-Kali connectivity.
- Verify Internet connectivity from Windows.
- Document the lab with screenshots and repeatable steps.

## Windows 10 VM Configuration

The Windows 10 VM was created with the following general settings:

- **Operating system:** Windows 10 (64-bit)
- **Memory:** at least 4 GB recommended
- **Virtual disk:** dynamically allocated VDI, at least 40 GB
- **Network adapter:** Intel PRO/1000 MT Desktop
- **Network mode:** NAT Network
- **NAT Network name:** `NatNetwork`

## Static IPv4 Configuration

Inside Windows 10, the Ethernet adapter was configured manually with:

```text
IP address:       10.0.0.10
Subnet mask:      255.255.255.0
Default gateway:  10.0.0.1
Preferred DNS:    8.8.8.8
```

This provides the Windows VM with a predictable address for lab exercises and makes host-to-host testing easier.

## Connectivity Validation

From the Windows 10 command prompt, the Kali VM was tested with:

```cmd
ping 10.0.0.2
```

The test returned four successful replies with **0% packet loss**, confirming communication from Windows to Kali.

Internet reachability was then tested with:

```cmd
ping 8.8.8.8
```

This also returned successful replies with **0% packet loss**, confirming that the Windows VM could reach the Internet through the VirtualBox NAT Network.

The reverse connectivity test should also be performed from Kali Linux:

```bash
ping 10.0.0.10
```

If Windows does not respond to inbound ping requests, verify the Windows Defender Firewall ICMPv4 Echo Request rule rather than disabling the firewall completely.

## Evidence

Screenshots from the completed lab should be stored in the `screenshots/` directory and can include:

1. VirtualBox Windows VM overview
2. Windows NAT Network configuration
3. Windows static IPv4 settings
4. Windows-to-Kali successful ping
5. Windows Internet connectivity test
6. Kali-to-Windows successful ping

Suggested filenames:

```text
screenshots/
├── 01-virtualbox-windows10-vm.png
├── 02-nat-network-settings.png
├── 03-windows-static-ip.png
├── 04-windows-ping-kali.png
└── 05-kali-ping-windows.png
```

## Security Notes

This environment is intended for authorized lab and educational use only. Keeping the systems inside a VirtualBox NAT Network reduces direct exposure to the physical LAN while still allowing the VMs to communicate with each other and reach the Internet when required.

A NAT Network is useful for a cybersecurity lab because it provides a repeatable internal subnet while maintaining separation from production systems. Before performing security testing, verify that all targets are systems you own or are explicitly authorized to test.

## Skills Demonstrated

- Virtual machine deployment
- Windows installation and configuration
- VirtualBox networking
- IPv4 subnet configuration
- Static IP addressing
- ICMP troubleshooting
- Kali Linux and Windows interoperability
- Basic cybersecurity lab segmentation
- Technical documentation

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

> This repository documents a personal training environment. All testing should be performed only on systems and networks you own or have explicit permission to assess.
