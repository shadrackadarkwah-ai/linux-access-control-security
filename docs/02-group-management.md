# Linux Group Management

## 1. Overview

In this stage of my Linux Access Control and Security Administration project, I created groups for different project responsibilities and assigned the relevant user accounts to those groups.

Linux groups make it easier to manage access to shared files and directories without configuring permissions separately for every user.

## 2. Lab Environment

- **Operating system:** Ubuntu Linux
- **Environment:** Virtual machine

### Project users

- `developers-team`
- `security-team`
- `operation-team`

### Project groups

- `developers-group`
- `security-group`
- `operation-group`

## 3. Creating Groups

I created separate groups to represent the responsibilities of the project users.

### Command 1: Create the developers group

```bash
sudo groupadd developers-group
```

### Command 2: Create the security group

```bash
sudo groupadd security-group
```

### Command 3: Create the operations group

```bash
sudo groupadd operation-group
```

**Command explanation:**

- `sudo` runs the command with administrative privileges.
- `groupadd` creates a new Linux group.
- The group name identifies the group being created.

## 4. Adding Users to Groups

I used `usermod -aG` to add each project user to the corresponding supplementary group.

### Command 4: Add the developers user

```bash
sudo usermod -aG developers-group developers-team
```

### Command 5: Add the security user

```bash
sudo usermod -aG security-group security-team
```

### Command 6: Add the operations user

```bash
sudo usermod -aG operation-group operation-team
```

**Command explanation:**

- `usermod` modifies an existing user account.
- `-a` means *append*, preserving existing supplementary group memberships.
- `-G` specifies the supplementary groups to assign.
- The group name identifies the target group.
- The username identifies the account to modify.

**Security note:** Using `-aG` is important. Using `-G` without `-a` can replace the user's existing supplementary group list.

## 5. Verifying Group Creation

I used `getent group` to inspect the project groups.

```bash
getent group developers-group
getent group security-group
getent group operation-group
```

**Purpose:** Displays each group's record, including its group ID and listed supplementary members.

## 6. Verifying User Memberships

I used `id` to check the group memberships associated with each project user.

```bash
id developers-team
id security-team
id operation-team
```

**Purpose:** Displays each user's UID, primary GID, and supplementary groups.

I checked that each user had the intended supplementary group membership.

## 7. Security Considerations

- Assign users only to groups required for their responsibilities.
- Verify memberships after changing group assignments.
- Use supplementary groups to manage shared access where appropriate.
- Review group memberships periodically to prevent unnecessary access.

## 8. Conclusion

This stage strengthened my understanding of Linux group administration. Creating groups and assigning users to them establishes the foundation for managing shared directory ownership, file permissions, and access control lists (ACLs).

**Project:** Linux Access Control and Security Administration  
**Platform:** Ubuntu Linux  
**Documentation stage:** Group Management
