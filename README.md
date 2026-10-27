# linux-access-control-security

# 01 — Linux User Management

## 1. Overview

In this stage of my Linux Access Control and Security Administration project, I created project user accounts and inspected user identities, account records, group information, and password-aging settings.

The goal was to understand Linux user management and how account information supports access control.

## 2. Lab Environment

- **Operating system:** Ubuntu Linux
- **Environment:** Virtual machine
- **Project accounts:** `developers-team`, `security-team`, `operation-team`

## 3. Identifying the Current User

### Command 1: Display the current username

```bash
whoami
```

**Purpose:** Displays the username of the user currently running the command.

### Command 2: Display user and group identity

```bash
id
```

**Purpose:** Displays the current user's UID, primary GID, and supplementary groups.

### Command 3: Display the primary group name

```bash
id -gn
```

**Purpose:** Displays the current user's primary group name.

### Command 4: Display numeric group IDs

```bash
id -G
```

**Purpose:** Displays the numeric group IDs associated with the current user.

### Command 5: Display group names

```bash
id -Gn
```

**Purpose:** Displays the names of the groups associated with the current user.

## 4. Creating Project User Accounts

I created three user accounts for the different responsibilities in my Linux access-control lab.

### Command 6: Create the project accounts

```bash
sudo useradd developers-team
sudo useradd security-team
sudo useradd operation-team
```

**Command breakdown:**

- `sudo` runs the command with administrative privileges.
- `useradd` creates a new user account.
- `developers-team`, `security-team`, and `operation-team` are the account names.

**Important:** I used `useradd` without the `-m` option. Therefore, a home directory was not automatically created by these commands under the usual Ubuntu configuration.

## 5. Inspecting User and Group Account Records

### Command 7: Display user account records

```bash
getent passwd
```

**Purpose:** Displays user account records from the system's configured account databases.

### Command 8: Inspect a specific user account

```bash
getent passwd developers-team
```

**Purpose:** Displays the account record for `developers-team`.

I could use the same command with `security-team` or `operation-team` to inspect the other accounts.

### Command 9: Display group records

```bash
getent group
```

**Purpose:** Displays group records, including group names, group IDs, and listed group members.

## 6. Inspecting Password-Aging Information

After creating the project accounts, I inspected their password-aging information.

### Command 10: Inspect the developers account

```bash
sudo chage -l developers-team
```

### Command 11: Inspect the security account

```bash
sudo chage -l security-team
```

### Command 12: Inspect the operations account

```bash
sudo chage -l operation-team
```

**Command breakdown:**

- `sudo` runs the command with administrative privileges.
- `chage` displays or manages password-aging information.
- `-l` lists the account's password-aging settings.
- The username specifies which account to inspect.

These commands inspect existing settings; they do not, by themselves, change password-aging policies.

## 7. Verification

I used Linux identity and account inspection commands to review user identities and account records. This helped me understand the relationship between usernames, user IDs, group memberships, and password-aging information.

## 8. Security Considerations

- User accounts should have only the access required for their responsibilities.
- Group memberships should be reviewed before relying on group-based access controls.
- Password-aging settings should be reviewed according to organizational security requirements.
- Screenshots should be checked for sensitive information before being published publicly.

## 9. Conclusion

This stage strengthened my understanding of Linux user creation and account inspection. These fundamentals support the later stages of my project, including group management, file ownership, permissions, access control lists (ACLs), and security validation.

**Project:** Linux Access Control and Security Administration  
**Platform:** Ubuntu Linux  
**Documentation stage:** User Management
