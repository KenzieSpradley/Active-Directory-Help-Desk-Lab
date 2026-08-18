Ticket #03 - Network Share Access Denied

User:
Sam Miller - Sales

Issue:
User reported receiving Access Denied when attempting to access the HR network share.

Resource:
\\TECHLAB-DC01\HR

Investigation:
Verified that the network share was reachable.
Reviewed the user's Active Directory group memberships.
Sam was a member of GG-Sales but not GG-HR.
Reviewed the HR folder permissions and determined that access was granted through the GG-HR security group.

Root Cause:
The user's account did not belong to the security group authorized to access the HR resource.

Authorization:
Access was assumed approved by HR management for this simulated lab scenario.

Resolution:
Added Sam Miller to the GG-HR security group.

Signed the user out and back in to refresh the user's Windows security token.

Verification:
Used WHOAMI /GROUPS to verify GG-HR membership.
Successfully accessed \\TECHLAB-DC01\HR and created a test file.

Tools Used:
Active Directory Users and Computers
NTFS Permissions
SMB File Sharing
Command Prompt
Windows 11