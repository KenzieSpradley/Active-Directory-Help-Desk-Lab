# Ticket #04 — Employee Offboarding

## Ticket Information

**User:** Sam Miller  
**Department:** Sales  
**Domain:** techlab.local  
**Request Source:** Human Resources  
**Priority:** Normal  
**Status:** Completed

---

## Request

An approved request was received to offboard Sam Miller from the TECHLAB environment following the simulated end of employment.

The objective was to prevent further authentication and remove the user's access to departmental resources.

---

## Initial Review

Before modifying the account, I reviewed the employee's existing Active Directory configuration.

The account had membership in:

- Domain Users
- GG-Sales
- GG-HR

The GG-HR membership had previously been assigned as temporary access during another simulated help desk ticket.

---

## Actions Performed

I completed the following Active Directory offboarding actions:

1. Located Sam Miller's account.
2. Reviewed the existing security-group memberships.
3. Disabled the Active Directory user account.
4. Removed GG-Sales membership.
5. Removed temporary GG-HR membership.
6. Retained the standard Domain Users primary group.
7. Attempted authentication from TECHLAB-PC01.
8. Confirmed that the disabled account could no longer sign in.
9. Moved the account into the Disabled Users Organizational Unit.

---

## Verification

An authentication attempt was performed from TECHLAB-PC01 using the disabled account.

Windows rejected the login.

This confirmed that the user could no longer authenticate to the domain.

The account was then verified inside the Disabled Users OU.

---

## Tools Used

- Active Directory Users and Computers
- Windows Server
- Windows 11
- Active Directory Security Groups

---

## Skills Demonstrated

- Employee offboarding
- Active Directory user lifecycle management
- Account disabling
- Security-group management
- Access removal
- Authentication verification
- Organizational Unit management

---

## Production Environment Considerations

A real-world employee offboarding process could include additional systems and procedures depending on the organization, such as:

- Microsoft 365 account handling
- Email access
- MFA sessions
- VPN access
- Application accounts
- Software licenses
- Company-owned devices
- File ownership
- Data retention requirements

This lab focused specifically on the on-premises Active Directory portion of the offboarding process.

---

## Resolution Summary

**Request:** Remove the departing employee's access to the TECHLAB environment.

**Solution:** Disabled the Active Directory account, removed departmental security-group memberships, verified authentication was blocked, and moved the account to the Disabled Users OU.

**Result:** The offboarded account could no longer authenticate or retain its previous departmental access.
