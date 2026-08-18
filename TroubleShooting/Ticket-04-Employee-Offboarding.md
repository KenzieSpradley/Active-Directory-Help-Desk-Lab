Ticket #04 - Employee Offboarding

User:
Sam Miller - Sales

Request:
HR submitted an approved request to offboard Sam Miller following the end of employment.

Environment:
Active Directory Domain: techlab.local
Domain Controller: TECHLAB-DC01
Workstation: TECHLAB-PC01

Initial Review:
Verified the employee's Active Directory account and existing security group memberships.

The user was a member of:
- Domain Users
- GG-Sales
- GG-HR

Actions Performed:
- Disabled the Active Directory user account.
- Removed GG-Sales membership.
- Removed temporary GG-HR access.
- Retained the user's standard Domain Users primary group.
- Verified account status using PowerShell.

Verification:
Get-ADUser reported Enabled=False.

Get-ADPrincipalGroupMembership confirmed that the user's departmental security-group access had been removed.

Attempted authentication from TECHLAB-PC01 and confirmed that the disabled account could no longer log in.

Tools Used:
- Active Directory Users and Computers
- PowerShell
- Windows 11