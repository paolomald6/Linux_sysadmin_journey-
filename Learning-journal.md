## 2026-11-16 · Session S3: First Proxmox VM – Red Hat 10 Installation
- **Done:** Downloaded the Red Hat Enterprise Linux 10 ISO and attempted to create a first virtual machine on Proxmox VE using the command line
- **Broken:** VM failed to start due to an incompatible CPU configuration. The default KVM CPU type was not compatible with Red Hat 10
- **Fixed:** Debugged the issue through the Proxmox graphical interface and changed the VM CPU type from KVM to x86-64-v3, allowing the VM to boot
- **Learned:** How to create and configure virtual machines on Proxmox VE using the command line, troubleshoot VM boot issues through the graphical interface, and configure CPU types (KVM vs x86-64-v3) for OS compatibility.

## 2026-11-15 · Session S2: Proxmox VE Installation
- **Done:** Downloaded Proxmox VE 9.2-1 ISO, created a bootable USB using the dd command on Ubuntu, installed Proxmox VE, configured hostname pve, and accessed the web interface on port 8006
- **Broken:** Rufus on Windows 11 failed to create a working Proxmox installation USB
- **Fixed:** Used an Ubuntu machine and the dd command to write the Proxmox ISO directly to the USB flash drive
- **Learned:** How to install Proxmox VE, create bootable USB installation using dd, pveproxy (Web server for Administration interface).

## 2026-11-14 · Session S1: Lab Hardware Inventory & Git Setup
- **Done:** Checked the hardware specifications of each lab machine, created the Git repository, created the Hardware Overview directory, and documented the hardware specifications in 01-physical-machines.md
- **Broken:** Nothing
- **Fixed:** Nothing
- **Learned:** Linux commands to check hardware specifications on a machine and basic usage of GitHub. 
