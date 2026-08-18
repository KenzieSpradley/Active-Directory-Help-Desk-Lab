# Ticket #03 — Network Share Access Denied

## Ticket Information

**User:** Sam Miller  
**Department:** Sales  
**Workstation:** TECHLAB-PC01  
**Resource:** \\TECHLAB-DC01\HR  
**Domain:** techlab.local  
**Priority:** Normal  
**Status:** Resolved

---

## Issue

The user attempted to access the HR departmental network share but received an Access Denied message.

The affected resource was:

`\\TECHLAB-DC01\HR`

---

## Investigation

I verified that the network share existed and was accessible by an authorized HR employee.

Jamie Brown, a member of GG-HR, could successfully access the resource.

This helped determine that the server and network share were functioning correctly.

I then reviewed:

- The HR folder's NTFS permissions
- Active Directory security groups
- Sam Miller's group memberships

The HR folder granted access through the GG-HR security group.

Sam was a member of GG-Sales but was not originally a member of GG-HR.

---

## Root Cause

The user did not belong to the Active Directory security group authorized to access the HR network share.

The Access Denied message was therefore expected behavior based on the existing permissions.

---

## Authorization

For this simulated help desk ticket, temporary access was assumed to have been approved by HR management for a cross-department project.

In a production environment, access to a restricted departmental resource should not be granted solely because a user requests it. Appropriate authorization should be verified first.

---

## Resolution

I performed the following actions:

1. Verified the access request was approved for the lab scenario.
2. Added Sam Miller to the GG-HR security group.
3. Signed the user completely out of Windows.
4. Signed the user back in to generate an updated security token containing the new group membership.
5. Attempted to access the HR share again.

Permissions were managed through the existing security group rather than assigning NTFS permissions directly to the individual user.

---

## Verification

After signing back in, Sam successfully accessed:

`\\TECHLAB-DC01\HR`

The user was also able to create a test file, confirming that the expected permissions were working.

---

## Tools Used

- Active Directory Users and Computers
- Windows Server
- Windows 11
- SMB File Sharing
- NTFS Permissions
- Active Directory Security Groups

---

## Skills Demonstrated

- Network share troubleshooting
- NTFS permissions
- SMB file sharing
- Active Directory security groups
- Group-based access control
- Windows security tokens
- Least-privilege concepts
- Access verification

---

## Resolution Summary

**Root Cause:** User was not a member of GG-HR.

**Solution:** After simulated approval, added the user to GG-HR and refreshed the user's logon security token.

**Result:** User successfully accessed the authorized HR network share.
