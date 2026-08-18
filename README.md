# Active Directory Help Desk Lab

## Overview

This project documents the creation of a virtual Windows Active Directory environment designed to simulate common tasks performed by an entry-level IT Help Desk technician.

I built a Windows Server Domain Controller and Windows 11 client workstation using VirtualBox. After configuring Active Directory Domain Services, DNS, users, Organizational Units, security groups, and Group Policy, I joined the Windows 11 workstation to the domain and used the environment to complete several simulated help desk tickets.

The troubleshooting scenarios included:

- Password resets
- Account lockouts
- Network share permission issues
- Security group access
- Group Policy drive mapping
- Employee offboarding

The goal of this project was to gain hands-on experience administering and troubleshooting a Windows domain environment rather than only studying the concepts.

---

# Lab Environment

| Component | Configuration |
|---|---|
| Domain | `techlab.local` |
| Domain Controller | `TECHLAB-DC01` |
| Domain Controller IP | `192.168.56.10` |
| Client Workstation | `TECHLAB-PC01` |
| Client IP | `192.168.56.20` |
| Client DNS | `192.168.56.10` |
| Server OS | Windows Server |
| Client OS | Windows 11 |
| Virtualization | Oracle VirtualBox |

---

# Technologies & Skills

- Active Directory Domain Services (AD DS)
- Active Directory Users and Computers (ADUC)
- Windows Server
- Windows 11
- Oracle VirtualBox
- DNS
- IPv4 networking
- Organizational Units (OUs)
- User account administration
- Security groups
- Group Policy
- Group Policy Preferences
- Password policies
- Account lockout policies
- SMB network shares
- NTFS permissions
- Domain joining
- Domain authentication
- Access troubleshooting
- User lifecycle management
- Employee offboarding

---

# 1. Windows Server Setup

I created a Windows Server virtual machine in VirtualBox to serve as the Domain Controller for the lab.

![Windows Server Virtual Machine](Screenshots/01-Setup/virtualbox-windows-server-vm.png)

After installing Windows Server, I configured the server and renamed it `TECHLAB-DC01`.

![Windows Server Dashboard](Screenshots/01-Setup/windows-server-dashboard.png)

The Domain Controller was assigned the static IPv4 address:

`192.168.56.10`

Using a static address ensures domain clients can consistently locate the Domain Controller and its DNS service.

![Domain Controller Static IP](Screenshots/01-Setup/static-ip.png)

Active Directory Domain Services was installed on the server.

![Active Directory Domain Services Installed](Screenshots/01-Setup/ad-ds-installed.png)

The server successfully passed the prerequisite check before being promoted to a Domain Controller.

![Domain Controller Prerequisite Check](Screenshots/01-Setup/domain-controller-prerequisite-check.png)

---

# 2. Active Directory Configuration

I created the Active Directory domain:

`techlab.local`

The environment was organized using Organizational Units for users, computers, departments, security groups, and disabled accounts.

## Organizational Unit Structure

The domain structure was designed to separate users based on their department and administrative purpose.

![Active Directory OU Structure](Screenshots/02-Active-Directory/ou-structure.png)

Departments included:

- Help Desk
- Human Resources
- Information Technology
- Sales

Separate OUs were also used for workstations, security groups, and disabled users.

## Security Groups

I created department-based security groups so access could be managed through group membership instead of assigning permissions directly to individual users.

Groups included:

- `GG-HelpDesk`
- `GG-HR`
- `GG-IT`
- `GG-Sales`

![Active Directory Security Groups](Screenshots/02-Active-Directory/gg-groups.png)

This structure allowed permissions to be assigned according to a user's role or department.

---

# 3. Domain Password Policy

I configured a domain-wide password policy through Group Policy.

The policy included requirements for password length, complexity, password history, and expiration.

![Domain Password Policy](Screenshots/02-Active-Directory/domain-password-policy.png)

After making Group Policy changes, I forced a policy refresh using:

`gpupdate /force`

![Group Policy Update](Screenshots/02-Active-Directory/gpupdate-force.png)

---

# 4. Windows 11 Domain Workstation

I created a separate Windows 11 virtual machine named:

`TECHLAB-PC01`

![Windows 11 Workstation VM](Screenshots/03-Domain-Join/workstation-vm.png)

The workstation was configured with:

- IP address: `192.168.56.20`
- Subnet mask: `255.255.255.0`
- DNS server: `192.168.56.10`

The DNS configuration is important because the workstation must use the Active Directory DNS server to locate domain services.

![TECHLAB-PC01 Network Configuration](Screenshots/03-Domain-Join/static-ip.png)

---

