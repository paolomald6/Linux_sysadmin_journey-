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

##  download and verification
### wget: download a  from a URL
`wget 'URL'`
- Option `-O`: save the download with a specific name
- Example: `wget -O rhel-10.3-x86_64-dvd.iso 'URL'` downloads the ISO with a custom name

### sha256sum: calculate or verify SHA-256 checksums
`sha256sum .iso`

## Proxmox
### pveversion: installed Proxmox VE version
`pveversion`
### qm create: create a virtual machine
`qm create <vmid> --name "RHEL10-1" --memory 4096 --cores 2 --arch x86_64 --scsihw virtio-scsi-pci --scsi0 local-lvm:25 -net0 virtio,bridge=vmbr0 -cdrom local:iso/rhel-10.3-x86_64-dvd.iso`
- Option `--scsihw virtio-scsi-pci`: SCSI controller type (virtio, faster in a VM)
- Option `--scsi0 local-lvm:25`: 25 GB disk on the SCSI bus, stored on local-lvm
- Option `-net0 virtio,bridge=vmbr0`: network card (virtio model) connected to the bridge vmbr0
- Option `--cores 2`: number of CPU cores
- Option `--arch x86_64`: CPU architecture of the VM
### qm config: display a virtual machine configuration
`qm config <vmid>`
### qm set: change the configuration of an existing virtual machine
`qm set <vmid> --cores 2`
- Option `--cores`: number of CPU cores
- Option `--agent 1`: enable the QEMU guest agent for the VM (the VM must be fully stopped and started again to apply it)
- Example: `qm set <vmid> --agent 1`
### qm list: list virtual machines
`qm list`
### qm start: start a virtual machine
`qm start <vmid>`
### cat: display a virtual machine configuration 
`cat /etc/pve/qemu-server/<vmid>.conf`
### qm destroy: delete a virtual machine
`qm destroy 300 --purge`
- Option `--purge`: also remove associated configuration and references
### qm agent: send a command to the QEMU guest agent inside a VM
`qm agent <vmid> <command>`
- Example: `qm agent <vmid> network-get-interfaces` lists the VM's network interfaces and IP addresses (the agent must be running in the VM)

## Red Hat subscription
### subscription-manager status: show the subscription status of the system
`sudo subscription-manager status`
### subscription-manager register: register the system with a Red Hat account
`sudo subscription-manager register`

## Users and permissions
### adduser: create a user, or add an existing user to a group
`adduser <username>`
- Option `--home`: set the home directory path
- Example: `adduser --home /home/<username> <username>` creates the user with a custom home directory
- Example: `adduser <username> <groupname>` adds the existing user to a group
### passwd: change a password, or lock/unlock an account
`passwd username`
- Option `-l`: lock the account (password login disabled)
- Option `-u`: unlock the account
- Option `-S`: show the status (L = locked, P = usable password)
- Example: `sudo passwd -l root` locks the root password
### addgroup: create a group
`sudo addgroup <groupname>`
### gpasswd: manage group members
`sudo gpasswd --add <username> <groupname>`
- Option `--add` (or `-a`): add a user to a group
- Option `-M`: set the full member list, replacing the current members
- Example: `sudo gpasswd -M <usera>,<userb> <groupname>`
### deluser: delete a user
`sudo deluser <username>`
- Option `--remove-home`: also delete the user's home directory (s are lost)
- Example: `sudo deluser --remove-home <username>`
### delgroup: delete a group
`sudo delgroup <groupname>`
### chown: change the owner and group of a 
`sudo chown <user>:<group> <>`
- Example: `sudo chown <username>:<groupname> .ssh/authorized_keys` gives the  to this user and group

## Configuration s
### /etc/sudoers: define who can run which commands with sudo
`sudo visudo`
- `User_Alias NAME = %group`: alias for a group of users (`%` = system group)
- `Cmnd_Alias NAME = cmd1, cmd2`: alias for a list of allowed commands (full paths)
- Example: `SYSTEM_ADMIN ALL=(ALL) NOPASSWD: UPDATE_CMDS, LOG_CMDS, RVIM` lets the group run these command lists without a password

