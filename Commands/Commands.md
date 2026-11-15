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

## Proxmox
### pveversion: installed Proxmox VE version
`pveversion`
