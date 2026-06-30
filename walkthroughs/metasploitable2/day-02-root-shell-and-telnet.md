# Day 02 - Root Shell via Port 1524
**Date:** 2026-06-30

## What I Did
- Rebooted Metasploitable 2 to clean state
- Connected directly via netcat: nc 192.168.100.10 1524
- Got immediate root@metasploitable shell, zero authentication
- Confirmed with whoami and id

## Key Lesson
Port 1524 is a misconfiguration, not a vulnerability — a root shell
listening with no authentication at all. No exploit code needed,
just a port scanner and netcat.

Vulnerability = flaw that requires exploitation (yesterday's vsftpd backdoor)
Misconfiguration = open door left by mistake (today's port 1524)

## Milestone
First successful root shell achieved.

## Port 23 - Telnet Default Credentials
- Confirmed port 23 open via nmap
- Connected directly: telnet 192.168.100.10
- Logged in using default Metasploitable credentials (msfadmin/msfadmin)
- Got shell access immediately, no exploit required

### Key Lesson
Like port 1524, this is a misconfiguration rather than a vulnerability — 
default credentials left unchanged on a service that transmits everything 
in plaintext. Anyone sniffing the network during login could read 
credentials directly off the wire.

### If Default Creds Hadn't Worked
- Try other common default pairs (root/root, admin/admin, root/toor)
- Brute force with Hydra: hydra -l msfadmin -P /usr/share/wordlists/rockyou.txt telnet://192.168.100.10
- Username enumeration first if no valid username is known
- Check telnetd version against known CVEs if brute forcing fails

### Detection Note
Encryption (SSH) protects credentials from network sniffing but doesn't 
hide failed login attempts from server-side logs. Brute forcing generates 
log noise regardless of encryption — caught via auth logs and 
rate-limiting/fail2ban, not just packet inspection.

## Progress So Far
- Port 1524: root shell, no auth
- Port 21 (vsftpd 2.3.4): backdoor identified, session inconsistent via Metasploit, revisit needed
- Port 23 (telnet): default creds, shell access

## Next Session
- NFS, Samba, or ProFTPD (port 2121)
- Revisit clean vsftpd backdoor exploitation via Metasploit
