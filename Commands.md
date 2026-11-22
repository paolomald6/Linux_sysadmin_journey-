# Linux commands

## System information
### lscpu: CPU model, cores, virtualization
`lscpu`
- Example: `lscpu | grep -i virtualization` shows VT-x or AMD-V

### free: RAM usage in readable units
`free -h`

### lsblk: disks and partitions
`lsblk`

### df: free space per mounted filesystem
`df -h`
- Option `-T`: type (ext4, tmpfs...)
- Example: `df -hT` shows the filesystem type next to each line
  
### uname: OS and kernel information
`uname`
- Option `-a`: all info (kernel version, hostname, architecture)
  
### lshw: full hardware details (CPU, RAM, disks, network...)
`sudo lshw`
- Option `--short`: one line per device
- Option `-class`: show only one category (memory, disk, network...)
- Example: `sudo lshw -class memory` shows RAM type (DDR4, DDR5...)

## Disk and USB
### dd: copy a file to a disk, e.g. write an ISO to a USB drive
`sudo dd if=./file.iso of=/dev/XYZ`
- Option `if=`: input file
- Option `of=`: output target. **It erases everything on the target**, check the device with `lsblk` first
- Option `bs=1M`: block size, copies 1 MB at a time (faster)
- Option `conv=fdatasync`: flushes data to the device before exiting, so the USB is safe to remove
- Example: `sudo dd bs=1M conv=fdatasync if=./proxmox-ve_*.iso of=/dev/XYZ` writes the Proxmox installer to a USB drive

## Network
### ss: open ports and network connections
`ss -tlnp`
- Option `-t`: TCP only
- Option `-l`: listening sockets only
- Option `-n`: numeric ports (8006, not names)
- Option `-p`: show the process using each port
- Example: `ss -tlnp | grep 8006` checks that the Proxmox web interface is listening

## File download and verification
### wget: download a file from a URL
`wget 'URL'`
- Option `-O`: save the download with a specific filename
- Example: `wget -O rhel-10.3-x86_64-dvd.iso 'URL'` downloads the ISO with a custom filename

### sha256sum: calculate or verify SHA-256 checksums
`sha256sum file.iso`

## Proxmox
### pveversion: installed Proxmox VE version
`pveversion`
### qm create: create a virtual machine
`qm create 300 --name "RHEL10-1" --memory 4096 -ide0 local-lvm:4 -net0 e1000 -cdrom local:iso/rhel-10.3-x86_64-dvd.iso`
- Option `--scsihw virtio-scsi-pci`: SCSI controller type (virtio, faster in a VM)
- Option `--scsi0 local-lvm:25`: 25 GB disk on the SCSI bus, stored on local-lvm
- Option `-net0 virtio,bridge=vmbr0`: network card (virtio model) connected to the bridge vmbr0
- Option `--cores 2`: number of CPU cores
- Option `--arch x86_64`: CPU architecture of the VM
### qm config: display a virtual machine configuration
`qm config 300`
### qm list: list virtual machines
`qm list`
### qm start: start a virtual machine
`qm start 300`
### cat: display a virtual machine configuration file
`cat /etc/pve/qemu-server/300.conf`
### qm destroy: delete a virtual machine
`qm destroy 300 --purge`
- Option `--purge`: also remove associated configuration and references

## Red Hat subscription
### subscription-manager status: show the subscription status of the system
`sudo subscription-manager status`
### subscription-manager register: register the system with a Red Hat account
`sudo subscription-manager register`

## Users and permissions
### adduser: create a user, or add an existing user to a group
`adduser username`
- Example: `adduser basile sudo` adds the existing user basile to the sudo group
- Option `--home`: set the home directory path
- Example: `adduser --home /home/basile basile` creates the user basile with /home/basile as home directory
### passwd: change a password, or lock/unlock an account
`passwd username`
- Option `-l`: lock the account (password login disabled)
- Option `-u`: unlock the account
- Option `-S`: show the status (L = locked, P = usable password)
- Example: `sudo passwd -l root` locks the root password
### addgroup: create a group
`sudo addgroup ssh-users`
### gpasswd: manage group members
`sudo gpasswd --add basile ssh-users`
- Option `--add`: add a user to a group

## Configuration files
### /etc/sudoers: define who can run which commands with sudo
`sudo visudo`
- `User_Alias NAME = %group`: alias for a group of users (`%` = system group)
- `Cmnd_Alias NAME = cmd1, cmd2`: alias for a list of allowed commands (full paths)
- Example: `SYSTEM_ADMIN ALL=(ALL) NOPASSWD: UPDATE_CMDS, LOG_CMDS, RVIM` lets the group run these command lists without a password

### /etc/ssh/sshd_config: SSH server configuration
`sudo nano /etc/ssh/sshd_config`
- Option `AllowGroups ssh-users`: only members of this group can log in over SSH
