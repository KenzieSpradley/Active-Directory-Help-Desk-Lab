Ticket #01 - Forgotten Password

User:
Jamie Brown - HR

Issue:
User reported being unable to log in after forgetting their domain password.

Environment:
Windows 11
Active Directory Domain: techlab.local
Domain Controller: TECHLAB-DC01

Troubleshooting:
Verified the user's Active Directory account and initiated a password reset through Active Directory Users and Computers.

Resolution:
Assigned a temporary password and enabled "User must change password at next logon."

Verification:
User successfully authenticated to TECHLAB-PC01, changed the temporary password, and logged into the domain.

Tools Used:
Active Directory Users and Computers
Windows 11
Command Prompt