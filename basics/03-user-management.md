# Users

## Commands

whoami  
id  
adduser  
passwd  

## Notes

- users stored in /etc/passwd

## Lesson 22: Creating & Managing a User

### Linux User Types

#### Super / root user

- Most powerful user and administrator account
- Example: `root`
- Home directory: `/root`
- Typical shell: `/bin/bash`

#### System user

- Created for software, applications, and services
- Examples: `ftp`, `ssh`, `apache`
- May use a service-specific home directory and a non-login shell such as `/usr/sbin/nologin`

#### Normal user

- Regular account created by the administrator or root user
- Examples: `visitor`, `ec2-user`
- Typical home directory: `/home/username`
- Typical shell: `/bin/bash`

| Type | Example | Home directory | Typical shell |
| --- | --- | --- | --- |
| Super user | `root` | `/root` | `/bin/bash` |
| System user | `ftp`, `ssh`, `apache` | Service-specific | `/usr/sbin/nologin` |
| Normal user | `visitor`, `ec2-user` | `/home/username` | `/bin/bash` |

### Identify the current user

Use `whoami` to show the current user:

```bash
whoami
```

`id username` shows identity and group information. For example:

```bash
id root
```

```text
uid=0(root) gid=0(root) groups=0(root)
```

- UID = User ID
- GID = primary Group ID
- UID 0 belongs to `root`

### `/etc/passwd`

`/etc/passwd` contains user account information. A simplified entry has these fields:

```text
username:x:UID:GID:comment:home:shell
```

The fields are username, password placeholder (`x`), UID, GID, comment or user information, home directory, and login shell. Root commonly uses `/root` and `/bin/bash`; system users may use `/usr/sbin/nologin`; normal users commonly use `/home/username`.

Inspect the file with:

```bash
cat /etc/passwd
```

### What happens when a user is created

A user account receives UID/GID information and an entry in `/etc/passwd`. A home directory may need to be explicitly created depending on the command, options, and system defaults. On the Ubuntu lab system, plain `useradd username` did not necessarily create one.

### `useradd`

General syntax:

```bash
useradd [options] username
```

Options shown in the course:

| Option | Purpose |
| --- | --- |
| `-u` | Set user ID |
| `-G` | Set supplementary groups |
| `-g` | Set primary group |
| `-d` | Set home directory |
| `-c` | Set comment/user information |
| `-s` | Set login shell |
| `-m` | Create the user's home directory |

Create a user:

```bash
useradd john
```

Usernames must be unique; `useradd` reports an error if the account already exists.

To create a user with a home directory, use `-m`:

```bash
useradd -m mark
id mark
```

### Check and delete a user

Check a user's UID, GID, and groups with:

```bash
id john
id mark
```

`userdel username` removes the user account:

```bash
userdel john
```

### `/etc/group` and group membership

`/etc/group` contains group information. A simplified entry has this format:

```text
group_name:x:GID:members
```

The fields are group name, password placeholder, GID, and group members. Inspect the file with:

```bash
cat /etc/group
```

The `-g` option sets a primary group; `-G` sets supplementary groups. The lab used `usermod -G` and checked the result with `id`:

```bash
usermod -G john mark
id mark
```

`usermod -G GROUP USER` sets the user's supplementary group list. Without `-a`, it replaces the existing supplementary groups. Practical note beyond the exact lab command: use `-aG` to add a group without replacing the list:

```bash
usermod -aG john mark
```

### Practical examples

```bash
whoami
id root

cat /etc/passwd
cat /etc/group

useradd john
useradd -m mark

id john
id mark

userdel john

usermod -G john mark
id mark
```

Practical note for adding a supplementary group without replacing existing ones:

```bash
usermod -aG john mark
```

| Command / File | Purpose |
| --- | --- |
| `whoami` | Show current user |
| `id user` | Show UID, GID, and groups |
| `/etc/passwd` | User account information |
| `/etc/group` | Group information |
| `useradd username` | Create a user |
| `useradd -m username` | Create a user with a home directory |
| `userdel username` | Delete a user |
| `usermod -G group user` | Set supplementary groups |
| `usermod -aG group user` | Add a supplementary group without replacing existing ones |

