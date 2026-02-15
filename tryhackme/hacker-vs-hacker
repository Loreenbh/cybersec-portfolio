# Hacker vs Hacker — TryHackMe

## Objective
The goal was to obtain a shell on the machine and escalate privileges to root.  
An unusual behavior was observed: automated processes limited user sessions, requiring careful planning.

## Reconnaissance
The web service contained an upload feature through a <form> tag, which served as the main entry point.  
Analysis revealed insufficient file validation, allowing further exploitation.

## Exploitation
Uploaded files were stored in a restricted directory.  
Enumeration allowed identification of an executable file, leading to remote code execution as a low-privileged web user.

## System Access
Exploration of the system allowed access to the user’s files and preparation for privilege escalation.

![User access](images/user.png)

## Privilege Escalation
System analysis revealed a cron job executed by root.  
This task relied on a misconfigured PATH variable, allowing execution of scripts in a user-writable directory.
A script was placed in the writable directory to leverage the misconfigured cron task for privilege escalation.

![Privesc](images/privesc.png)

## Root Access
Privilege escalation succeeded, and a root shell was obtained.

![Root](images/root.png)

## Conclusion / Security Lessons
This lab demonstrated a complete exploitation chain:
- Improperly filtered file upload  
- Web-based remote code execution  
- User-level access  
- Discovery and analysis of automated cron processes  
- Privilege escalation through PATH misconfiguration
