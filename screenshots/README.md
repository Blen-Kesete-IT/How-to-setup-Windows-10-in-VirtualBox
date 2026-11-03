# Screenshots

Use this folder for visual evidence from the Windows 10 / Kali Linux VirtualBox lab.

Recommended filenames:

```text
01-virtualbox-windows10-vm.png
02-nat-network-settings.png
03-windows-static-ip.png
04-windows-ping-kali-and-internet.png
05-kali-ping-windows.png
```

## Evidence Checklist

- [ ] Windows 10 VM visible in Oracle VirtualBox Manager
- [ ] Adapter 1 configured for `NAT Network`
- [ ] NAT Network name shown as `NatNetwork`
- [ ] Windows IPv4 address shown as `10.0.0.10`
- [ ] Subnet mask shown as `255.255.255.0`
- [ ] Default gateway shown as `10.0.0.1`
- [ ] Preferred DNS shown as `8.8.8.8`
- [ ] Successful Windows → Kali ping (`10.0.0.2`)
- [ ] Successful Windows → Internet ping (`8.8.8.8`)
- [ ] Successful Kali → Windows ping (`10.0.0.10`)

The screenshots shared for this project already demonstrate the Windows VM, NAT Network selection, static IPv4 configuration, Windows-to-Kali connectivity, and Internet connectivity. Upload the image files into this folder to have them render directly in the repository.
