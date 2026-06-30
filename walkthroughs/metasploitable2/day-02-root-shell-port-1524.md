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