### /etc/ssh/sshd_config: SSH server configuration
`sudo nano /etc/ssh/sshd_config`
- Option `AllowGroups ssh-users`: only members of this group can log in over SSH
## SSH
### ssh-keygen: generate an SSH key pair
`ssh-keygen`
- Option `-t`: key type (ed25519 is the modern default)
_ Option `-C`: Add a comment to the public key, to identify it
- Example: `ssh-keygen -C "Debian" -t ed25519` creates a private key and a `.pub` 
public key in `~/.ssh/`

### ssh-copy-id: install your public key on a remote server
`ssh-copy-id <username>@<server-ip>`
- Option `-i`: choose which public key to copy
- Example: `ssh-copy-id -i ~/.ssh/id_ed25519.pub <username>@<server-ip>`

## Git
### git config: set your Git identity
`git config user.name "<your-name>"`
`git config user.email "<your-email>"`
- Option `--global`: apply to all repositories instead of the current one
### git status: show modified files and what is staged
`git status`
### git diff: show changes not yet staged
`git diff`
- Option `--staged`: show changes already staged for the next commit
### git add: stage files for the next commit
`git add <file>`
- Example: `git add .` stages all changes in the current folder
### git commit: save the staged changes with a message
`git commit -m "<message>"`
- Option `-m`: commit message
### git fetch: download remote changes without merging them
`git fetch`
### git pull: download remote changes and merge them
`git pull`
### git push: send your local commits to the remote repository
`git push`

## Files
### cp: copy files or folders
`cp <source> <destination>`
- Option `-p`: preserve permissions, timestamps and (as root) the owner
- Example: `sudo cp -p /home/<username>/.ssh/authorized_keys /home/<newuser>/.ssh/` copies the SSH authorized keys to another user
### find: search for files and folders
`find <path> -name "<filename>"`
- Option `-name`: match the file name (case-sensitive, use `-iname` to ignore case)
- Example: `find /etc -name "sshd_config"` looks for this file under /etc

## systemd
### /etc/systemd/system/: custom service and timer files
`sudo nano /etc/systemd/system/hello.service`
`sudo nano /etc/systemd/system/hello.timer`
- `.service`: defines what to run
- `.timer`: defines when to run it
### systemctl daemon-reload: reload systemd after creating or editing a unit file
`sudo systemctl daemon-reload`
### systemctl enable: start a unit automatically at boot
`sudo systemctl enable <unit>`
- Option `--now`: also start it immediately
- Example: `sudo systemctl enable --now hello.timer`
### systemctl disable: stop starting a unit at boot
`sudo systemctl disable <unit>`
- Option `--now`: also stop it immediately
- Example: `sudo systemctl disable --now ssh-monitor.timer`
### systemctl get-default: show the default target (what the system boots into)
`systemctl get-default`
- Example: returns `graphical.target` (with desktop) or `multi-user.target` (text mode only)
### systemctl set-default: change the default target for the next boots
`sudo systemctl set-default <target>`
- Example: `sudo systemctl set-default multi-user.target` boots in text mode, without the graphical interface
### systemctl isolate: switch to a target right now
`sudo systemctl isolate <target>`
- Example: `sudo systemctl isolate multi-user.target` stops the graphical interface immediately, without rebooting
### systemctl status: show the state of a unit and its latest logs
`systemctl status <unit>`
- Example: `systemctl status display-manager` checks if the graphical login manager is running
### systemctl list-units: list the loaded units
`systemctl list-units`
- Option `--type`: filter by unit type (service, timer, target...)
- Example: `sudo systemctl list-units --type target` lists the active targets
### systemctl list-dependencies: show what a unit depends on
`systemctl list-dependencies <unit>`
- Example: `sudo systemctl list-dependencies graphical.target` shows everything started by the graphical target

##Logs
### tail: show the end of a file
`tail <file>`
- Option `-f`: follow, keep displaying new lines as they are added
- Example: `tail -f /var/log/auth.log` watches SSH login attempts live
### dmesg: kernel messages (hardware detection, drivers, boot errors)
`sudo dmesg`
