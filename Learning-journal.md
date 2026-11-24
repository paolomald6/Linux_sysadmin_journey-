## 2026-11-24 · Session S10: QEMU guest agent and systemd service dependency
- **Done:** Wrote the documentation for my first systemd service (`Docs/03-Systemd.md`). Fixed syntax mistakes in `hello.service` and added `Requisite=sshd.service` and `After=sshd.service`, so the service that writes "SSH is working" to a log file every 10 seconds only runs when SSH is already up. Installed and enabled the QEMU guest agent in a VM, then queried its network interfaces from the Proxmox shell with `qm agent`.
- **Broken:** The guest agent did not work from Proxmox: I had installed it in the VM but had not enabled it in the VM's Proxmox configuration.
- **Fixed:** Enabled the agent in the VM options with `qm set <VMID> --agent 1`. After that, `qm agent <VMID> network-get-interfaces` returned the VM's network interfaces.
- **Learned:** The QEMU guest agent runs inside the VM and lets Proxmox talk to it directly, without SSH. It must be installed in the VM and enabled in the VM's Proxmox configuration; installing it alone on VM is not enough. In a systemd unit, `Requisite=` makes a service depend on another one, and `After=` sets the start order.
- **To explore:** Which other `qm agent` commands exist ? Does enabling the agent option require a full stop and start of the VM?
- **Reference:** [Docs/03-Systemd.md](Docs/03-Systemd.md)

## 2026-11-23 · Session S9: SSH access for a new user and first systemd timer
- **Done:** Created a test user with `adduser`, a group with `addgroup`, and added my users to it with `gpasswd -a`. Added `AllowGroups <group>` to the SSH configuration, copied `authorized_keys` to the new user with `cp -p`, and logged in over SSH as that user. Wrote my first systemd service and timer (`hello.service`, `hello.timer`) that log "SSH is working" every 10 seconds, and I followed the output with `tail -f`.
- **Broken:** The new user could not log in with the copied public key because the file was not owned by him. The timer also fired every minute instead of every 10 seconds.
- **Fixed:** Changed the ownership with `chown test:<group> .ssh/authorized_keys`. After running `systemctl enable` on the timer, it ran every 10 seconds. I have not yet confirmed why it fired every minute before.
- **Learned:** With public-key-only SSH, the user's `~/.ssh/authorized_keys` must belong to that user. `cp -p` keeps the original owner, so the copied file needed a `chown`. `AllowGroups` limits SSH login to members of the listed groups. In systemd, the `.timer` file defines the timing and the `.service` file defines the commands to run.
- **To explore:** Why did the timer fire every minute before `systemctl enable`?

## 2026-11-22 · Session S8: Git fundamentals and GitHub SSH authentication
- **Done:** Configured Git on the Debian VM and cloned the repository. Tested `clone`, `push`, `pull`, `fetch` and `diff`. Edited repository files from Debian and pushed the changes to GitHub. Set up SSH key authentication so Git can connect to GitHub without a password.
- **Broken:** Confused the local repository with the remote one, and the different names given to branches.
- **Fixed:** Read the Git documentation and watched tutorials, then tried each command on the repository to see what it changed.
- **Learned:** `git clone` copies a remote repository to the local machine. `git push` sends local commits to the remote. `git pull` fetches remote changes and integrates them. `git fetch` only downloads them, without changing my files. `git diff` shows the differences between file versions. For GitHub SSH authentication, rather similar to what I did last week for Proxmox SSH.

