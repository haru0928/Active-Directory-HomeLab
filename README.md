# Enterprise Active Directory Home Lab

## 📌 Executive Summary

This project documents the deployment, configuration, and security management of an **Active Directory Domain Services (AD DS)** environment using **Windows Server 2022 Standard (Desktop Experience)**.

The lab is designed to simulate enterprise-grade Identity and Access Management (IAM), Directory Services, Group Policy enforcement, and centralized network administration. Constructed on an ARM64 Apple Silicon host via x86_64 virtualization/emulation, this environment demonstrates system administration, cross-architecture virtualization troubleshooting, and infrastructure engineering skills required for modern cyber operations.

---

## 📐 Environment & Hardware Architecture

| Component | Specification / Configuration |
| --- | --- |
| **Host Hardware** | Apple MacBook Air M4 (macOS / ARM64) |
| **Hypervisor** | UTM v4.x (QEMU Emulation Engine) |
| **Machine Profile** | `q35` Standard PC profile with Intel ICH9 chipset |
| **Guest OS** | Windows Server 2022 Standard Evaluation (`SERVER_EVAL_x64FRE_en-us.iso`) |
| **CPU / Memory** | 4 x86_64 Cores / 4096 MB RAM |
| **Storage Architecture** | 64 GiB Virtual NVMe Drive (`QCOW2`) + IDE CD/DVD Mount |
| **Virtual Network** | Isolated Host-Only / NAT Subnet |

---

## 🛠️ Phase 1: Hypervisor Engineering & Boot Optimization

Deploying x86_64 guest operating systems on Apple Silicon hosts introduces specific architectural constraints and boot path challenges. This phase details the system tuning performed during initial deployment:

### Architectural Adjustments & Troubleshooting

1. **EFI Firmware & Architecture Alignment:**
* Resolved initial QEMU firmware size constraints by standardizing the VM instance on the emulated `q35` system profile.


2. **Storage Controller Optimization:**
* Resolved virtual disk detection issues by configuring a dedicated 64 GiB NVMe virtual drive alongside an IDE CD/DVD optical slot.
* Cleaned up duplicate bus controllers to eliminate drive ambiguity in QEMU's UEFI bootloader.


3. **UEFI Boot File System Routing:**
* Manually targeted the Windows EFI Boot Manager path (`\EFI\Microsoft\BOOT\bootmgfw.efi`) via the Boot Maintenance Manager when adjusting the NVRAM boot sequence.



---

## ⚙️ Phase 2: Core Domain Controller Setup *(In Progress)*

The operational roadmap for promoting this instance into a fully functional Primary Domain Controller (`DC01`) encompasses:

* [x] **Base System Installation:** Windows Server 2022 Desktop Experience baseline installation.
* [x] **Account Provisioning:** Local Administrator hardening and setup.
* [ ] **Network Interface Configuration:** Assignment of static IPv4 parameters, Gateway, and loopback DNS binding.
* [ ] **Hostname & Domain Definition:** Computer rename to `DC01` and promotion to Forest Root Domain (`corp.local` / `lab.local`).
* [ ] **Identity & Access Management (IAM) Hierarchy:**
* Custom Organizational Unit (OU) schema implementation (`Executive`, `IT_Ops`, `SecOps`, `Workstations`).
* Creation of administrative and standard user accounts following the Principle of Least Privilege (PoLP).


* [ ] **Group Policy Objects (GPOs):**
* Centralized password policy enforcement.
* Audit logging & event collection rules.
* Local Administrator account control & workstation hardening.



---

## 🖼️ Network & Active Directory Topology

*(Architecture Diagram to be inserted here — e.g., Mermaid.js or draw.io graphic)*

```text
[ Apple Silicon M4 Host (macOS) ]
       │
       └── [ UTM / QEMU x86_64 Emulation Layer ]
                 │
                 ├── [ Windows Server 2022 (DC01) ]
                 │     ├── Role: AD DS / DNS / DHCP
                 │     ├── IP: 192.168.x.x (Static)
                 │     └── Domain: lab.local
                 │
                 └── [ Windows Client Endpoint ] (Planned)
                       └── Domain-Joined Workstation

```

---

## 🚀 Key Takeaways & Applied Concepts

* **Identity & Access Management (IAM):** Hands-on experience establishing enterprise directory boundaries, account lifecycles, and access control models.
* **Virtualization & Emulation:** Advanced configuration of QEMU/UTM storage buses, NVMe controllers, and cross-architecture hardware translation.
* **Systems Administration:** Deployment of core Microsoft infrastructure protocols (DNS, AD DS, Kerberos, SMB, GPO).

---

## 📜 Documentation & Author

* **Author:** Ricardo Escalante
* **Portfolio:** [Link to Portfolio Website]
* **Target Focus:** Cyberspace Operations / Enterprise Infrastructure Security
