# Day 05 - ProFTPD 1.3.1 Exploitation Attempt
**Date:** 2026-08-05

## Recon
- nmap -sV -p 2121 192.168.100.10
- Found: ProFTPD 1.3.1 running on port 2121

## Exploitation Attempts

### sreplace Buffer Overflow (proftp_sreplace)
- Module: exploit/linux/ftp/proftp_sreplace
- Version range: 1.2 - 1.3.0
- Result: No matching target — 1.3.1 falls just outside coverage

### mod_copy Command Execution (proftpd_modcopy_exec)
- Module: exploit/unix/ftp/proftpd_modcopy_exec
- Connected to FTP successfully as msfadmin
- Tested mod_copy manually: SITE CPFR returned 500
- mod_copy not enabled on this version
- Result: Exploit aborted

### 1.3.3c Backdoor
- Not applicable — different version entirely

## Key Lesson
ProFTPD 1.3.1 sits in a gap between available Metasploit modules.
Not every service has a clean exploit — this is realistic. In a real
engagement this is documented and other attack paths are prioritized.
Root was already achieved through four other services on this machine,
making further time on ProFTPD unnecessary.

Real attackers don't brute force one path — they map all attack
surfaces, exploit the easiest ones, and pivot using gained access.

## Metasploitable 2 - Full Summary
All services from initial nmap scan addressed:
- Port 1524: root shell, no auth required
- Port 23 (telnet): default credentials msfadmin:msfadmin
- Port 21 (vsftpd 2.3.4): backdoor command execution, root shell
- NFS: root filesystem mounted, shadow file read, 6/7 hashes cracked
- Samba 3.0.20 CVE-2007-2447: command injection, root shell
- Port 5432 (PostgreSQL): service shell → privesc → root
- Port 2121 (ProFTPD 1.3.1): version gap, no suitable exploit found

## Next Steps
- OWASP Juice Shop (web application attacks)
- VulnHub machines (varied targets)
