# Ticket #02 — Active Directory Account Lockout

## Ticket Information

**User:** Chris Davis  
**Department:** Help Desk  
**Workstation:** TECHLAB-PC01  
**Domain:** techlab.local  
**Priority:** Normal  
**Status:** Resolved

---

## Issue

The user reported being unable to sign into their Windows workstation.

The issue occurred after multiple unsuccessful password attempts.

---

## Investigation

I reviewed the user's account in Active Directory Users and Computers.

The account was confirmed to be locked.

The domain Account Lockout Policy was configured with a threshold of five invalid login attempts.

This explained why the user was unable to authenticate even after entering the correct password.

---

## Root Cause

The user exceeded the domain's configured account lockout threshold by entering an incorrect password multiple times.

This caused Active Directory to lock the account.

---

## Resolution

Because the user knew their existing password, a password reset was not necessary.

I performed the following actions:

1. Located Chris Davis in Active Directory Users and Computers.
2. Opened the user's account properties.
3. Confirmed the account was locked.
4. Unlocked the account.
5. Left the existing password unchanged.

---

## Verification

The user returned to TECHLAB-PC01 and authenticated using the existing domain password.

The login completed successfully.

---

## Tools Used

- Active Directory Users and Computers
- Group Policy Management
- Windows Server
- Windows 11

---

## Skills Demonstrated

- Account lockout troubleshooting
- Active Directory administration
- Account Lockout Policy
- Authentication troubleshooting
- Root-cause identification
- Resolution verification

---

## Resolution Summary

**Root Cause:** Account exceeded the five-attempt lockout threshold.

**Solution:** Unlocked the Active Directory account without resetting the user's password.

**Result:** User successfully authenticated using the existing password.
