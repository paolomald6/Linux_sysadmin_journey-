# SSH Configuration – Public Key Authentication

## Goal
Secure SSH access to the Proxmox virtual machines using public key authentication instead of passwords.

## Environment
- Client: Windows 11 laptop
- SSH client: Windows OpenSSH and PuTTY
- SSH Server: Proxmox Linux VMs

## Steps

1. Generated a public/private SSH key pair on Windows 11 using `ssh-keygen`.
2. Copied the public key to the Linux server and added it to `~/.ssh/authorized_keys`. 
- Command on Windows `type $env:USERPROFILE\.ssh\id_rsa.pub | ssh USERNAME@IP_ADDRESS "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"`  
- Command on Linux  `ssh-cpy-id` but 
4. Connected to the server using SSH public key authentication.
5. Generated a PuTTY-compatible private key (`.ppk`) using PuTTYgen.
6. Configured two PuTTY sessions with the corresponding username and private key file.
7. Saved both PuTTY sessions for easy future connections.

## Choices made
- Authentication method: SSH public key only.
- Key generation: Windows OpenSSH (`ssh-keygen`).
- PuTTY authentication: Private key file (`.ppk`).

## Security configuration
- Disabled SSH password authentication and root login on VM's.
- Enabled public key authentication in `/etc/ssh/sshd_config`.

## Check
- `ssh USERNAME@IP_ADDRESS` connects using the private key.
- PuTTY connects using the saved session.
- Password authentication and root are disabled on the SSH server.
