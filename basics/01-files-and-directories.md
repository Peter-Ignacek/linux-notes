# File System

## Important directories

/home - user files  
/etc - config files  
/var - logs  

<img width="945" height="541" alt="image" src="https://github.com/user-attachments/assets/082e1dc2-9adb-4dd2-bf2d-afd30ad411e6" />
<img width="945" height="529" alt="image" src="https://github.com/user-attachments/assets/39eac48c-9891-47f0-a60c-6c776d717ff1" />
<img width="945" height="528" alt="image" src="https://github.com/user-attachments/assets/e68ffd17-965a-4a6f-8d37-ee94c843f48f" />


## Commands

pwd - show current directory  
ls - list files  
ls -la - list with hidden files  
cd - change directory  

## Notes

- / = top Directory = C:\of windows
- /<ROOT_GUARDIAN> = directory for <ROOT_GUARDIAN> user = C:\Documents and Settings\Administrator   
- /home = home directory for other users = c:\Documents and Settings\username
- /usr = default softwaresare installed in = c:\program files
- /bin = it contains commands used by all users
(Binary files)
- /sbin = contains commands used by only Super User (root)
(Super user's binary files)
- /etc = configs
- /var/log = logs


# 1. Basic Linux Commands

## Basic Linux Commands

### `date`

Shows the current system date and time.

```bash
date
```

### `cal`

Shows the current month's calendar.

```bash
cal
```

### `uptime`

Shows how long the system has been running, along with logged-in users and load averages.

```bash
uptime
```

### `whoami`

Shows the currently logged-in username.

```bash
whoami
```

### `finger`

Displays information about users.

```bash
finger
```

On the Ubuntu lab system, `finger` was not installed by default and had to be installed with:

```bash
apt install finger
```

Some utilities may require installation of an additional package.

### `id`

Shows identity information including:

- UID
- GID
- group memberships

```bash
id
```

Example from the root user:

```ini
uid=0(root) gid=0(root) groups=0(root)
```

UID 0 is the root user.

### `who`

Shows users currently logged into the system.

The output can contain the username, terminal or TTY, and login date and time.

```bash
who
```

### `w`

Shows logged-in users together with additional system and activity information, such as uptime, load average, terminal, login time, idle time, and current activity.

```bash
w
```

### `man`

Shows the manual page for a command.

General syntax:

```bash
man <command>
```

Example:

```bash
man who
```

Man pages commonly contain sections such as `NAME`, `SYNOPSIS`, and `DESCRIPTION`. They also document available options and flags.

| Command | Purpose |
| --- | --- |
| `date` | Current date and time |
| `cal` | Calendar |
| `uptime` | System uptime and load information |
| `whoami` | Current username |
| `finger` | User information |
| `id` | UID, GID and groups |
| `who` | Logged-in users |
| `w` | Logged-in users and activity |
| `man <command>` | Command manual |

# 2. Read a File / View Files

## Read a File / View Files

### `ls`

Lists files and directories.

```bash
ls
```

To list the contents of another directory, provide its path:

```bash
ls /
```

### `cat`

Displays the contents of a file directly in the terminal.

```bash
cat filename
```

Example:

```bash
cat example.txt
```

`cat` expects a file path. Giving it an invalid path or a directory name from the wrong working directory results in an error.

### `less`

Views a file interactively and is useful for longer files.

```bash
less filename
```

### `more`

Displays a file page by page.

```bash
more filename
```

### `head`

Displays the beginning of a file. By default it shows the first 10 lines.

```bash
head filename
```

To display the first 20 lines:

```bash
head -n 20 filename
```

The commonly supported short form is:

```bash
head -20 filename
```

Do not use `head 20 filename`; `20` is interpreted as a filename.

### `tail`

Displays the end of a file. By default it shows the last 10 lines.

```bash
tail filename
```

To display the last 20 lines:

```bash
tail -n 20 filename
```

## Create a File

The lesson covered several ways to create files.

### `touch`

Creates an empty file.

General syntax:

```bash
touch filename
```

Example:

```bash
touch file1.txt
```

If the file does not already exist, `touch` creates it as an empty file. Its size is `0`, which can be seen with:

```bash
ls -l
```

Example output:

```text
-rw-r--r-- 1 root root 0 Sep 28 22:48 file1.txt
```

### Useful `ls` combinations

The following combinations were used to list files and sort them by modification time:

```bash
ls -l
ls -l -t
ls -lt
ls -ltr
```

`ls -ltr` uses long format, sorts by modification time, and reverses the order.

### `cat > filename`

Creates a file and lets the user type content into it directly from the terminal.

General syntax:

```bash
cat > filename
```

Example:

```bash
cat > file2
```

After entering neutral placeholder text, finish the input with `Ctrl+C`. The file can then be listed and viewed with:

```bash
ls -ltr
less file2
```

Linux filenames are case-sensitive: `File3` and `file3` are different names.

For example, if a file was created as `File3`, then `less file3` or `cat file3` does not refer to the same file. Use the exact filename:

```bash
less File3
cat File3
```

### `nano`

`nano` can create a new file if the specified file does not already exist and opens it in an editor.

General syntax:

```bash
nano filename
```

Example:

```bash
nano File3
```

### `vi`

`vi` can also create a new file if the file does not already exist and opens it in the `vi` editor.

General syntax:

```bash
vi filename
```

Example:

```bash
vi File3
```

| Command | Purpose |
| --- | --- |
| `touch filename` | Create an empty file |
| `cat > filename` | Create a file and enter content from the terminal |
| `nano filename` | Open/create a file in Nano |
| `vi filename` | Open/create a file in Vi |

Practical examples:

```bash
touch file1.txt
ls -l
ls -lt
ls -ltr
cat > file2
less file2
nano File3
vi File3
```

The commonly supported short form is:

```bash
tail -20 filename
```

### `page`

The course slide mentioned `page filename` for displaying a file page by page. On the Ubuntu lab system, the command was not available by default.

> `page` may not be available on a minimal Ubuntu installation. `less` or `more` can be used to view long files.

| Command | Purpose |
| --- | --- |
| `ls` | List directory contents |
| `cat filename` | Display the complete file contents |
| `less filename` | View a file interactively |
| `more filename` | Display a file page by page |
| `head filename` | Display the first 10 lines |
| `tail filename` | Display the last 10 lines |
| `page filename` | Page-by-page viewer; may not be installed |

Practical examples:

```bash
ls /
cat filename
less filename
more filename
head filename
head -n 20 filename
tail filename
tail -n 20 filename
```

## Edit or Append Content to a File

The lesson covered the difference between overwriting a file and appending new content.

### `cat > filename`

Using a single `>` redirects input into a file.

General syntax:

```bash
cat > filename
```

The user can then type text directly into the terminal. If the file already exists, `>` overwrites its previous contents.

> `>` replaces the existing file contents.

Example:

```bash
cat > file1.txt
New content
```

Interactive input from `cat` can be terminated from the terminal after entering the desired content.

### `cat >> filename`

Using `>>` appends new content to the end of an existing file instead of replacing it.

General syntax:

```bash
cat >> filename
```

Example:

```bash
cat >> file.txt
Additional line
```

The existing contents remain and the new line is added at the end. Verify the result with:

```bash
cat file.txt
cat file1.txt
```

| Operator | Behavior |
| --- | --- |
| `>` | Overwrite file contents |
| `>>` | Append to existing file contents |

### Editing with `nano`

Nano can be used to create or edit text files interactively.

```bash
nano File4
```

### Editing with `vi`

Vi can be used to create or edit files.

General syntax:

```bash
vi filename
```

### Practical examples

```bash
cat > file1.txt
cat file1.txt

cat >> file1.txt
cat file1.txt

nano File4
vi File4
```

| Command | Purpose |
| --- | --- |
| `cat > filename` | Write to a file and overwrite existing contents |
| `cat >> filename` | Append content to the end of a file |
| `cat filename` | Verify/read file contents |
| `nano filename` | Edit a file with Nano |
| `vi filename` | Edit a file with Vi |

## Create Directories

The lesson covered creating directories and navigating between them.

### `mkdir`

Creates a directory.

General syntax:

```bash
mkdir directory_name
```

Example:

```bash
mkdir dir1
```

Afterwards, `ls` showed the new directory. `mkdir` also accepts multiple directory names in one command:

```bash
mkdir dir2 dir3 dir4
```

### Checking directories with `ls -l`

Use `ls -l` to verify that a directory was created. In the output, a leading `d` indicates a directory, for example `drwxr-xr-x`.

```bash
ls -l
```

### Entering a directory with `cd`

Navigate into a directory with `cd` and verify the current location with `pwd`.

```bash
cd dir1
pwd
```

Example path:

```text
/root/dir1
```

### Creating directories inside another directory

While inside `dir1`, create and verify additional directories with:

```bash
mkdir dir2 dir3 dir4
ls
ls -l
```

### Going back to the parent directory

Use `cd ..` to move one directory level up.

> `..` refers to the parent directory.

The command and argument must be separated by a space.

| Command | Purpose |
| --- | --- |
| `mkdir dir1` | Create one directory |
| `mkdir dir2 dir3 dir4` | Create multiple directories |
| `cd dir1` | Enter a directory |
| `cd ..` | Move to the parent directory |
| `pwd` | Show the current directory |
| `ls -l` | Verify directories in long format |

Practical examples:

```bash
mkdir dir1
ls -l
cd dir1
pwd

mkdir dir2 dir3 dir4
ls -l

cd ..
pwd
```

## Remove Files and Directories

The lesson covered removing files, removing empty directories, and removing directories recursively.

### `tree`

Displays the current directory structure and is useful for visually checking directory contents before and after changes.

```bash
tree
```

### `rm`

Removes files.

General syntax:

```bash
rm filename
```

Example:

```bash
rm file.txt
```

`rm` can accept multiple filenames in one command:

```bash
rm File1 File3 file2
```

### `rm -f`

The `-f` flag means force. It suppresses prompts and commonly ignores nonexistent files. Use it carefully.

```bash
rm -f file1.txt
```

### `rmdir`

Removes an empty directory.

General syntax:

```bash
rmdir directory_name
```

Example:

```bash
rmdir dir2
```

> `rmdir` only removes empty directories.

For example, `rmdir dir1` fails if `dir1` still contains subdirectories:

```yaml
rmdir: failed to remove 'dir1': Directory not empty
```

### `rm -rf`

Removes a directory recursively, including its contents.

General syntax:

```bash
rm -rf directory_name
```

Example:

```bash
rm -rf dir1
```

The flags mean:

- `-r` = recursive
- `-f` = force

> `rm -rf` can permanently remove an entire directory tree without confirmation. Use it very carefully.

### Wildcards

`*` is a wildcard matching many entries in the current directory. Combining it with `rm -rf` can delete almost everything in the current directory.

### Practical examples

```bash
tree

rm file.txt
rm File1 File3 file2
rm -f file1.txt

rmdir dir2

rm -rf dir1

tree
```

| Command | Purpose |
| --- | --- |
| `rm filename` | Remove a file |
| `rm file1 file2` | Remove multiple files |
| `rm -f filename` | Force file removal |
| `rmdir dirname` | Remove an empty directory |
| `rm -rf dirname` | Recursively remove a directory and its contents |
| `tree` | Display directory structure |

### Safety notes

- `rm` does not normally move files to a recycle bin.
- Deleted files may be difficult or impossible to recover.
- `rm -rf` should be used with particular care.
- When unsure, verify the current directory with `pwd` and inspect contents with `ls` or `tree` before destructive commands.
