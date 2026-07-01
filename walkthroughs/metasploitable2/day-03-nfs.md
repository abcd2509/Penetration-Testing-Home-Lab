# Day 03 - NFS Misconfiguration & Samba Exploitation
**Date:** 2026-07-01

## NFS
### Recon
- showmount -e 192.168.100.10
- Found: / * (entire filesystem exposed to any host)

### Exploitation
- sudo mount -t nfs 192.168.100.10:/ /tmp/nfs
- Read /etc/shadow directly from mounted filesystem
- Cracked 6/7 password hashes with John the Ripper

### Cracked Credentials
- user:user
- service:service
- postgres:postgres
- msfadmin:msfadmin
- klog:123456789
- sys:batman

### Root Cause
Human error — NFS exported to * with no access control.
No exploit required, just a mount command.

## Samba
### Recon
- smbclient -L 192.168.100.10 --no-pass
- Anonymous login successful
- Found exposed shares: tmp, opt, print$
- Version: Samba 3.0.20-Debian

### Exploitation
- Module: exploit/multi/samba/usermap_script
- CVE: 2007-2447
- Command injection via username field in Samba's
  username map script feature
- Got root shell: uid=0(root) gid=0(root)

### Root Cause
Two separate issues:
1. Misconfiguration — anonymous login enabled, no authentication required
2. Vulnerability — unpatched 19 year old CVE in Samba 3.0.20,
   requires a software patch not just a config change

## Key Lesson
Misconfigurations are fixed with config changes.
Vulnerabilities require vendor patches or mitigation.
Both can give an attacker root.

## Progress So Far
- Port 1524: root shell, no auth (Day 02)
- Port 23 (telnet): default creds (Day 02)
- NFS: mounted root filesystem, cracked shadow file (Day 03)
- Samba 3.0.20: command injection, root shell (Day 03)

## Next Session
- ProFTPD port 2121
- Revisit vsftpd backdoor cleanly
- PostgreSQL command execution via cracked credentials
