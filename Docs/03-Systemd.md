## Systemd first service 

## Goal
Write my first systemd service - The service write a log every 10s

## Environment
- Debian VM 

## Steps

1. Create 2 files : 
- `touch hello.service` (manages the service itself)
- `touch hello.timer` (manages the re-occurence)

2. hello.service config :
[Unit]
# Description of the service
Description=Log a note every 10 seconds in /var/log/monitor.ssh
# Do not execute if SSH is not already active
Requisite=ssh.service
# Ensure SSH starts before this service
After=ssh.service
[Service]
# Execute the task once, then exit
Type=oneshot
# Write a message to the log file
ExecStart=/bin/sh -c '/usr/bin/echo "SSH is working" >> /var/log/monitor.ssh'

3. hello.timer config :
[Unit]
Description=Execution every 10 seconds
[Timer]
# First execution 10 seconds after boot
OnBootSec=10s
# Repeat 10 seconds after the service was last activated
OnUnitActiveSec=10s
# Allow a timing accuracy of 1 second
AccuracySec=1s
# Specify the service triggered by this timer
Unit=hello.service
[Install]
# Start the timer automatically at boot
WantedBy=timers.target

5. Reload the deamon for systemd to use it `sudo systemctl daemon-reload`
6. Enable and start hello.timer  `sudo systemctl enable --now hello.service`
7. Start the service manually for testing `sudo systemctl start hello.service`
8. Check that the service writes a log entry every 10 seconds `tail -f /var/log/monitor.ssh`

## Choices made
- Repeat any 10s
- Check first if sshd.service is started.

## Check
- 'tail -f /var/log/monitor.ssh'
- `systemctl status <service .timer/.service>`
