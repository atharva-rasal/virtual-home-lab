# 🛡️ Cybersecurity Home Lab Setup (3-in-1)

This guide walks you through setting up a fully functional cybersecurity home lab that includes:

- 🧨 Offensive Lab (Red Teaming)
- 🛡️ Defensive Lab (Blue Teaming / SIEM)
- 🧬 Malware Analysis Lab (Flare VM)

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

1. Install VMware Workstation Pro and configure virtual networks:
   - Use NAT network: `10.0.0.0/24`
   - Set DHCP range starting from `.10`

2. Extract and import Kali Linux VM and Metasploitable 2 VM into VMware.

3. Rename them for clarity (e.g., "Kali" and "Meta").

4. Assign both VMs to NAT network.

5. Start both VMs:
   - Kali login: `kali / kali`
   - Meta login: `msfadmin / msfadmin`

6. From Kali terminal:

   ```bash
   ping 10.0.0.11  # assuming Meta gets this IP
   sudo apt update && sudo apt upgrade -y
   ```

7. From Meta terminal:

   ```bash
   ping 10.0.0.10  # to check connection with Kali
   ```

8. Lab ready for use with tools like:
   - Nmap
   - Metasploit
   - Hydra
   - Nikto
   - Burp Suite
   - etc.

---

## 🛡️ Part 2: Defensive Lab (Blue Team / SOC)

### Objective
Monitor Ubuntu Desktop with Wazuh SIEM on Ubuntu Server.

### Steps

1. **Install Ubuntu Desktop (Client):**
   - Assign static IP: `10.0.0.12`
   - Login: `ubuntu / <your-password>`
   - Update:

     ```bash
     sudo apt update && sudo apt upgrade -y
     ```

2. **Install Ubuntu Server (Wazuh Manager):**
   - Use Ubuntu 20.04 LTS
   - Assign static IP: `10.0.0.13`
   - Update:

     ```bash
     sudo apt update && sudo apt upgrade -y
     ```

3. **Install Wazuh:**

   ```bash
   curl -sO https://packages.wazuh.com/4.8/wazuh-install.sh
   sudo bash ./wazuh-install.sh -a
   ```

4. **Change Default Admin Password:**

   ```bash
   curl -so wazuh-passwords-tool.sh https://packages.wazuh.com/resources/tools/passwords-tool/wazuh-passwords-tool.sh
   sudo bash wazuh-passwords-tool.sh -a -u admin -p 'Secret3Pass*word'
   sudo systemctl restart wazuh-manager
   ```

5. **Connect Agent (Ubuntu Desktop):**
   - Open Firefox and go to: [https://10.0.0.13](https://10.0.0.13)
   - Login: `admin / Secret3Pass*word`
   - Add Agent > Select Debian > Copy the given command

   ```bash
   sudo <paste-the-command-here>
   ```

6. **Restart the Agent:**

   ```bash
   sudo systemctl daemon-reexec
   sudo systemctl enable wazuh-agent
   sudo systemctl restart wazuh-agent
   ```

7. Confirm agent is active in Wazuh dashboard.

---

## 🧐 Part 3: Malware Analysis Lab (Isolated Environment)

### Objective
Install Windows 10 VM and convert it into a malware analysis lab using Flare VM.

### Steps

1. **Create New VM:**
   - Use Windows 10 ISO
   - Skip Microsoft account and disable all privacy settings

2. **Post-install Config:**
   - Disable:
     - Windows Defender
     - Real-time protection
     - Windows Updates

   > Use Group Policy Editor: `gpedit.msc`  
   Navigate to:  
   `Computer Configuration > Administrative Templates > Windows Components`

3. **Install Flare VM:**

   Open PowerShell as Administrator:

   ```powershell
   Set-ExecutionPolicy Bypass -Scope Process -Force
   iwr https://raw.githubusercontent.com/mandiant/flare-vm/master/install.ps1 -UseBasicParsing | iex
   ```

4. **Let Flare VM complete installation**  
   (can take over an hour and multiple reboots)

5. **Sandbox the VM:**
   - Change network to **Host-only** mode

---

## 📊 Final Notes

- Take **snapshots** at key stages (e.g., clean install, post-config).
- Document:
  - IP addresses
  - Credentials
  - Network configurations
- Keep your VMs and ISOs in **organized folders**.
- Practice:
  - Attacks from Kali → Metasploitable
  - Monitoring with Wazuh
  - Analyzing samples in Flare VM (offline only)

> ⚠️ Ensure safe handling of malware samples — always work in isolated, non-persistent environments!
