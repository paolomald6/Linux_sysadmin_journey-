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

### lshw: full hardware details (CPU, RAM, disks, network...)
`sudo lshw`
- Option `--short`: one line per device
- Option `-class`: show only one category (memory, disk, network...)
- Example: `sudo lshw -class memory` shows RAM type (DDR4, DDR5...)
