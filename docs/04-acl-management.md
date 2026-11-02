# Linux Access Control Lists (ACLs) Management

## 1. Overview

In this stage of my Linux Access Control and Security Administration project, I configured and verified Access Control Lists (ACLs) to provide more granular access control for different teams working on the Application Project.

I used ACLs to assign specific permissions to the security and operations groups without changing the standard ownership of the project directory and files.

I also configured default ACLs to support permission inheritance for newly created files and directories.

## 2. Lab Environment

Operating system: Ubuntu Linux  
Project directory: `/opt/application-project`  
Project file: `/opt/application-project/project-notes.txt`  
Test file: `/opt/application-project/operations-default-test.txt`  
Directory owner: developers-team  
Directory group: developers-group  

Groups involved:
- developers-group
- security-group
- operation-group

## 3. Understanding Access Control Lists

Access Control Lists (ACLs) extend the traditional Linux permission system by allowing administrators to assign permissions to specific users and groups.

Standard Linux permissions provide access control for the owner, owning group, and others.

ACLs provide additional flexibility by allowing named users and groups to receive specific permissions.

In this project, ACLs were used to separate the access responsibilities of the security and operations teams.

## 4. Inspecting Directory ACLs

Command 1: Inspect the project directory and test file ACLs

```bash
sudo getfacl /opt/application-project /opt/application-project/operations-default-test.txt
```

Explanation:

- `sudo` runs the command with administrative privileges.
- `getfacl` displays Access Control List entries for files and directories.
- `/opt/application-project` specifies the project directory.
- `/opt/application-project/operations-default-test.txt` specifies the test file.

The command displayed the ownership, standard permissions, named group permissions, default ACL entries, and effective permissions.

## 5. Understanding Directory ACL Permissions

The directory ACL output showed the following entries:

```text
# owner: developers-team
# group: developers-group
# flags: -s-

user::rwx
group::rwx
group:security-group:r-x
group:operation-group:rwx
mask::rwx
other::---
```

Explanation:

- `user::rwx`: The directory owner has read, write, and execute permissions.
- `group::rwx`: The owning group has read, write, and execute permissions.
- `group:security-group:r-x`: The security group has read and execute permissions.
- `group:operation-group:rwx`: The operations group has read, write, and execute permissions.
- `mask::rwx`: Defines the maximum effective permissions for the owning group and named group entries.
- `other::---`: Other users have no permissions through the other permission entry.

The directory also retained the setgid flag, helping newly created items inherit the directory's owning group under normal Linux filesystem behaviour.

## 6. Configuring and Understanding Default ACLs

Default ACLs allow directories to define access-control entries that are inherited by newly created files and subdirectories.

The project directory displayed the following default ACL entries:

```text
default:user::rwx
default:group::rwx
default:group:security-group:r-x
default:group:operation-group:rwx
default:mask::rwx
default:other::---
```

Explanation:

- `default:user::rwx`: Defines the default owner permissions.
- `default:group::rwx`: Defines the default owning group permissions.
- `default:group:security-group:r-x`: Defines the default permissions for the security group.
- `default:group:operation-group:rwx`: Defines the default permissions for the operations group.
- `default:mask::rwx`: Defines the maximum effective group permissions in the default ACL.
- `default:other::---`: Defines the default permissions for other users.

These entries help maintain consistent access-control policies for newly created items.

Actual inherited permissions also depend on the permissions requested when an object is created.

## 7. Verifying Inherited ACL Permissions

I inspected the ACL entries of the test file:

```bash
sudo getfacl /opt/application-project/operations-default-test.txt
```

The screenshot showed the following file information:

```text
# owner: operation-team
# group: developers-group

user::rw-
group::rwx
group:security-group:r-x
group:operation-group:rwx
mask::rw-
other::---
```

The file also displayed the following effective permissions:

```text
group::rwx                  #effective:rw-
group:security-group:r-x    #effective:r--
group:operation-group:rwx   #effective:rw-
```

Explanation:

- `user::rw-`: The file owner has read and write permissions.
- `group::rwx`: The owning group ACL entry contains read, write, and execute permissions.
- `group:security-group:r-x`: The security group ACL entry contains read and execute permissions.
- `group:operation-group:rwx`: The operations group ACL entry contains read, write, and execute permissions.
- `mask::rw-`: Restricts the effective permissions of the owning group and named group entries.
- `other::---`: Other users have no permissions.

## 8. Understanding the ACL Mask

The ACL mask determines the maximum effective permissions available to the owning group and named group entries.

In the test file, the mask was:

```text
mask::rw-
```

This produced the following effective permissions:

| ACL Entry | Configured Permissions | Effective Permissions |
|---|---|---|
| Owning group | rwx | rw- |
| security-group | r-x | r-- |
| operation-group | rwx | rw- |

The screenshot demonstrated that although some ACL entries contained execute permission, the ACL mask restricted their effective permissions.

This is important because the permissions displayed in an ACL entry do not always represent the permissions a group can effectively use.

## 9. Verifying ACL Configuration

I used the `getfacl` command to inspect and verify the ACL configuration.

The verification confirmed:

- The project directory was owned by developers-team.
- The owning group was developers-group.
- The directory contained named ACL entries for security-group and operation-group.
- Default ACL entries were configured on the project directory.
- The test file contained ACL entries for both security and operations groups.
- The ACL mask restricted the effective permissions of the group entries.
- The test file was owned by operation-team with developers-group as its owning group.

The screenshot captured the final ACL verification results.

## 10. Security Considerations

- Apply the principle of least privilege when assigning ACL permissions.
- Avoid granting unnecessary write or execute permissions.
- Review the ACL mask because it determines effective group permissions.
- Use default ACLs to maintain consistent access-control policies for newly created items.
- Regularly inspect ACL entries to identify unintended permissions.
- Restrict access for users who do not require access to project resources.
- Verify ownership and permissions after making ACL changes.

## 11. Conclusion

This stage gave me practical experience understanding and verifying Linux Access Control Lists.

I learned how named group ACLs provide more granular access control, how default ACLs support permission inheritance, and how the ACL mask controls effective permissions.

I also used the `getfacl` command to inspect directory permissions, default ACL entries, file ownership, and effective group permissions.

This exercise strengthened my understanding of Linux access control, permission inheritance, and security administration.

Project: Linux Access Control and Security Administration  
Platform: Ubuntu Linux  
Documentation stage: ACL Management
