# Lab Setup Guide

## 1. Create the Windows 10 Virtual Machine

In Oracle VirtualBox, create a new VM with:

- **Name:** Windows10-Lab (or similar)
- **Type:** Microsoft Windows
- **Version:** Windows 10 (64-bit)
- **Memory:** 4096 MB or more
- **Virtual disk:** VDI, dynamically allocated, minimum 40 GB

## 2. Attach the Windows ISO

Open the VM settings and go to **Storage**. Select the empty optical drive and attach the Windows 10 ISO.

Start the VM and complete the Windows installation:

1. Select language and keyboard options.
2. Choose **Install Now**.
3. Skip the product key if appropriate for the lab.
4. Choose **Custom: Install Windows only**.
5. Select the virtual disk.
6. Complete installation and initial Windows setup.

## 3. Configure VirtualBox Networking

Power off the Windows VM before changing its virtual network settings.

Go to:

**Settings > Network > Adapter 1**

Configure:

```text
Enable Network Adapter: Yes
Attached to:            NAT Network
Name:                   NatNetwork
Virtual Cable:          Connected
```

The Windows and Kali VMs must be attached to the **same NAT Network** for direct communication.

## 4. Configure a Static IPv4 Address in Windows

Open:

**Control Panel > Network and Sharing Center > Change adapter settings**

Then:

1. Right-click the Ethernet adapter.
2. Select **Properties**.
3. Select **Internet Protocol Version 4 (TCP/IPv4)**.
4. Click **Properties**.
5. Choose **Use the following IP address**.

Use:

```text
IP address:       10.0.0.10
Subnet mask:      255.255.255.0
Default gateway:  10.0.0.1
Preferred DNS:    8.8.8.8
```

## 5. Verify the Kali Address

On Kali Linux, verify that its lab interface is configured as expected:

```bash
ip addr
```

For this lab, Kali is expected to use:

```text
10.0.0.2/24
```

If the address is different, update the test commands or network configuration so both machines are on the same subnet.

## 6. Confirm Windows Configuration

Open Command Prompt and run:

```cmd
ipconfig
```

Confirm that the Ethernet adapter shows the intended address, subnet mask, and gateway.

## 7. Lab Design Considerations

A VirtualBox NAT Network is preferable to standard per-VM NAT when two VMs must communicate with each other. It provides a shared private virtual subnet while still allowing outbound access through NAT.

For a cybersecurity practice environment, keep offensive testing limited to systems you own or have explicit permission to assess. Avoid bridging an intentionally vulnerable lab machine directly onto a production or household LAN unless that exposure is specifically required and understood.
