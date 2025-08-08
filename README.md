# 🛡️ Cybersecurity Home Lab Setup (3-in-1)

This guide walks you through setting up a fully functional cybersecurity home lab that includes:

- 🧨 Offensive Lab (Red Teaming)
- 🛡️ Defensive Lab (Blue Teaming / SIEM)
- 🧬 Malware Analysis Lab (Flare VM)

> Based on a lab series by François from the Africana Institute of Technology.

---

## 🔧 System Requirements

- **RAM**: 16 GB minimum (32 GB recommended)
- **Disk**: 500 GB+ SSD
- **OS**: Windows 10/11 (64-bit)
- **Virtualization**: Enabled (check via Task Manager → Performance → CPU → Virtualization)

---

## 📦 Tools to Download

| Tool | Purpose |
|------|---------|
| [VMware Workstation Pro](https://www.vmware.com/products/workstation-pro.html) | Virtualization platform |
| [Kali Linux VM (VMware)](https://www.kali.org/get-kali/#kali-virtual-machines) | Penetration testing |
| [Metasploitable 2](https://sourceforge.net/projects/metasploitable/) | Vulnerable machine |
| [Ubuntu Desktop ISO](https://ubuntu.com/download/desktop) | Wazuh client |
| [Ubuntu Server 20.04 LTS ISO](https://releases.ubuntu.com/20.04/) | Wazuh manager |
| [Windows 10 ISO](https://www.microsoft.com/software-download/windows10) | Flare VM base |
| [7-Zip](https://www.7-zip.org/) | Extract compressed files |
| [Flare VM Script](https://github.com/mandiant/flare-vm) | Malware analysis toolkit |

---

## 🔴 Part 1: Offensive Lab (Kali + Metasploitable)

1. **Install VMware Workstation Pro** and configure virtual network:
   - Use NAT: `10.0.0.0/24`
   - DHCP start: `.10`

2. **Import VMs**:
   - Kali VM (default: `kali/kali`)
   - Metasploitable VM (default: `msfadmin/msfadmin`)

3. **Ensure both are on NAT network**  
   Test connectivity:
   ```bash
   ping 10.0.0.11  # From Kali
   ping 10.0.0.10  # From Metasploitable
