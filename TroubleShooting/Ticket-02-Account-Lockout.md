Ticket #02 - Account Lockout

User:
Chris Davis - Help Desk

Issue:
User was unable to authenticate after multiple incorrect password attempts.

Environment:
Windows 11
Active Directory Domain: techlab.local
Domain Controller: TECHLAB-DC01
Workstation: TECHLAB-PC01

Investigation:
Reviewed the user's account in Active Directory Users and Computers and confirmed the account was locked.

Root Cause:
The domain Account Lockout Policy was configured with a threshold of five invalid logon attempts. The user exceeded this threshold.

Resolution:
Unlocked the user's Active Directory account without resetting the password.

Verification:
User successfully authenticated to TECHLAB-PC01 using the existing password. Verified the domain account with WHOAMI and confirmed LockedOut=False using PowerShell.

Tools Used:
- Active Directory Users and Computers
- Group Policy Management
- PowerShell
- Command Prompt