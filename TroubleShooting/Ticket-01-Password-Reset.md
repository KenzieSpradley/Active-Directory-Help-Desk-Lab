# Ticket #01 — Forgotten Password

## Ticket Information

**User:** Jamie Brown  
**Department:** Human Resources  
**Workstation:** TECHLAB-PC01  
**Domain:** techlab.local  
**Priority:** Normal  
**Status:** Resolved

---

## Issue

The user reported being unable to sign into the domain because they had forgotten their password.

The user required a password reset to regain access to their domain account.

---

## Investigation

I located Jamie Brown's account using Active Directory Users and Computers on TECHLAB-DC01.

Before making changes, I verified that I was working with the correct user account.

The issue was determined to be a forgotten password rather than an account lockout or disabled account.

---

## Resolution

I performed the following actions:

1. Located the user in Active Directory Users and Computers.
2. Selected **Reset Password**.
3. Assigned a temporary password that met the domain password requirements.
4. Enabled **User must change password at next logon**.
5. Provided the temporary credentials for the simulated user.

---

## Verification

I tested the resolution from TECHLAB-PC01.

The user was prompted to change the temporary password during the next login.

After creating a new password, Jamie successfully authenticated to the techlab.local domain.

---

## Tools Used

- Active Directory Users and Computers
- Windows Server
- Windows 11
- Active Directory Domain Services

---

## Skills Demonstrated

- Active Directory user administration
- Password resets
- Identity verification
- Temporary password management
- Domain authentication
- Resolution verification

---

## Resolution Summary

**Root Cause:** User forgot their domain password.

**Solution:** Reset the password and required the user to create a new password at next logon.

**Result:** User successfully regained access to the domain.
