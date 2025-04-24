Project Objective
In this project, I explored common methods for maintaining access (persistence) to Windows systems after an initial compromise. My goal was to understand how attackers retain control over a machine even if their initial attack vector is patched or detected.

Work Performed
Leveraging Unprivileged Accounts

Added thmuser0 to the Administrators group for full control.

For stealth, added thmuser1 to Backup Operators, granting access to SAM and SYSTEM files via reg save.

Bypassed UAC restrictions by modifying the LocalAccountTokenFilterPolicy registry key to enable full-privilege WinRM sessions.

Extracted password hashes using secretsdump.py and performed Pass-the-Hash to log in as Administrator.

Assigning Special Privileges

Used secedit to grant thmuser2 the SeBackupPrivilege and SeRestorePrivilege, allowing unrestricted file access.

Modified WinRM security descriptors to permit thmuser2 remote access without admin rights.

RID Hijacking – User ID Spoofing

Launched regedit as SYSTEM using PsExec64.

Manually changed thmuser3's RID to 500 (matching Administrator), granting admin privileges upon next login.

Results
Successfully gained persistent access through:

Direct admin group addition (thmuser0 → flag1.exe → THM{FLAG_BACKED_UP!}).

Backup Operators + SAM dump (thmuser1 → Pass-the-Hash → Administrator).

Special privileges (thmuser2 → flag2.exe → THM{IM_JUST_A_NORMAL_USER}).

RID Hijacking (thmuser3 → Admin access → flag3.exe).

Key Takeaways
Persistence is achievable without permanent admin access—via hidden groups, privileges, or registry manipulation.

UAC and security policies are critical—disabling LocalAccountTokenFilterPolicy was essential for some techniques.

Registry and group monitoring is mandatory—many methods leave traces (e.g., unexpected user privileges).

Defense Recommendations
Restrict membership in sensitive groups (e.g., Backup Operators).

Monitor registry changes (especially HKLM\SAM).

Implement LAPS to complicate hash theft.

Conclusion: This project demonstrated how easily access can be maintained if privilege and registry changes go unchecked. I now better understand how to defend against such attacks!

Flags Captured:

THM{FLAG_BACKED_UP!}

THM{IM_JUST_A_NORMAL_USER}