# 5. Joining Windows 11 to Active Directory

After verifying network and DNS connectivity between the workstation and Domain Controller, I configured `TECHLAB-PC01` to join:

`techlab.local`

![TECHLAB-PC01 Domain Join Configuration](Screenshots/03-Domain-Join/domain-join.png)

The workstation successfully joined the Active Directory domain.

![Successful Domain Join](Screenshots/03-Domain-Join/successful-domain-join.png)

After the domain join, `TECHLAB-PC01` appeared as a computer object inside Active Directory.

I then organized the workstation into the appropriate workstation OU.

![Active Directory Workstation](Screenshots/03-Domain-Join/ad-workstation.png)

This confirmed communication between the Windows 11 client and the Active Directory environment.

---

# 6. Help Desk Ticket #1 — Forgotten Password

## Issue

An HR employee was unable to log into the domain because they had forgotten their password.

## Investigation

I located the user's account using Active Directory Users and Computers and verified the account before making changes.

## Resolution

The user's password was reset through Active Directory.

The user was also required to change the temporary password during their next login.

![Active Directory Password Reset](Screenshots/04-Password-Reset/password-reset.png)

When the employee attempted to sign in, Windows required the temporary password to be changed.

![User Password Change Prompt](Screenshots/04-Password-Reset/user-reset-prompt.png)

## Verification

After changing the password, the employee successfully authenticated to the domain.

![Successful Password Reset](Screenshots/04-Password-Reset/successful-reset.png)

### Skills Demonstrated

- Active Directory user administration
- Password resets
- Temporary password management
- Domain authentication
- Resolution verification