## Set a Password and Login as a User

The lesson demonstrated setting a password for an existing user and then logging in as that user.

### `passwd`

Use `passwd username` to set or change a user's password:

```bash
passwd username
```

Example:

```bash
passwd john
```

The system prompts for a new password and asks for it to be entered again. Password input is normally not displayed on the terminal while typing.

```text
New password:
Retype new password:
passwd: password updated successfully
```

### Login as another user

After logging in as `john`, verify the current directory with `pwd`. A normal user's home directory is typically `/home/username`.

Example:

```text
/home/john
```

### Verify the logged-in user

Use these commands to check the active user and session:

```bash
whoami
pwd
id
```

- `whoami` shows the current username.
- `pwd` shows the current working directory.
- `id` shows UID, GID, and group memberships.

### Practical example

```bash
passwd john

# after logging in as john
whoami
pwd
id
```

Expected home directory:

```text
/home/john
```

| Command | Purpose |
| --- | --- |
| `passwd username` | Set or change a user's password |
| `whoami` | Verify the current user |
| `pwd` | Verify the current directory |
| `id` | Show UID, GID, and group memberships |

> Never store real passwords in notes, scripts, screenshots, or Git repositories.

## `ls -l` Explained

Use `ls -l` to display detailed information about files:

```bash
ls -l
```

Example:

```text
-rw-r--r-- 1 root root 122 May 7 20:29 file1
```

The fields, in order, are file type and permissions, hard link count, owner, owner's primary group, file size, modification date and time, and filename.

### File type symbol

The first character identifies the object type:

| Symbol | Type |
| --- | --- |
| `-` | Regular file |
| `b` | Block device |
| `c` | Character device |
| `d` | Directory |
| `l` | Symbolic link |

## File Permissions

Permissions are applied at three levels:

1. Owner / user
2. Group
3. Others

The permission string starts with the file type, followed by three permission triplets: owner, group, and others.

The permission symbols are:

| Symbol | Meaning |
| --- | --- |
| `r` | Read |
| `w` | Write or modify |
| `x` | Execute |

### File and directory permissions

Permission meanings depend on whether the object is a file or a directory:

| Permission | File | Directory |
| --- | --- | --- |
| `r` | Read/open file contents | List directory contents |
| `w` | Write/edit file contents | Add, delete, or rename entries in the directory |
| `x` | Execute/run a file or script | Enter or traverse the directory, for example with `cd` |

### Permission string structure

In a permission string such as `-rwxrw-r--`, the first character is the file type, followed by three permission triplets:

```text
- | rwx | rw- | r--
    user  group others
```

- First character: file type
- Next three: owner permissions
- Next three: group permissions
- Last three: others permissions

A lab example was `-rw-rw-r--`:

```text
- | rw- | rw- | r--
    user  group others
```

This represents a regular file where the owner can read and write, the group can read and write, and others can read only. A `-` inside a permission triplet means that permission is not granted.

### Lab example: ownership and permissions

The lab created a file as user `john` and inspected it with `ls -l`:

```bash
touch file1
ls -l
```

Example output:

```text
-rw-rw-r-- 1 john john 0 ... file1
```

Here the owner is `john`, the owner's primary group is `john`, and the permissions are `rw-` for the owner, `rw-` for the group, and `r--` for others.

### Group access example

A file created by `john` in `/tmp` belonged to owner `john` and group `john`, with permissions like `-rw-rw-r--`. The earlier lab added `mark` to supplementary group `john`; `id mark` showed primary group `mark` and supplementary group `john`.

If a user belongs to the file's group, the group permission bits apply to that user. Therefore, `mark` could read the file created by `john` through the group's read permission.

### Directory permission example

Use `ls -ld` to inspect a directory entry itself:

```bash
ls -ld /home/john
```

