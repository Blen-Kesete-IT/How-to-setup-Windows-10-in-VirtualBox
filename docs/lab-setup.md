# Lab Setup Guide

> **Privacy:** Exact IP addresses, gateway values, DNS addresses, and other network identifiers are intentionally redacted from this public documentation.

## 1. Create the Windows 10 Virtual Machine

In Oracle VirtualBox, create a new VM with:

- **Name:** Windows10-Lab (or similar)
- **Type:** Microsoft Windows
- **Version:** Windows 10 (64-bit)
- **Memory:** 4096 MB or more
- **Virtual disk:** VDI, dynamically allocated, minimum 40 GB

## 2. Attach the Windows ISO

Open the VM settings and go to **Storage**. Select the empty optical drive and attach the Windows 10 ISO. Start the VM and complete the Windows installation.

## 3. Configure VirtualBox Networking

Power off the Windows VM before changing its virtual network settings. Go to **Settings > Network > Adapter 1** and configure the adapter to use the same private **NAT Network** as the Kali VM.

```text
Enable Network Adapter: Yes
Attached to:            NAT Network
Network name:           [REDACTED]
Virtual Cable:          Connected
```

## 4. Configure Static IPv4 in Windows

Open **Control Panel > Network and Sharing Center > Change adapter settings**, open the Ethernet adapter properties, and select **Internet Protocol Version 4 (TCP/IPv4)**.

The lab used a manually assigned private IPv4 configuration. The exact values are intentionally not published:

```text
IP address:       [REDACTED]
Subnet mask:      [REDACTED]
Default gateway:  [REDACTED]
DNS:              [REDACTED]
```

## 5. Verify the Kali Interface

On Kali Linux, verify its lab interface:

```bash
ip addr
```

Confirm that Kali and Windows are connected to the intended lab network. Do not publish the resulting network identifiers in public screenshots or documentation.

## 6. Confirm Windows Configuration

Open Command Prompt and run:

```cmd
ipconfig
```

Confirm that the Ethernet adapter shows the intended private configuration. Redact address information before publishing output.

## 7. Lab Design Considerations

A VirtualBox NAT Network provides a shared private virtual network for the lab VMs while supporting controlled outbound connectivity.

For a cybersecurity practice environment, keep testing limited to systems you own or have explicit permission to assess. Before publishing documentation, redact IP addresses, MAC addresses, usernames, hostnames, and other identifiers as required by the course or organization.