[View Ticket #1 Documentation](Troubleshooting/Ticket-01-Password-Reset.md)

---

# 7. Help Desk Ticket #2 — Account Lockout

## Issue

A user entered an incorrect password multiple times and became locked out of their Active Directory account.

## Account Lockout Policy

I configured the domain to lock user accounts after five unsuccessful authentication attempts.

![Domain Account Lockout Policy](Screenshots/05-Account-Lockout/domain-account-lockout-policy.png)

## Investigation

After intentionally triggering the lockout for the lab scenario, I located the user's account in Active Directory and confirmed that it was locked.

![Locked Active Directory User](Screenshots/05-Account-Lockout/locked-user-account.png)

## Resolution

Because the user knew their correct password, resetting the password was unnecessary.

Instead, I unlocked the existing account through Active Directory Users and Computers.

![Active Directory Account Unlock](Screenshots/05-Account-Lockout/account-unlock.png)

## Verification

The user successfully authenticated after the account was unlocked.

![Successful Login After Unlock](Screenshots/05-Account-Lockout/login-after-unlock.png)

### Skills Demonstrated

- Account lockout troubleshooting
- Active Directory account management
- Group Policy
- Authentication troubleshooting
- Troubleshooting before making unnecessary changes

[View Ticket #2 Documentation](Troubleshooting/Ticket-02-Account-Lockout.md)

---

# 8. Help Desk Ticket #3 — Network Share Access Denied

## Scenario

I created a departmental HR network share on the Domain Controller to simulate a company file server.

The share was available at:

`\\TECHLAB-DC01\HR`

![HR Shared Folder](Screenshots/06-File-Permissions/shared-folder.png)

## NTFS Permissions

Access to the HR folder was controlled through the `GG-HR` Active Directory security group.

Rather than assigning permissions directly to individual employees, access was granted through security-group membership.

![HR Folder Permissions](Screenshots/06-File-Permissions/hr-folder-properties.png)

## Issue

A Sales employee attempted to access:

`\\TECHLAB-DC01\HR`

The employee received an Access Denied message.

![HR Folder Access Denied](Screenshots/06-File-Permissions/folder-access-denied.png)

## Investigation

I reviewed the employee's Active Directory properties and security-group memberships.

The employee belonged to the Sales security group but did not originally belong to `GG-HR`.

![User Group Membership Before Fix](Screenshots/06-File-Permissions/user-properties-before.png)

This explained why the user could reach the server but could not access the protected HR resource.

## Resolution

For the simulated ticket, I assumed that HR management approved temporary access for a cross-department project.

Instead of modifying the NTFS permissions for the individual employee, I added the employee to the existing `GG-HR` security group.

![Updated Group Membership](Screenshots/06-File-Permissions/updated-group-membership.png)

The employee signed out and back into Windows so a new security token would be generated containing the updated group membership.

## Verification

The employee was then able to successfully access the HR network share.

![HR Folder Access Restored](Screenshots/06-File-Permissions/access-restored.png)

### Skills Demonstrated

- SMB network shares
- NTFS permissions
- Active Directory security groups
- Group-based access control
- Permission troubleshooting
- Windows security tokens
- Least-privilege concepts
- Access verification

[View Ticket #3 Documentation](Troubleshooting/Ticket-03-Share-Permissions.md)

---

# 9. Group Policy — Automatic Network Drive Mapping

After creating the HR network share, I used Group Policy Preferences to automatically map the share for HR employees.

I created:

`GPO-HR-DriveMap`

and linked the policy to the HR Organizational Unit.

![HR Drive Mapping GPO](Screenshots/07-Group-Policy/hr-gpo.png)

The mapped drive was configured with:

- **Location:** `\\TECHLAB-DC01\HR`
- **Drive Letter:** `H:`
- **Label:** `HR Department`

![HR Drive Map Configuration](Screenshots/07-Group-Policy/drive-config.png)

After refreshing Group Policy and signing in as an HR employee, the policy automatically provided access to the departmental drive.

I verified that the policy successfully applied to the user.

![Group Policy Verification](Screenshots/07-Group-Policy/gpo-verification.png)

### Skills Demonstrated

- Group Policy Management
- Group Policy Preferences
- Network drive mapping
- OU-based policy deployment
- SMB shares
- GPO verification

---

# 10. Help Desk Ticket #4 — Employee Offboarding

## Request

For the final help desk scenario, I simulated an approved employee offboarding request.

Before making changes, I reviewed the employee's existing Active Directory group memberships.

![Group Membership Before Offboarding](Screenshots/08-Offboarding/group-memberships-before.png)

## Account Disablement

The employee's Active Directory account was disabled to prevent further authentication.

![Disabled Active Directory Account](Screenshots/08-Offboarding/account-disabled.png)

## Access Removal

Departmental and temporary security-group memberships were removed from the account.

![Group Membership After Offboarding](Screenshots/08-Offboarding/group-memberships-after.png)

This removed the employee's access to resources that were controlled through those security groups.

## Authentication Verification

I attempted to authenticate to the domain workstation using the disabled account.

Windows rejected the login, confirming that the account could no longer authenticate.

![Disabled Account Sign-In Blocked](Screenshots/08-Offboarding/sign-in-blocked.png)

## Account Organization

Finally, I moved the disabled account from the Sales OU into a dedicated:

`Disabled Users`

Organizational Unit.

![User Moved to Disabled Users OU](Screenshots/08-Offboarding/user-moved-to-disabled-users.png)

### Skills Demonstrated

- Employee offboarding
- Active Directory account disabling
- Security-group management
- Access removal
- User lifecycle management
- Authentication verification
- Active Directory organization

[View Ticket #4 Documentation](Troubleshooting/Ticket-04-Employee-Offboarding.md)

---

# Troubleshooting Methodology

Throughout the lab, I followed a basic troubleshooting process:

1. Identify the user's reported problem.
2. Gather information about the affected account or resource.
3. Verify network, account, group, or policy configuration.
4. Identify the root cause.
5. Make the appropriate change.
6. Test the solution from the user's workstation.
7. Verify that the original issue was resolved.
8. Document the troubleshooting process.

One of the main lessons from the project was to verify the cause of an issue before making changes. For example, a locked account does not necessarily require a password reset, and an Access Denied message does not necessarily indicate a network connectivity problem.

---

# What I Learned

This project gave me hands-on experience with the relationship between Windows clients, Active Directory, DNS, Group Policy, security groups, and file permissions.

Some of the most important concepts I practiced were:

- Creating and organizing Active Directory users and computers
- Managing users through security groups
- Joining Windows workstations to a domain
- Understanding the importance of DNS in Active Directory
- Resetting passwords and troubleshooting account lockouts
- Creating and securing network file shares
- Troubleshooting Access Denied errors
- Using Group Policy to deploy user settings
- Verifying changes instead of assuming they worked
- Following an employee offboarding process
- Documenting troubleshooting steps and resolutions

---

# Project Summary

This lab simulated a small business Windows domain environment consisting of a Domain Controller and Windows 11 workstation.

I configured the infrastructure and then used it to simulate common help desk responsibilities including:

- User account administration
- Password resets
- Account unlocks
- Group membership management
- File-share permissions
- Access troubleshooting
- Group Policy deployment
- Domain workstation administration
- Employee offboarding

The project helped me move beyond studying Active Directory concepts by giving me hands-on experience configuring, breaking, troubleshooting, and verifying a working Windows domain environment.

---

## Disclaimer

This environment was created as a home lab for educational and portfolio purposes.

All users, organizations, domain names, tickets, and business scenarios shown in this repository are fictional.