The lab showed a directory owned by `john` whose permissions did not grant access to other users. Another user could not enter it:

```bash
cd /home/john
```

The practical reason is that a directory needs execute (`x`) permission for a user to enter or traverse it.

### Useful commands

| Command | Purpose |
| --- | --- |
| `ls -l` | Show detailed file information |
| `ls -ld directory` | Show the directory entry itself, not its contents |
| `id` | Show UID, GID, and group memberships |
| `whoami` | Show current user |
| `pwd` | Show current directory |

### Important notes

- Permissions are evaluated for owner, group, and others.
- `-` inside a permission triplet means that permission is not granted.
- Ownership and permissions are separate concepts.
- Group membership can grant access through the group permission bits.
- `x` on a directory means traverse or enter permission.
- Use `ls -l` and `ls -ld` to inspect permissions and ownership.

## Changing Permissions with `chmod`

Linux permissions can be changed using the symbolic method or the numeric (absolute) method.

### Symbolic method

General syntax:

```bash
chmod [who][operator][permissions] file
```

Who:

| Symbol | Meaning |
| --- | --- |
| `u` | User / owner |
| `g` | Group |
| `o` | Others |

Operators:

| Symbol | Meaning |
| --- | --- |
| `+` | Add permission |
| `-` | Remove permission |
| `=` | Set exact permissions |

Permissions:

| Symbol | Meaning |
| --- | --- |
| `r` | Read |
| `w` | Write |
| `x` | Execute |

Examples:

```bash
chmod u+x file1
```

This adds execute permission for the owner, for example changing `-rw-rw-r--` to `-rwxrw-r--`.

```bash
chmod u+x,g-w file2
```

This adds execute permission for the owner and removes write permission from the group.

```bash
chmod u+x,g-w,o=r file1
chmod u+x,g-w,o=x file1
```

In the first command, the owner gets execute, the group loses write, and others are set to read only. In the second, others are set to execute only.

### Numeric / absolute method

Numeric permissions use these values, which are added together:

| Permission | Value |
| --- | --- |
| `r` | 4 |
| `w` | 2 |
| `x` | 1 |

| Number | Permissions |
| --- | --- |
| `0` | `---` |
| `1` | `--x` |
| `2` | `-w-` |
| `3` | `-wx` |
| `4` | `r--` |
| `5` | `r-x` |
| `6` | `rw-` |
| `7` | `rwx` |

The three digits set permissions for owner, group, and others, in that order:

```text
chmod 754 file
      │││
      ││└─ others
      │└── group
      └─── owner
```

#### Numeric examples

```bash
chmod 666 file1
```

Result: `-rw-rw-rw-` — owner, group, and others can all read and write.

```bash
chmod 755 file1
```

Result: `-rwxr-xr-x` — owner has read, write, and execute; group and others have read and execute.

```bash
chmod 777 file1
```

Result: `-rwxrwxrwx`.

> `777` gives read, write, and execute permissions to everyone and should generally be used with caution.

```bash
chmod 000 file1
```

Result: `----------`. `000` removes all permissions for owner, group, and others; reading the file with `cat file1` then returns `Permission denied`.

Restore read access for everyone and write access for the owner with:

```bash
chmod 644 file1
```

Result: `-rw-r--r--` — owner can read and write; group and others can read.

### Directory permissions with `chmod`

Inspect the directory itself with:

```bash
ls -ld /home/john
```

For `chmod 710 /home/john`, the digits mean:

```text
710
│││
││└─ others = ---
│└── group = --x
└─── owner = rwx
```

The owner has full permissions, the group has execute/traverse only, and others have no permissions. A user without the needed directory access may receive a permission error when trying to enter it.

`755` gives `rwxr-xr-x`: the owner has full permissions, while group and others have read and execute. `770` gives `rwxrwx---`: owner and group have full permissions, while others have none.

### Common numeric permissions

