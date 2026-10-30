# NETWORKWALKS-B084-WK1-PM1-CYBERSECURITY-LAB-SETUP
# 🔐 Cybersecurity Lab Environment Setup
## 📌 Project Overview

This project sets up a virtual cybersecurity and penetration-testing laboratory using VMware Workstation and Kali Linux.

The lab provides a controlled environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security-testing activities can be performed safely and repeatedly.

The lab runs on a private virtual network, so additional machines can be added later as targets for authorized security testing.

## 🎯 Objectives

The main objectives of this project are to:

* Install and configure VMware Workstation.
* Import the Kali Linux virtual machine.
* Create a dedicated NAT network for the cybersecurity lab.
* Configure network connectivity for Kali Linux.
* Assign a consistent IP address to the Kali VM.
* Verify network connectivity and DNS resolution.
* Take a clean VM snapshot for recovery.
* Document the complete setup process.
* Prepare the environment for future cybersecurity projects.

## 🛡️ Purpose of the Lab

The lab provides an isolated, controlled environment for cybersecurity learning and authorized security testing.

It can be used for activities such as:

Network reconnaissance
Port scanning
Vulnerability assessment
Packet analysis
Web security testing
Exploitation practice
Security-tool experimentation

⚠️ Important: This laboratory must only be used for systems that you own or have explicit permission to test. Do not use the lab or its tools to attack unauthorized systems.
# 🏗️ Lab Architecture
<img width="1360" height="768" alt="image" src="https://github.com/user-attachments/assets/76fc3e4f-f94b-41ce-88cb-4ba8f521b59c" />

Additional target machines can be added to the same virtual network in future projects.

# ⚙️ Lab Configuration

| 🧩 Component | ⚙️ Configuration
| :--- | :--- |
| 🖥️ **Host OS** |	**Windows 10** |
| 🧠 **Host RAM**	|	**8 GB** |
| ⚡ **Processor**	|	**Intel Core i7** |
| 🧰 **Hypervisor**	|	**VMware Workstation Pro** |
| 🐉 **Security OS**	|	**Kali Linux 2026.2** |
| 🧠 **Kali RAM**	|	**2048 MB** |
| 📡 **Network Address**	|	**10.0.0.0/24** |
| 🐧 **Kali IP Address**	|	**10.0.0.2/24** |
| 🚪 **Default Gateway**	|	**10.0.0.1** |
| 🌍 **DNS Server**	|	**8.8.8.8** |
| 🔮 **Future VM Range**	|	**10.0.0.3–10.0.0.99** |

## 🪜 Lab Setup Procedure

### Step 1. Install 7-Zip

7-Zip was installed to extract the Kali Linux virtual machine package, which is distributed as a .7z archive.

**Tool:** 7-Zip

### Step 2. Install VMware Workstation

VMware Workstation Pro was installed as the hypervisor.

### Step 3. Configure the NAT Network

A dedicated NAT Network was created in VirtualBox.

Configuration: Network Name: NatNetwork IPv4 Prefix: 10.0.0.0/24 DHCP: Enabled IPv6: Disabled

<img width="1360" height="708" alt="image" src="https://github.com/user-attachments/assets/e7e4b11b-ee22-495e-9064-1e5a09ca14f4" />

A NAT network was selected because multiple virtual machines attached to the same VMnet can communicate with one another while also having outbound network connectivity. This allows future attacker and target VMs to communicate within the lab.

### Step 4. Import Kali Linux

<img width="1168" height="670" alt="image" src="https://github.com/user-attachments/assets/84b97352-87cd-4a7b-b0a9-e1bdaed86fe6" />

The Kali Linux virtual machine was downloaded from the official Kali Linux website and extracted with 7-Zip.

1. Open **File > Open** in VMware Workstation.
2. Select the extracted `.vmx` file.
3. Power on the VM and accept the import prompts if they appear.

The VM network adapter was configured as follows:

- **Network Adapter:** NAT (VMnet8)
- **Adapter Type:** VMXNET3 or Intel e1000, as shown in the VM settings

The VM was allocated:

- **RAM:** 2048 MB

A shared folder was also configured under **VM > Settings > Options > Shared Folders** to transfer required files between the host operating system and the Kali VM.

### Step 5. Configure the Kali Linux Network

The Kali Linux network configuration was checked and set to a consistent IPv4 address.

Example configuration:

- **IP Address:** 10.0.0.2
- **Subnet Mask:** 255.255.255.0
- **Gateway:** 10.0.0.1
- **DNS:** 8.8.8.8

