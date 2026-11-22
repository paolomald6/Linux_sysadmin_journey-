# Linux SysAdmin Journey

Documenting my path to Linux system administrator.

**Start date:** 14 November 2026

## My lab

| # | Machine | CPU | RAM | Storage | OS | Role |
|---|---|---|---|---|---|---|
| 01 | Raspberry Pi 4 Model B | Broadcom BCM2711 (quad-core ARM Cortex-A72) | 4 GB | 32 GB microSD | Not installed yet | Network services (planned) |
| 02 | HP workstation | Intel Core i5-9500 (6 cores) | 16 GB | 250 GB | Proxmox VE 9.2 | Hypervisor |
| 03 | Lenovo ThinkPad P15s | Intel Core i7-1165G7 (4 cores / 8 threads) | 32 GB | 500 GB | Windows 11 | Admin workstation |

### Virtual machines (on Proxmox)

| OS | Purpose |
|---|---|
| Red Hat Enterprise Linux 10 (minimal) | RHCSA practice |
| Debian (minimal, no GUI) | Services and Git workstation |
| Ubuntu | Linux Upskill Challenge |

## What I have done so far

- Installed Proxmox VE (bootable USB written with `dd`)
- Created VMs from the command line with `qm create` (CPU type `x86-64-v3` for RHEL 10)
- Set up SSH key authentication and login banners
- Hardened SSH on Proxmox: dedicated admin user, root login disabled, `AllowGroups`
- Wrote restricted `sudo` rules with `Cmnd_Alias` in `/etc/sudoers`
  
## Repository map

- `learning-journal.md`: what I did each session, what broke, how I fixed it
- `Commands/`: command cheat sheet
- `Docs/`: setup guides (Proxmox VE installation)

## Current status
Installation, SSH and first hardening done. Next: systemd and logs, Bash scripting, storage (LVM).
