
# Linux File and Folder Permissions

## 1. Overview

In this stage of my Linux Access Control and Security Administration project, I created a shared project directory, configured its ownership, and practised changing and verifying file permissions.

I used Linux commands to control who could access the project folder and its `project-notes.txt` file.

## 2. Lab Environment

- **Operating system:** Ubuntu Linux
- **Project directory:** `/opt/application-project`
- **Project file:** `/opt/application-project/project-notes.txt`
- **Directory owner:** `developers-team`
- **Directory group:** `developers-group`

## 3. Creating and Inspecting the Project Directory

### Command 1: Create the project directory

```bash
sudo mkdir -p /opt/application-project
```

**Explanation:**
- `sudo` runs the command with administrative privileges.
- `mkdir` creates a directory.
- `-p` creates missing parent directories if necessary and avoids an error if the target directory already exists.
- `/opt/application-project` is the directory path.

### Command 2: Inspect the directory

```bash
ls -ld /opt/application-project
```

**Explanation:**
- `ls` lists filesystem information.
- `-l` displays details such as permissions, owner, group, and modification time.
- `-d` displays the directory itself rather than its contents.

Initially, the directory was owned by `root:root`.

## 4. Configuring Directory Ownership

### Command 3: Change the directory owner and group

```bash
sudo chown developers-team:developers-group /opt/application-project
```

**Explanation:**
- `chown` changes ownership.
- `developers-team` becomes the owner.
- `developers-group` becomes the owning group.

I verified the change with:

```bash
ls -ld /opt/application-project
```

The output showed `developers-team` as the owner and `developers-group` as the group.

## 5. Setting Directory Permissions

### Command 4: Apply permission mode 2770

```bash
sudo chmod 2770 /opt/application-project
```

**Explanation:**
- `chmod` changes permissions.
- `2770` enables the setgid bit and gives the owner and group read, write, and execute permissions, while denying access to others.

The directory displayed:

```text
drwxrws---
```

### Understanding the permission mode

| Component | Meaning |
|---|---|
| `2` | Enables setgid on the directory |
| `7` | Owner: read, write, execute |
| `7` | Group: read, write, execute |
| `0` | Others: no permissions |

The setgid bit helps new files and subdirectories inherit the directory's group ownership under typical Linux filesystem behaviour.

## 6. Creating the Project File

### Command 5: Create `project-notes.txt`

```bash
sudo touch /opt/application-project/project-notes.txt
```

**Explanation:**
- `touch` creates an empty file if it does not exist.
- The full path specifies where the file is created.

### Command 6: Inspect the file

```bash
sudo ls -l /opt/application-project/project-notes.txt
```

The screenshot showed the new file initially owned by `root` with group `developers-group`. Its initial permissions were `-rw-r--r--`.

## 7. Configuring File Ownership

### Command 7: Change the file owner and group

```bash
sudo chown developers-team:developers-group /opt/application-project/project-notes.txt
```

This assigned the file to `developers-team` and `developers-group`.

## 8. Practising File Permissions

I used several `chmod` commands to observe how file permissions changed.

### Command 8: Set permissions to 660

```bash
sudo chmod 660 /opt/application-project/project-notes.txt
```

This gave the owner and group read and write permissions, with no permissions for others.

### Command 9: Remove group write permission

```bash
sudo chmod g-w /opt/application-project/project-notes.txt
```

- `g` refers to the group permission class.
- `-w` removes write permission.

### Command 10: Add group write permission

```bash
sudo chmod g+w /opt/application-project/project-notes.txt
```

- `g` refers to the group permission class.
- `+w` adds write permission.

### Command 11: Remove group read permission

```bash
sudo chmod g-r /opt/application-project/project-notes.txt
```

- `g` refers to the group permission class.
- `-r` removes read permission.

## 9. Verifying File Permissions

After each change, I inspected the file with:

```bash
sudo ls -l /opt/application-project/project-notes.txt
```

This allowed me to compare the permission string and confirm how each `chmod` operation changed the file's access settings.

The screenshot records the following progression:

1. Initial file permissions: `-rw-r--r--`
2. After applying `660`: `-rw-rw----`
3. After `g-w`: `-rw-r-----`
4. After `g+w`: `-rw-rw----`
5. After `g-r`: `-rw--w----`

## 10. Security Considerations

- Use the minimum permissions required for each file and directory.
- Verify ownership before relying on group-based permissions.
- Directory execute permission is required to traverse a directory.
- File permissions and directory permissions control different types of access.
- Review permissions after every change to ensure the intended access is configured.

## 11. Conclusion

This stage gave me practical experience creating a shared directory, setting ownership, applying the setgid bit, and changing file permissions using both numeric and symbolic modes.

I also used `ls -ld` and `ls -l` to verify the results of my changes.

**Project:** Linux Access Control and Security Administration  
**Platform:** Ubuntu Linux  
**Documentation stage:** File and Folder Permissions