## 2026-11-20 · Session S7: Ubuntu VM & Proxmox SSH Security Hardening
- **Done:** Created a new Ubuntu VM to follow the Linux Upskill Challenge. Hardened Proxmox SSH access by creating a dedicated administrative user, disabling root SSH login, and configuring restricted sudo privileges for specific administrative tasks such as system updates and log inspection.
- **Broken:** Had difficulty understanding some of the commands and aliases defined in the custom `sudoers` configuration, particularly how they restricted the user's administrative permissions.
- **Fixed:** Researched and analyzed the commands and sudoers aliases to understand their purpose and the exact scope of permissions granted to the new administrative user. Corrected the aliases in `/etc/sudoers` by replacing (`-`) with underscores (`_`), with (`-`) the alias were not recognised `Cmnd_Alias`
- **Learned:** `Cmnd_Alias` and `User_Alias` names accept only uppercase letters, digits and underscores, so a hyphen makes the alias unrecognised. `%group` in sudoers targets a system group, and commands in a `Cmnd_Alias` are given as full paths. `AllowGroups` in `sshd_config` restricts SSH logins to one group.
- **Reference:** [Proxmox Security Series – Securing Root Access](https://dev.to/sebos/proxmox-security-seriessecuring-root-access-190e)
   
## 2026-11-19 · Session S6: SSH Key Authentication & Banner Configuration
- **Done:** Configured SSH key authentication and login banners on Debian and RedHat 10 VMs. Generated the key pair on my Windows 11 laptop and configured PuTTY sessions with the private key for easier connections.
- **Broken:** Windows 11 did not include `ssh-copy-id`. Additional mistakes involved incorrect public key naming and forgetting to specify the remote username in SSH commands.
- **Fixed:** Used a Powershell command to deploy the public key, corrected the SSH command to specify the Linux user (By default the Windows user was used)
- **Learned:** Windows 11 has no `ssh-copy-id`, so the public key must be added to `~/.ssh/authorized_keys` of the target user with a PowerShell command. `ssh` uses the local Windows username unless I write `user@host`. Setting the server to accept public keys only disables password login. PuTTY stores a private key per saved session.
    
## 2026-11-18 · Session S5: Debian VM Installation, SSH Troubleshooting & Git Fundamentals
- **Done:** Created a new Debian VM on proxmox and installed Debian without a graphical interface, keeping the installation minimal (No GUI). Studied Git fundamentals. Installed `sudo` on Debian and configured sudo privileges for the user account.
- **Broken:** Could not connect to the Debian VM through SSH due to an incorrect password. The noVNC console was not registering Caps Lock correctly, resulting in a password being entered without uppercase letters.
- **Fixed:** Identified the noVNC keyboard issue as the root cause. Aftewards I changed all my password, root and user to add the uppercases (for security purposes).
- **Learned:** A minimal Debian install has no `sudo`, so it must be installed and the user added to the sudo group. The noVNC console can drop Caps Lock, so a rejected password may be a keyboard problem rather than a wrong password. Git tracks files on my machine, GitHub is the service that hosts the repository.
      
## 2026-11-17 · Session S4: Proxmox VM Configuration & Red Hat 10 Installation
- **Done:** Deleted and recreated the virtual machine to improve the `qm create` command, configured CPU cores, network bridge, virtual disk using VirtIO, and set the CPU type to `x86-64-v3`. Installed Red Hat Enterprise Linux 10 and registered the system with Red Hat Subscription Manager. Downloaded the Debian ISO for the next virtual machine (wget)
- **Broken:** The initial VM creation commands I used yesterday, did not configure all the required parameters correctly, making it necessary for me to check on the GUI to get the VM running.
- **Fixed:** Refined the `qm create` command to configure the VM correctly from the start, including CPU type, core count, network bridge, and VirtIO disk
- **Learned:** `qm create` can set everything in one command: CPU type (`--cpu`), cores (`--cores`), VirtIO disk (`--scsi0` with `--scsihw virtio-scsi-pci`) and network bridge (`-net0 virtio,bridge=vmbr0`). `subscription-manager register` links a RHEL system to a Red Hat account. `wget` downloads an ISO straight onto the server.
    
## 2026-11-16 · Session S3: First Proxmox VM – Red Hat 10 Installation
- **Done:** Downloaded the Red Hat Enterprise Linux 10 ISO and attempted to create a first virtual machine on Proxmox VE using the command line
- **Broken:** VM failed to start due to an incompatible CPU configuration. The default KVM CPU type was not compatible with Red Hat 10
- **Fixed:** Debugged the issue through the Proxmox graphical interface and changed the VM CPU type from KVM to x86-64-v3, allowing the VM to boot
- **Learned:** RHEL 10 needs the `x86-64-v3` CPU level, so a VM with the default KVM CPU type fails to boot. A VM can be created from the command line with `qm create`, and the Proxmox web console shows the boot errors.

## 2026-11-15 · Session S2: Proxmox VE Installation
- **Done:** Downloaded Proxmox VE 9.2-1 ISO, created a bootable USB using the dd command on Ubuntu, installed Proxmox VE, configured hostname pve, and accessed the web interface on port 8006
- **Broken:** Rufus on Windows 11 failed to create a working Proxmox installation USB
- **Fixed:** Used an Ubuntu machine and the dd command to write the Proxmox ISO directly to the USB flash drive
- **Learned:** `dd` writes an ISO byte for byte to a device and erases it, so the device name must be checked with `lsblk` first. The Proxmox web interface is served by `pveproxy` on port 8006.

## 2026-11-14 · Session S1: Lab Hardware Inventory & Git Setup
- **Done:** Checked the hardware specifications of each lab machine, created the GitHub repository, created the Hardware Overview directory, and documented the hardware specifications in 01-physical-machines.md
- **Broken:** Nothing
- **Fixed:** Nothing
- **Learned:** `lscpu`, `free -h`, `lsblk`, `df -h` and `lshw` show a machine's CPU, RAM, disks, free space and full hardware list. Repository on GitHub.
