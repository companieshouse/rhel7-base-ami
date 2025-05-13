### OpenSCAP Remediation Roles

This project automates the process of **remediating security vulnerabilities** based on an OpenSCAP report.
The playbook is structured with multiple roles, each responsible for specific aspects of security configuration and compliance.

#### Roles Included for Security Remediation

| Role             | Purpose                            | Tasks |
|------------------|-------------------------------------|-------|
| core_dumps       | Disable core file creation          | Disable core dump backtraces  <br> Disable storing core dumps |
| disable_mounting | Prevent loading insecure filesystems| Disable Mounting of cramfs  <br> Disable Mounting of freevxfs  <br> Disable Mounting of hfs  <br> Disable Mounting of hfsplus  <br> Disable Mounting of jffs2  <br> Disable Modprobe Loading of USB Storage Driver |
| ipv6             | Harden IPv6 network behavior        | Disable Accepting Source-Routed Packets on all IPv6 Interfaces  <br> Disable IPv6 Forwarding  <br> Disable Accepting Source-Routed Packets on IPv6 Interfaces by Default |
| journald         | Secure and persist system logs      | Configure journald to compress large log files  <br> Send logs to rsyslog  <br> Write log files to persistent disk |
| motd             | Set compliant login banner          | Modify System Message of the Day Banner |
| nfs              | Disable insecure file sharing       | Disable rpcbind Service  <br> Disable Network File System (nfs) |
| password_policy  | Enforce strong authentication       | Prevent Dictionary Word Passwords  <br> Min. Different Characters  <br> Max Repeating Characters  <br> Min. Different Categories  <br> Prevent Empty Passwords  <br> pam_wheel group exists and is empty  <br> Enforce pam_wheel su auth  <br> Limit Password Reuse (auth/system)  <br> Lock on Failed Attempts  <br> Set Lockout Time |
| postfix          | Restrict mail service exposure      | Disable Postfix Network Listening |
| ptrace           | Restrict debugging access           | Restrict usage of ptrace to descendant processes |
| rsyslog          | Set secure log file permissions     | Configure rsyslog default file permissions |
| session_timeout  | Prevent idle user sessions          | Set Interactive Session Timeout |
| ssh              | Harden remote access configuration  | Set Client Alive Count Max  <br> Set Client Alive Interval  <br> Set LogLevel VERBOSE  <br> Set MaxSessions limit  <br> Configure MaxStartups  <br> Use Only FIPS Ciphers  <br> Use Strong Key Exchange  <br> Use Strong MACs  <br> Limit SSH Access |
| sudo             | Secure privilege escalation         | Use tty for sudo  <br> Ensure Sudo Logfile Exists  <br> Require Re-Authentication |
| umask            | Enforce secure file permissions     | Set Default Umask  <br> Set Umask in /etc/profile |

#### Excluded Remediation Tasks

The tasks below were skipped because they don’t apply, could cause issues, or are handled externally:

### Excluded Remediation Tasks

The tasks below were skipped because they don’t apply, could cause issues, or are managed externally:

| Task                                                                 | Reason for Exclusion |
|----------------------------------------------------------------------|-----------------------|
| Ensure Users Re-Authenticate for Privilege Escalation - sudo         | No `NOPASSWD` or `!authenticate` directives are not set in config |
| Lock Accounts After Failed Password Attempts                         | Already implemented; still flagged by scanner |
| Set Lockout Time for Failed Password Attempts                        | Already implemented; still flagged by scanner |
| Ensure Authentication Required for Single User Mode                  | Not relevant in cloud; serial console not used or supported |
| Set Boot Loader Password in grub2                                    | Bootloader is not accessible or required in cloud environments |
| Install firewalld Package                                            | Firewall is managed externally |
| Ensure No Daemons are Unconfined by SELinux                          | Marked as "unknown" in scan; no clear remediation required |
| Ensure Mail Transfer Agent is not Listening on any non-loopback Address | Marked as "unknown"; not currently applicable in environment |
| Uninstall rsync Package                                              | Required by `git` on RHEL 7; removal breaks dependencies |

