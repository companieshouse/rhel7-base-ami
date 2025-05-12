# RHEL7 BASE AMI

Provides a base RHEL 7 AMI build.


### Ansible

All Ansible configuration resides in the `./ansible` directory. The Ansible configuration will be called during the provisioning step of the Packer build as defined in `./packer/build.pkr.hcl`.

This template provides the basic code layout and structure only.

### Development

For convenience development scripts have been suppled and are as follows:

| Script                | Description                                                                                                                                       |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| inventory_aws_ec2.yml | Defines a dynamic inventory for Ansible by way of the `aws_ec2` plugin                                                                            |
| print-inventory       | Prints the inventory hosts for debugging purposes                                                                                                 |
| run-ansible           | Runs the playbook against the configured hosts. **Note**: This requires Ansible be installed on the host system                                   |
| variables.json        | A means of specifying additional variables. **Note**: These values should never be committed and as such this file has been added to `.gitignore` |

### Packer Variables

All Packer configuration resides in the `./packer` directory and utilises standard Packer configuration syntax.

The template provides the following variables to control the Packer build and provisioning process.

| Variable                   | Type   | Default                          | Description                                                                                                                                               |
| -------------------------- | ------ | -------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ami_account_ids            | string | -                                | A list of account IDs that have access to launch the resulting AMI(s).                                                                                    |
| ami_name_prefix            | string | -                                | Prefix used for the name tags of resulting AMIs. The version will be appended to this.                                                                    |
| aws_instance_type          | string | t3.small                         | AWS EC2 instance type used when building the AMI.                                                                                                         |
| aws_region                 | string | eu-west-2                        | The region in which the AMI will be built.                                                                                                                |
| aws_source_ami_filter_name | string | RHEL-7.9_HVM\*                    | Source AMI filter string as per the DescribeImages API documentation. If multiple match, the latest image will be used.                                   |
| aws_source_ami_owner_id    | string | -                                | The source AMI owner ID. Used in combination with `aws_source_ami_filter_name` to match the source AMI.                                                   |
| aws_subnet_filter_name     | string | -                                | Subnet filter string as per the DescribeSubnets API documentation. If multiple match, the subnet with the greatest number of IPv4 addresses will be used. |
| configuration_group        | string | unnamed                          | The name of the group to which to add the instance for configuration purposes                                                                             |
| data_volume_size_gib       | number | -                                | The EC2 instance data volume size in Gibibytes (GiB)                                                                                                      |
| force_delete_snapshot      | bool   | false                            | Automatically delete snapshots associated with AMIs deregistered by `force_deregister`.                                                                   |
| force_deregister           | bool   | false                            | Deregister an existing AMI if one with the same name exists.                                                                                              |
| playbook_file_path         | string | ../ansible/playbook.yml          | Relative path to the Ansible playbook file.                                                                                                               |
| root_volume_size_gb        | number | 20                               | The EC2 instance root volume size in Gibibytes (GiB).                                                                                                     |
| ssh_private_key_file       | string | /home/packer/.ssh/packer-builder | The path to the common Packer builder private SSH key.                                                                                                    |
| ssh_username               | string | ec2-user                           | The username Packer will use when connecting with SSH.                                                                                                    |
| version                    | string | -                                | Semantic version number for the AMI. Will be automatically appended to `ami_name_prefix` to tag the resulting AMI and snapshots.                          |

### OpenSCAP Remediation Roles

This project automates the process of **remediating security vulnerabilities** based on an OpenSCAP report.
The playbook is structured with multiple roles, each responsible for specific aspects of security configuration and compliance.

#### Roles Included for Security Remediation


#### core_dumps
**Purpose**:

**Tasks**:
- Disable core dump backtraces
- Disable storing core dumps

#### disable_mounting
**Purpose**:

**Tasks**:
- Disable Mounting of cramfs
- Disable Mounting of freevxfs
- Disable Mounting of hfs
- Disable Mounting of hfsplus
- Disable Mounting of jffs2
- Disable Modprobe Loading of USB Storage Driver

#### ipv6
**Purpose**:

**Tasks**:
- Disable Kernel Parameter for Accepting Source-Routed Packets on all IPv6 Interfaces
- Disable Kernel Parameter for IPv6 Forwarding
- Disable Kernel Parameter for Accepting Source-Routed Packets on IPv6 Interfaces by Default

#### journald
**Purpose**:

**Tasks**:
- Ensure journald is configured to compress large log files
- Ensure journald is configured to send logs to rsyslog
- Ensure journald is configured to write log files to persistent disk

#### motd
**Purpose**:

**Tasks**:
- Modify the System Message of the Day Banner - ensure correct banner

#### nfs
**Purpose**:

**Tasks**:
- Disable rpcbind Service
- Disable Network File System (nfs)

#### password_policy
**Purpose**:

**Tasks**:
- Ensure PAM Enforces Password Requirements - Prevent the Use of Dictionary Words
- Ensure PAM Enforces Password Requirements - Minimum Different Characters
- Set Password Maximum Consecutive Repeating Characters
- Ensure PAM Enforces Password Requirements - Minimum Different Categories
- Prevent Login to Accounts With Empty Password
- Ensure the Group Used by pam_wheel.so Module Exists on System and is Empty
- Enforce Usage of pam_wheel with Group Parameter for su Authentication
- Limit Password Reuse: password-auth
- Limit Password Reuse: system-auth
- Lock Accounts After Failed Password Attempts
- Set Lockout Time for Failed Password Attempts

#### postfix
**Purpose**:

**Tasks**:
- Disable Postfix Network Listening

#### ptrace
**Purpose**:

**Tasks**:
- Restrict usage of ptrace to descendant processes

#### rsyslog
**Purpose**:

**Tasks**:
- Ensure rsyslog Default File Permissions Configured

#### session_timeout
**Purpose**:

**Tasks**:
- Set Interactive Session Timeout

#### ssh
**Purpose**:

**Tasks**:
- Set SSH Client Alive Count Max
- Set SSH Client Alive Interval
- Set SSH Daemon LogLevel to VERBOSE
- Set SSH MaxSessions limit
- Ensure SSH MaxStartups is configured
- Use Only FIPS 140-2 Validated Ciphers
- Use Only Strong Key Exchange algorithms
- Use Only Strong MACs
- Limit Users' SSH Access

#### sudo
**Purpose**:

**Tasks**:
- Ensure Only Users Logged In To Real tty Can Execute Sudo - sudo use_pty
- Ensure Sudo Logfile Exists - sudo logfile
- Require Re-Authentication When Using the sudo Command

#### umask
**Purpose**:

**Tasks**:
- Ensure the Default Umask is Set Correctly
- Ensure the Default Umask is Set Correctly in etc/profile
