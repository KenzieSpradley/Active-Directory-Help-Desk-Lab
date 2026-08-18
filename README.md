# Active Directory Help Desk Lab

## Overview

This project documents the creation of a virtual Windows Active Directory environment designed to simulate common tasks performed by an entry-level IT Help Desk technician.

I built a Windows Server domain controller and a Windows 11 client workstation using VirtualBox. After configuring Active Directory Domain Services, DNS, users, organizational units, and security groups, I joined the Windows 11 workstation to the domain and used the environment to complete several simulated help desk tickets.

The troubleshooting scenarios included password resets, account lockouts, network share permissions, Group Policy drive mapping, and employee offboarding.

---

## Lab Environment

### Domain

- **Domain:** `techlab.local`
- **Domain Controller:** `TECHLAB-DC01`
- **Domain Controller IP:** `192.168.56.10`

### Client Workstation

- **Computer:** `TECHLAB-PC01`
- **Operating System:** Windows 11
- **IP Address:** `192.168.56.20`
- **DNS Server:** `192.168.56.10`

### Virtualization

- Oracle VirtualBox
- Windows Server
- Windows 11

---

## Technologies & Skills Used

- Active Directory Domain Services (AD DS)
- Active Directory Users and Computers (ADUC)
- Windows Server
- Windows 11
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
- User authentication
- Access troubleshooting
- Employee onboarding/offboarding concepts

---

# Active Directory Configuration

I configured `TECHLAB-DC01` as the Domain Controller for the `techlab.local` domain.

The Active Directory environment was organized using dedicated Organizational Units for users, computers, and security groups.

The user structure included separate departments for:

- Help Desk
- Human Resources
- Information Technology
- Sales
- Disabled Users

Security groups were created to manage access based on department rather than assigning permissions directly to individual users.

Examples included:

- `GG-HelpDesk`
- `GG-HR`
- `GG-IT`
- `GG-Sales`

---

# Windows 11 Domain Join

I created a separate Windows 11 virtual workstation named `TECHLAB-PC01`.

The workstation was configured with a static IPv4 address and configured to use `TECHLAB-DC01` as its DNS server.

After verifying network connectivity and DNS resolution, I joined the workstation to:

`techlab.local`

Domain authentication was verified by successfully logging into the workstation with an Active Directory user account.

Commands such as `whoami`, `hostname`, and `echo %logonserver%` were used to verify the authenticated user, workstation name, and Domain Controller.

---

# Help Desk Scenarios

## Ticket #1 — Forgotten Password

### Issue

An HR employee was unable to log into the domain because they had forgotten their password.

### Resolution

Using Active Directory Users and Computers:

1. Located the employee's Active Directory account.
2. Reset the user's password.
3. Required the user to change the temporary password at next logon.
4. Verified successful authentication from `TECHLAB-PC01`.

### Skills Demonstrated

- Password resets
- Active Directory user administration
- Domain authentication
- User identity verification

[View full ticket documentation](Troubleshooting/Ticket-01-Password-Reset.md)

---

## Ticket #2 — Account Lockout

### Issue

A user exceeded the configured invalid-login threshold and was locked out of their domain account.

### Investigation

The domain Account Lockout Policy was configured to lock an account after five unsuccessful authentication attempts.

The user's account status was reviewed in Active Directory and confirmed to be locked.

### Resolution

The account was unlocked without unnecessarily resetting the user's password.

The user then successfully authenticated using their existing password.

### Skills Demonstrated

- Account lockout troubleshooting
- Active Directory account management
- Group Policy
- Authentication troubleshooting

[View full ticket documentation](Troubleshooting/Ticket-02-Account-Lockout.md)

---

## Ticket #3 — Network Share Access Denied

### Issue

A Sales employee attempted to access the HR network share:

`\\TECHLAB-DC01\HR`

The user received an Access Denied message.

### Investigation

I reviewed:

- Network connectivity
- Share availability
- NTFS permissions
- Active Directory security-group membership

The HR folder granted access through the `GG-HR` security group.

The affected employee was a member of `GG-Sales` but was not originally a member of `GG-HR`.

### Resolution

For the lab scenario, approved temporary access was simulated.

The employee was added to `GG-HR`, signed out, and signed back in so Windows could create a new security token containing the updated group membership.

Access to the HR network share was then successfully verified.

### Skills Demonstrated

- NTFS permissions
- SMB file sharing
- Security groups
- Access control
- Permission troubleshooting
- Least-privilege concepts

[View full ticket documentation](Troubleshooting/Ticket-03-Share-Permissions.md)

---

# Group Policy — Automatic Drive Mapping

I configured Group Policy Preferences to automatically map the HR network share for users in the HR Organizational Unit.

### Configuration

- **GPO:** `GPO-HR-DriveMap`
- **Network Path:** `\\TECHLAB-DC01\HR`
- **Drive Letter:** `H:`
- **Drive Label:** `HR Department`

The policy was linked to the HR OU.

After Group Policy refreshed, an HR employee received the mapped `H:` drive automatically when signing into the domain workstation.

The applied policy was verified using `gpresult /r`.

### Skills Demonstrated

- Group Policy Management
- Group Policy Preferences
- Drive mapping
- OU-based policy deployment
- GPO troubleshooting and verification

---

# Ticket #4 — Employee Offboarding

## Request

An employee was designated for offboarding from the TECHLAB environment.

## Actions Performed

1. Reviewed the employee's existing Active Directory group memberships.
2. Disabled the Active Directory account.
3. Removed departmental and temporary security-group access.
4. Verified the account could no longer authenticate.
5. Moved the disabled account into the `Disabled Users` OU.

## Verification

An authentication attempt from `TECHLAB-PC01` was rejected after the account was disabled.

### Skills Demonstrated

- User lifecycle management
- Active Directory account disabling
- Security-group management
- Access removal
- Employee offboarding

[View full ticket documentation](Troubleshooting/Ticket-04-Employee-Offboarding.md)

---

# Key Takeaways

This lab provided hands-on experience building and supporting a small Windows domain environment.

Rather than only configuring Active Directory, I used the environment to practice troubleshooting common user-support issues from initial problem through verification.

The project strengthened my understanding of how Active Directory, DNS, security groups, NTFS permissions, Group Policy, and Windows clients interact in a domain environment.

---

## Project Scope

This environment was created as a home lab for educational purposes. The users, organization, domain, tickets, and business scenarios shown in this repository are fictional.
