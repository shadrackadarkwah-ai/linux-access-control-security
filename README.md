# Linux Access Control and Security Administration

![Platform](https://img.shields.io/badge/Platform-Ubuntu%20Linux-E95420?logo=ubuntu&logoColor=white)
![Focus](https://img.shields.io/badge/Focus-Linux%20Security-blue)
![Access Control](https://img.shields.io/badge/Security-ACLs-green)
![Project Type](https://img.shields.io/badge/Type-Hands--On%20Lab-purple)

## 1. Project Overview

I completed this hands-on Linux system administration and security project to develop practical skills in user and group management, file and directory permissions, ownership, Access Control Lists (ACLs), and access-control verification.

Using an Ubuntu Linux virtual machine, I configured a shared application project directory for development, security, and operations accounts. I applied Linux permissions and ACLs to establish different access levels, then tested authorized and unauthorized file operations to verify the resulting behavior.

This project demonstrates my understanding of Linux access-control mechanisms and the importance of validating security configurations through practical testing.

## 2. Project Objectives

- Inspect Linux user accounts, groups, and account records.
- Create and verify project user and group accounts.
- Configure file and directory ownership.
- Apply Linux permission modes and the setgid bit.
- Configure named-group Access Control Lists.
- Implement default ACLs and examine permission inheritance.
- Test authorized file access and denied write operations.
- Verify effective permissions using Linux security utilities.
- Document the implementation and test results with screenshots.

## 3. Lab Environment

| Component | Configuration |
|---|---|
| Operating system | Ubuntu Linux |
| Environment | Virtual machine |
| Project directory | `/opt/application-project` |
| Development account | `developers-team` |
| Security account | `security-team` |
| Operations account | `operation-team` |
| Development group | `developers-group` |
| Security group | `security-group` |
| Operations group | `operation-group` |
| ACL utilities | `getfacl`, `setfacl` |

## 4. Project Architecture

I organized the lab around three teams to demonstrate how Linux permissions and group-based access control can separate responsibilities.

| Team | Account | Group | Intended responsibility |
|---|---|---|---|
| Development | `developers-team` | `developers-group` | Manage application project files |
| Security | `security-team` | `security-group` | Inspect permitted project content |
| Operations | `operation-team` | `operation-group` | Access and maintain permitted project files |

The effective permissions depend on user and group membership, directory traversal permissions, file permissions, ACL entries, and the ACL mask.

## 5. User and Group Management

I inspected Linux identity information and account records using standard Linux administration utilities.

Examples of the commands used included:

```bash
id
id -gn
id -G
id -Gn
getent passwd
getent group
```

I also inspected individual account records and verified project account information.

For password-aging inspection, I used the `chage` utility. For example:

```bash
sudo chage -l developers-team
```

The `-l` option displays password-aging information for the specified account.

I inspected group records and user identities to verify account and group information.

## 6. File and Directory Permissions

I used the following directory as the shared project workspace:

```text
/opt/application-project
```

I configured the directory ownership for the development account and group and applied permission mode `2770`.

```bash
sudo chmod 2770 /opt/application-project
```

The mode provides read, write, and execute permissions to the owner and group while denying access through the ordinary `other` permission entry.

The leading `2` sets the setgid bit on the directory, helping newly created entries inherit the directory's group ownership.

I verified the resulting configuration by inspecting directory ownership and permissions.

## 7. Access Control Lists (ACLs)

I used ACLs to configure different access levels for the security and operations groups.

I inspected the directory ACL using:

```bash
getfacl /opt/application-project
```

I configured named-group permissions with:

```bash
sudo setfacl -m g:security-group:rx,g:operation-group:rwx /opt/application-project
```

The intended policy was to give the security group read and directory-traversal permissions, while the operations group received read, write, and directory-traversal permissions.

I also examined how the ACL mask affects effective permissions. A named ACL entry can specify permissions that are reduced by the ACL mask, so I used `getfacl` to inspect the actual effective access.

## 8. Default ACLs and Inheritance

I configured default ACL entries so that newly created files and directories could inherit the intended access-control policy.

```bash
sudo setfacl -d -m u::rwx,g::rwx,g:security-group:rx,g:operation-group:rwx,m::rwx,o::--- /opt/application-project
```

I created a test file after configuring the default ACL and inspected its permissions.

The resulting effective permissions were influenced by the ACL mask and the permissions requested when the file was created. I therefore checked the resulting ACL rather than assuming that every inherited permission would be fully effective.

This exercise also demonstrated that default ACLs apply to newly created entries and do not automatically rewrite the ACLs of files that already existed.

## 9. Security Testing and Validation

I tested file access using the intended project accounts.

### Test 1: Security account read access

```bash
sudo -u security-team cat /opt/application-project/project-notes.txt
```

**Recorded result:** The security account could read the project notes.

### Test 2: Unauthorized write attempt

```bash
sudo -u security-team sh -c 'echo "Unauthorized modification" >> /opt/application-project/project-notes.txt'
```

**Recorded result:** The write attempt was denied, and the file contents remained unchanged.

### Test 3: Operations account write access

```bash
sudo -u operation-team sh -c 'echo "Operations Team - Maintenance Review" >> /opt/application-project/project-notes.txt'
```

**Recorded result:** The operations account successfully appended content to the project notes.

### Test 4: Default ACL verification

I created a new test file after applying the default ACL configuration. I inspected the resulting ACL and verified that the security account could read the file but could not append unauthorized content.

These tests helped me understand how Linux permissions and ACLs enforce access restrictions and why security settings must be validated through actual operations.

## 10. Technical Documentation

I organized the detailed documentation into separate guides so that each part of the project can be reviewed independently.

| Document | Description |
|---|---|
| [User Management](docs/01-user-management.md) | User account inspection and password-aging information |
| [Group Management](docs/02-group-management.md) | Group records and membership verification |
| [File Permissions and Ownership](docs/03-file-permissions-and-ownership.md) | Directory ownership, mode `2770`, and setgid |
| [Access Control Lists](docs/04-access-control-lists.md) | Named-group ACLs and effective permissions |
| [Default ACLs and Inheritance](docs/05-default-acls-and-inheritance.md) | Default ACL configuration and inheritance tests |
| [Security Testing and Validation](docs/06-security-testing-and-validation.md) | Access tests and verification results |
| [Lessons Learned](docs/07-lessons-learned.md) | Technical lessons and future improvements |

*These links will work after the corresponding documentation files have been added to the repository.*

## 11. Screenshots and Evidence

I stored screenshots from the Ubuntu lab in the [`Screenshots/`](Screenshots/) directory.

The screenshots document:

- User identity and account inspection.
- Group creation and membership verification.
- Directory ownership and permission configuration.
- ACL configuration for the security and operations groups.
- Permission inheritance and final verification.

The screenshots serve as visual evidence of the practical work completed during the project.

## 12. Skills Demonstrated

Through this project, I practiced:

- Linux system administration
- User and group management
- File and directory permissions
- Ownership and group ownership
- Setgid directory permissions
- Access Control Lists (ACLs)
- Default ACL inheritance
- Effective-permission analysis
- Security testing and validation
- Technical documentation

## 13. Security Considerations

This project was performed in a learning environment. Permission behavior should always be verified against the actual account memberships, file permissions, directory traversal rights, ACL entries, and ACL mask.

I also recognize the importance of protecting credentials and reviewing screenshots before publishing them. Passwords, access tokens, private keys, and other sensitive information should never be committed to a public repository.

## 14. Future Improvements

I plan to extend my Linux security portfolio with additional hands-on work involving:

- Linux process and service management.
- Security monitoring and audit logging.
- Automated permission verification.
- Repeatable system configuration checks.
- Additional Linux security hardening exercises.

## 15. Conclusion

This project strengthened my practical understanding of Linux access control and security administration. I configured user and group access, managed directory permissions, applied ACLs, explored default ACL inheritance, and tested both permitted and denied file operations.

By documenting the configuration and verification process, I created a technical record of my learning that can be reviewed as part of my ongoing development toward a career in DevSecOps.

---

**Author:** Shadrack Adarkwah  
**Project:** Linux Access Control and Security Administration  
**Platform:** Ubuntu Linux  
**Focus:** Linux Administration, Permissions, ACLs, and Security Testing