| Mode | Permissions | Common meaning |
| --- | --- | --- |
| `644` | `rw-r--r--` | Common regular file permissions |
| `666` | `rw-rw-rw-` | Read/write for everyone |
| `700` | `rwx------` | Private owner-only access |
| `710` | `rwx--x---` | Owner full, group traverse |
| `755` | `rwxr-xr-x` | Common executable/directory mode |
| `770` | `rwxrwx---` | Owner and group full access |
| `777` | `rwxrwxrwx` | Full access for everyone |
| `000` | `---------` | No permissions |

### Practical examples

```bash
chmod u+x file1
chmod g-w file1
chmod o=r file1

chmod 644 file1
chmod 755 file1
chmod 777 file1
chmod 000 file1

chmod 710 /home/john
chmod 755 /home/john
chmod 770 /home/john
```

### Important notes

- Use `ls -l` to inspect file permissions and `ls -ld directory` to inspect the directory itself.
- Symbolic mode changes selected permissions; numeric mode sets the complete permission set.
- Be careful with `777` and `000`; `000` can block access.
- Execute (`x`) permission on a directory controls traversal/access.

## Changing Ownership with `chown`

The lesson covered changing the owner and group of files.

### `chown`

General syntax:

```bash
chown owner file
```

To change the owner and group together:

```bash
chown owner:group file
```

`chown` changes file ownership. Specifying `owner:group` changes both the owner and group.

### Lab example

Example files had different owners and groups:

```text
-rw-r--r-- 1 john john ... file1
-rw-r--r-- 1 root root ... file2
-rw-rw-r-- 1 mark mark ... file3
```

As root, the lesson changed ownership with:

```bash
chown john:john file1
chown root:root file2
chown mark:mark file3
```

Use `ls -l` to check the permissions, owner, and group afterward.

> In `ls -l`, the owner and group are shown after the hard-link count.

### Changing only the owner

If no group is specified, `chown owner file` changes only the owner. The existing group remains unchanged:

```bash
chown john file3
```

Possible result:

```text
-rw-rw-r-- 1 john mark ... file3
```

### Permission requirement

Changing file ownership generally requires root or appropriate administrative privileges. The lab's attempt as user `john` failed with `Operation not permitted`; the ownership changes succeeded when performed as root.

### Ownership and permissions

`chown` changes the owner or group; `chmod` changes permissions. These are separate operations:

```bash
chown john:john file1
chmod 644 file1
```

## `file` Command Reminder

Use `file filename` to identify the type of a file or filesystem object.

```bash
file filename
```

### Practical examples

```bash
ls -l

chown john file3
chown john:john file1
chown root:root file2
chown mark:mark file3

ls -l
```

| Command | Purpose |
| --- | --- |
| `chown user file` | Change file owner |
| `chown user:group file` | Change owner and group |
| `ls -l` | Verify owner, group, and permissions |
| `chmod ...` | Change permissions, not ownership |
| `file filename` | Identify file type |

### Important notes

- `chown user file` changes only the owner.
- `chown user:group file` changes both owner and group.
- Changing ownership usually requires root or administrative privileges.
- Ownership and permissions are separate concepts.
- Use `ls -l` to verify the result.

## Abschnitt 4 Summary

| Command / File | Purpose |
| --- | --- |
| `whoami` | Show current user |
| `id user` | Show UID, GID, and groups |
| `/etc/passwd` | User account information |
| `/etc/group` | Group information |
| `useradd` | Create a user |
| `useradd -m` | Create user with home directory |
| `userdel` | Delete a user |
| `usermod -aG` | Add user to supplementary group |
| `passwd user` | Set or change user password |
| `ls -l` | Show ownership and permissions |
| `ls -ld dir` | Show permissions of the directory itself |
| `chmod` | Change permissions |
| `chown` | Change owner or group |
| `file` | Identify file type |

Permission cheat sheet:

```text
r = 4
w = 2
x = 1

7 = rwx
6 = rw-
5 = r-x
4 = r--
0 = ---
```

Examples:

```bash
chmod 644 file
chmod 755 file
chmod 770 directory
chown user:group file
```
