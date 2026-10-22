 # Network Troubleshooting and Firewall Validation

Tested Windows-to-Ubuntu SSH connectivity, introduced a targeted outbound firewall block, and verified recovery after removing the rule.

## Environment

| System | Hostname | IPv4 address |
|---|---|---|
| Windows 11 VM | Soc-analyst | 192.168.213.131 |
| Ubuntu VM | soc-ubuntu | 192.168.213.133 |

Virtual machines ran in VMware Workstation. Tests targeted SSH over TCP port 22.

## Procedure and Results

1. Established a baseline: TCP port 22 was reachable, and SSH login succeeded.
2. Confirmed that Windows firewall profiles were enabled.
3. Created the outbound block rule `Lab-Block-Ubuntu-SSH`, targeting TCP port 22 at `192.168.213.133`.
4. Retested connectivity: ping succeeded, but the TCP connection failed. SSH reported `connect to host 192.168.213.133 port 22: Permission denied` before authentication.
5. Inspected the rule’s address and port filters to confirm its scope.
6. Removed the lab rule with `Remove-NetFirewallRule`.
7. Verified recovery: the TCP connection succeeded, and SSH login worked. The commands `whoami` and `hostname` returned `dercy` and `soc-ubuntu`.

## Finding

Successful ping replies did not establish that SSH was accessible. The targeted outbound firewall rule blocked TCP port 22 while ping remained available. Removing the rule restored both TCP connectivity and SSH login.

## Evidence

- [Screenshots](Screenshots/)
- [Project notes](Notes/Project-Notes.txt)

Evidence covers baseline connectivity, the blocked connection, firewall rule scope, and restored access.

## Scope

This was a controlled exercise on personal virtual machines. Validation used PowerShell connection tests, firewall rule inspection, and SSH login attempts. No packet capture was performed.
