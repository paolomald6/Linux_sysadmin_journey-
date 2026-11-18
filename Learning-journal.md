## 2026-11-18 · Session S5: Debian VM Installation, SSH Troubleshooting & Git Fundamentals
- **Done:** Created a new Debian VM on proxmox and installed Debian without a graphical interface, keeping the installation minimal (No GUI). Studied Git fundamentals. Installed `sudo` on Debian and configured sudo privileges for the user account.
- **Broken:** Could not connect to the Debian VM through SSH due to an incorrect password. The noVNC console was not registering Caps Lock correctly, resulting in a password being entered without uppercase letters.
- **Fixed:** Identified the noVNC keyboard issue as the root cause. Aftewards I changed all my password, root and user to add the uppercases (for security purposes).
- **Learned:** How to install a minimal Debian system without a desktop environment, troubleshoot SSH authentication issues, understand the differences Git and GitHub fundementals.
- 
## 2026-11-17 · Session S4: Proxmox VM Configuration & Red Hat 10 Installation
- **Done:** Deleted and recreated the virtual machine to improve the `qm create` command, configured CPU cores, network bridge, virtual disk using VirtIO, and set the CPU type to `x86-64-v3`. Installed Red Hat Enterprise Linux 10 and registered the system with Red Hat Subscription Manager. Downloaded the Debian ISO for the next virtual machine (wget)
- **Broken:** The initial VM creation commands I used yesterday, did not configure all the required parameters correctly, making it necessary for me to check on the GUI to get the VM running.
- **Fixed:** Refined the `qm create` command to configure the VM correctly from the start, including CPU type, core count, network bridge, and VirtIO disk
- **Learned:** How CPU cores and threads work together to allocate CPU resources efficiently to a virtual machine, how to register Red Hat Enterprise Linux 10 using Subscription Manager. And digging `qm create options`
  
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
