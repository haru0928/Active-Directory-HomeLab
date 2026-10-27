# Enterprise Active Directory Home Lab & Security Baseline

## 📌 Executive Summary
This project documents the deployment, configuration, and security management of an enterprise-grade **Active Directory Domain Services (AD DS)** environment using **Windows Server 2022 Standard (Desktop Experience)** and a domain-joined **Windows 11 Client**.

Designed to simulate real-world Identity and Access Management (IAM), Directory Services, Group Policy enforcement, and centralized network administration. Built entirely on an **ARM64 Apple Silicon host via UTM**, this lab demonstrates systems administration, advanced cross-architecture virtualization troubleshooting, and infrastructure engineering skills tailored for modern cyber operations and 17S Cyberspace Operations readiness.

---

## 📐 Environment & Hardware Architecture

| Component | Specification / Configuration |
| :--- | :--- |
| **Host Hardware** | Apple MacBook Air M4 (macOS / ARM64) |
| **Hypervisor** | UTM v4.x (Hybrid Emulation & Native Virtualization Engines) |
| **Domain Controller (`DC01`)** | Windows Server 2022 Standard Evaluation (x86_64 via QEMU Emulation) |
| **Client Workstation (`WIN11-CLI01`)** | Windows 11 ARM64 (Native Apple Virtualization Framework) |
| **Network Architecture** | UTM Shared Network Subnet (`192.168.64.x`) |
| **Active Directory Domain** | `corp.local` |

---

## 🛠️ Phase 1: Hypervisor Engineering & Boot Optimization
Deploying x86_64 guest operating systems on Apple Silicon hosts introduces specific architectural constraints. The initial deployment involved:
* **EFI Firmware & Architecture Alignment:** Standardized the server VM instance on the emulated `q35` system profile with Intel ICH9 chipset to resolve QEMU firmware constraints.
* **Storage Controller Optimization:** Configured a dedicated 64 GiB NVMe virtual drive alongside an IDE optical slot, cleaning up duplicate bus controllers to eliminate bootloader ambiguity.
* **UEFI Boot File System Routing:** Manually targeted the Windows EFI Boot Manager path (`\EFI\Microsoft\BOOT\bootmgfw.efi`) via the UEFI Boot Maintenance Manager.

---

## 🏗️ Phase 2: Core Domain Controller & IAM Deployment
* **Domain Promotion:** Successfully promoted `DC01` to a Primary Domain Controller for the `corp.local` forest root domain with static IPv4 addressing (`192.168.64.10`) and loopback DNS binding.
* **Identity & Access Management (IAM) Hierarchy:** Implemented a structured custom Organizational Unit (OU) schema enforcing the Principle of Least Privilege (PoLP):
  * `corp.local/`
    * `Corp/`
      * `IT_Ops` (IT Operations personnel)
      * `SecOps` (Security Operations personnel)
      * `Users` (Standard enterprise users, e.g., `tuser`)
      * `Workstations` (Domain-joined client endpoints)

---

## 🌐 Phase 3: Cross-Architecture Networking & Client Integration
* **The Challenge:** UTM runs x86_64 emulation (`DC01`) and ARM64 native virtualization (`WIN11-CLI01`) on separate backend hypervisor engines, leading to default subnet isolation and packet destination unreachability.
* **The Resolution:** 
  * Standardized both VM instances onto UTM's **Shared Network (`192.168.64.x`)**.
  * Pinned `DC01` to static IP `192.168.64.10`.
  * Configured the client network adapter (`ncpa.cpl`) with explicit IPv4 DNS pointing to `192.168.64.10` while disabling IPv6 link-local interference (`fe80::...`).
* **Domain Join:** Successfully joined `WIN11-CLI01` to `corp.local`, verified handshakes via `nslookup` and `ping`, and moved the computer object into the `Workstations` OU.

---

## 🔒 Phase 4: Security Hardening & Group Policy Enforcement
To demonstrate endpoint security hardening and defensive posture:
* **GPO Creation:** Created and linked the `Workstation-Security-Baseline` Group Policy Object directly to the `Workstations` OU.
* **Account Lockout Policy:** Configured an explicit lockout threshold of **5 invalid logon attempts** to mitigate brute-force vector threats.
* **Policy Verification:** Forced immediate policy application across the endpoint using `gpupdate /force`.

---

## 🖼️ Network Topology Diagram

```text
[ Apple Silicon M4 Host (macOS) ]
       │
       ├── [ UTM QEMU Emulation Layer ]
       │     └── [ DC01: Windows Server 2022 ]
       │           ├── Role: AD DS / DNS / DC
       │           └── IP: 192.168.64.10 (Static)
       │           └── Domain: corp.local
       │
       └── [ UTM Apple Virtualization Framework ]
             └── [ WIN11-CLI01: Windows 11 ARM64 ]
                   ├── Role: Domain-Joined Workstation
                   └── OU: Workstations

```

---

## 🚀 Key Takeaways & Applied Concepts

* **Identity & Access Management (IAM):** Hands-on experience establishing enterprise directory boundaries, OU design, account lifecycles, and access control models.
* **Virtualization & Hypervisor Troubleshooting:** Advanced configuration of mixed-architecture environments (QEMU x86_64 emulation + ARM64 native virtualization), bridging/subnet routing, and network adapter troubleshooting.
* **Systems Administration & Hardening:** Deployment of core Microsoft infrastructure protocols (DNS, AD DS, Kerberos, SMB) and proactive security baselining via GPOs.

---

## 📸 Evidence & Screenshots
<img width="1090" height="852" alt="AD_Users_and_Computers" src="https://github.com/user-attachments/assets/4ef92f28-927d-4c9c-8438-dc2f61e67019" />
<img width="1319" height="1023" alt="hostnames_IP_Addresses" src="https://github.com/user-attachments/assets/274536aa-987f-4a05-bdbf-e1d66b67a427" />

## 📜 Documentation & Author

* **Author:** Ricardo Escalante
* **Target Focus:** Cyberspace Operations / Enterprise Infrastructure Security (17S)