A consistent IP address makes it easier to document the lab and reference the Kali machine in future exercises.

<img width="1161" height="650" alt="image" src="https://github.com/user-attachments/assets/e9a34199-3cea-420a-857d-da19cbf7eae5" />


### Step 6. Create a Clean VM Snapshot

After completing the initial configuration, a VMware snapshot was created.

1. Shut down or suspend the VM.
2. Select **VM > Snapshot > Take Snapshot**.
3. Name the snapshot.

**Example snapshot name:** `Clean Kali - Network Setup`

The snapshot represents the clean baseline of the laboratory. If a future exercise changes or damages the VM configuration, the machine can be restored to this baseline.

## 🔎 Lab Verification

| ✅ Test | 🧾 Command | 🎯 Expected Result |
|---|---|---|
| 🌐 Check IP address | `ip a` | Correct Kali IP displayed |
| 📡 Test gateway | `ping 10.0.0.1` | Successful replies |
| 🌍 Test Internet connectivity | `ping 8.8.8.8` | Successful replies |
| 🔎 Test DNS resolution | `nslookup networkwalks.com` | Domain resolves |
| 🧰 Verify Nmap | `nmap --version` | Nmap version displayed |
| 🔄 Verify snapshot | Restore snapshot and run `ip a` | Baseline configuration restored |

### Example Results

**IP Address:** 10.0.0.2/24
**Gateway:** 10.0.0.1
**DNS:** 8.8.8.8

## 🐞 Problems Encountered & Solutions

Documenting problems is an important part of the project.

### Problem 1. Internet Connectivity After Static IP Configuration

After manually configuring the IPv4 settings, Internet connectivity may fail depending on the Kali/NetworkManager configuration.

One workaround used during this lab was:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```

The network connection was then restarted or the VM rebooted, and connectivity was tested again.

> **Note:** Connection names may differ between systems. Run `nmcli connection show` first to identify your actual connection name before running the command.

### Problem 2. VMware VT-x / Virtualization Error

The VM initially failed to start because hardware virtualization was disabled in the system firmware (BIOS/UEFI).

The issue was resolved by:

1. Restarting the computer.
2. Entering BIOS/UEFI settings.
3. Enabling Intel VT-x / hardware virtualization.
4. Saving the configuration and restarting the computer.
5. Starting the Kali VM again.

After enabling virtualization, the VM started successfully.

If the VM still fails to start, check **VM > Settings > Processors** and confirm that **Virtualize Intel VT-x/EPT or AMD-V/RVI** is enabled. On Windows 10, Hyper-V or virtualization-based security features can also conflict with VMware. If you run into this, disable them in Windows Features and reboot.

## 💡 What I Learned

Through this project, I learned how to create and configure a virtual environment for cybersecurity practice.

The most important concepts I learned include:

### 1. NAT vs. Bridged vs. Host-Only Networking

Each VMware network mode serves a different purpose.

- **NAT** gives VMs outbound Internet access through the host, and VMs on the same VMnet can reach each other.
- **Bridged** places the VM directly on the physical network.
- **Host-only** isolates VMs from external networks.

A custom NAT configuration on VMnet8 makes it useful for building a multi-machine cybersecurity laboratory.

### 2. Virtual Machine Networking

I learned how VMware virtual network adapters connect virtual machines to different network types and how network configuration affects communication between machines.

### 3. Static IP Configuration

I learned how to configure and verify IPv4 addressing, subnet masks, gateways, and DNS settings in Kali Linux.

### 4. VM Snapshots

I learned that a clean snapshot should be created before performing risky or experimental activities. This provides a known-good recovery point for future cybersecurity exercises.

### 5. Documentation

I learned that documenting commands, configuration, screenshots, problems, and solutions is an important part of a professional cybersecurity project.

## 🔐 Security & Ethical Use

This laboratory is intended strictly for educational purposes. Use it only on systems you own or have explicit written permission to test.

## 🔗 Tools & Resources

- **7-Zip:** https://7-zip.org/download.html
- **VMware Workstation Pro:** https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion
- **Kali Linux:** https://kali.org/get-kali

## 👤 Author

**Raheem Ullah**
Cybersecurity Professional

**LinkedIn:** [linkedin.com/in/raheemullah-yousafzai-71ab5a283](https://linkedin.com/in/raheemullah-yousafzai-71ab5a283)

## 📌 Project Information

- **Program Name:** Cybersecurity at Networkwalks
- **Week:** 01
- **Project:** Cybersecurity & Pentesting Lab Setup
- **Repository:** GitHub
