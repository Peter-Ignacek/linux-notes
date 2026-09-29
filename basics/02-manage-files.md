# Permissions

## Commands

chmod  
chown  

## Example

chmod 755 file.sh  

## Notes

r = read  
w = write  
x = execute  

## Copy Files or Directories

The lesson covered copying files and directories with `cp`.

### `cp`

Copies a file.

General syntax:

```bash
cp source destination
```

Example:

```bash
touch file1 file2
cat > file1
cp file1 file2
cat file2
```

`cp source destination` copies the source file to the destination path. If the destination file already exists, its contents can be overwritten.

### Copying a file into a directory

When the destination is a directory, the copied file keeps its original filename inside that directory.

```bash
mkdir dir1
cp file1 dir1
tree
cat dir1/file1
```

### Copying directories recursively

Use `-r` (recursive) when copying directories and their contents.

```bash
cp -r dir1 dir2
```

This creates a copy of `dir1` named `dir2`, including its contents. Verify the result with `tree`.

### Useful verification commands

Use the following commands to verify files, sizes, structure, and copied contents:

```bash
ls -l
tree
cat filename
```

### Practical examples

```bash
cp file1 file2
cp file1 dir1
cp -r dir1 dir2
```

| Command | Purpose |
| --- | --- |
| `cp file1 file2` | Copy one file to another file |
| `cp file1 dir1` | Copy a file into a directory |
| `cp -r dir1 dir2` | Copy a directory recursively |

> Be careful when copying to an existing destination file, because `cp` can overwrite it.

## Move or Rename a File

The lesson covered using `mv` both to move files and to rename them.

### `mv`

General syntax:

```bash
mv source destination
```

### Moving a file into a directory

When the destination is a directory, `mv` moves the source file into that directory.

```bash
mv file2 dir2
tree
```

### Renaming a file

If the destination is another filename in the same directory, the file is renamed.

```bash
cd dir2
ls
mv file1 file3
ls
```

`mv` is used both for moving and renaming.

### Practical examples

```bash
mv file2 dir2
cd dir2
ls

mv file1 file3
ls
```

| Command | Purpose |
| --- | --- |
| `mv file1 dir1` | Move a file into a directory |
| `mv oldname newname` | Rename a file |
| `mv source destination` | General move/rename syntax |

> `mv` does not create a copy. The original source path disappears after a successful move.

## Change Directory

The lesson covered navigating between directories using `cd`.

### `cd`

General syntax:

```bash
cd path
```

### Change into a directory

```bash
cd tmp
pwd
```

Example result:

```text
/tmp
```

### Go to the parent directory

Use `cd ..` to move one directory level up.

> `..` refers to the parent directory.

The command and argument require a space.

### Change to an absolute path

Navigate directly to a directory using its full path:

```bash
cd /root/dir1
```

A path beginning with `/` is an absolute path and starts from the root directory.

### Change between unrelated directories

`cd` can move directly between directories when a valid path is provided:

```bash
cd /tmp
cd /root/dir1
```

### Root directory

From `/tmp`, `cd ..` can move to `/`. From there, `cd root` enters `/root`.

### Useful verification commands

Use `pwd` and `ls` to confirm the current location and directory contents.

### Practical examples

```bash
cd /tmp
pwd

cd ..
pwd

cd /root/dir1
pwd

cd /tmp
cd /root/dir1
```

| Command | Purpose |
| --- | --- |
| `cd directory` | Enter a directory |
| `cd ..` | Move one level up |
| `cd /absolute/path` | Move directly to an absolute path |
| `pwd` | Show the current directory |

> Directory names and paths are case-sensitive in Linux.

## Search and Inspect Files

The lesson covered finding files with `find`, comparing file contents with `diff`, and identifying file types with `file`.

### `find`

`find` searches for files and directories.

General syntax:

```bash
find PATH OPTION VALUE
```

Common options include:

| Option | Purpose |
| --- | --- |
| `-name` | Search by file or directory name |
| `-user` | Search for files owned by a specific user |
| `-group` | Search for files belonging to a specific group |

Search by name from the current directory:

```bash
find . -name file1
```

`.` means the current directory and everything below it. `-name file1` searches for entries named exactly `file1`.

Example output:

```text
./file1
./dir1/file1
```

To search from the filesystem root:

```bash
find / -name file1
```

`find / ...` searches from the root of the filesystem, while `find . ...` searches only from the current directory downward. Searching large system paths may produce `Permission denied` messages for locations the current environment cannot access. This does not necessarily mean that `find` itself failed.

### `diff`

`diff` compares the contents of two files and shows their differences.

General syntax:

```bash
diff file1 file2
```

Example:

```bash
diff file1 file3
```

In the output, lines beginning with `<` come from the first file and lines beginning with `>` come from the second file. If the files are identical, `diff` normally produces no output.

Example output:

```text
1c1
< first-file-content
---
> second-file-content
```

On minimal systems, some utilities may need to be installed separately.

### `file`

`file` identifies the type of a file or filesystem object.

General syntax:

```bash
file filename
```

Examples:

```bash
file file1
file dir1
```

Possible results include:

```text
file1: ASCII text
dir1: directory
```

`file` can identify different object types such as regular text files, directories, symbolic links, and device files.

### Practical examples

```bash
find . -name file1
find / -name file1

diff file1 file2

file file1
file dir1
```

| Command | Purpose |
| --- | --- |
| `find . -name file1` | Search for `file1` from the current directory |
| `find / -name file1` | Search for `file1` from filesystem root |
| `find PATH -user USER` | Search by owner |
| `find PATH -group GROUP` | Search by group |
| `diff file1 file2` | Compare contents of two files |
| `file filename` | Identify the type of a file/object |

> Use `find . ...` when you only need to search the current working tree. Searching from `/` is much broader and may be slower and produce permission errors.

## Search a Word in a File with `grep`

`grep` (Global Regular Expression Print) searches for a pattern and prints matching lines. It can search files or filter command output.

### `grep`

General syntax:

```bash
grep PATTERN filename
```

For example, search a log file for lines containing `error`:

```bash
grep error logfile.txt
```

### Case sensitivity

`grep` is case-sensitive by default. Use `-i` to ignore case:

```bash
grep permit /etc/ssh/sshd_config
grep -i permit /etc/ssh/sshd_config
```

### Search command output with a pipe

With a pipe (`|`), the output of one command can be passed into `grep`. This filters the `ls -l` output to show lines containing `environment`:

```bash
ls -l | grep environment
```

Searching for `d` matches any line containing that letter, so it does not reliably select only directories. In `ls -l` output, directory entries start with `d`; use `^d` to match lines beginning with `d`:

```bash
ls -l | grep ^d
```

`^` anchors the pattern to the beginning of the line. For example, `^a` matches lines that begin with the letter `a`:

```bash
grep ^a filename
```

### Patterns containing spaces

Patterns containing spaces should be quoted:

```bash
grep "PermitRootLogin prohibit-password" /etc/ssh/sshd_config
```

### Practical examples

```bash
grep error logfile.txt
grep -i permit /etc/ssh/sshd_config
ls -l | grep environment
ls -l | grep ^d
grep ^a filename
grep "multi word pattern" filename
```

| Command | Purpose |
| --- | --- |
| `grep word file` | Search for a word/pattern in a file |
| `grep -i word file` | Case-insensitive search |
| `command \| grep word` | Filter command output |
| `grep ^pattern file` | Match a pattern at the start of a line |
| `grep "multiple words" file` | Search for a pattern containing spaces |

### Pipe `|`

> `|` sends the output of the command on the left into the command on the right.

For example, `ls -l | grep ^d` filters the output of `ls -l` to show directory lines.

## Replace Text in a File with `sed`

`sed` (stream editor) searches text and can replace it in command output. By default, it does not change the original file.

### `sed`

General substitution syntax:

```bash
sed 's/old_text/new_text/' filename
```

Here, `s` means substitute, `old_text` is the search text, and `new_text` is the replacement. The transformed result is printed to the terminal, while the original file remains unchanged. Without extra flags, only the first matching occurrence on each line is replaced.

```bash
sed 's/ansible/linux/' file1
cat file1
```

### Replace all matches with `g`

`g` means global and replaces all matching occurrences on each line:

```bash
sed 's/ansible/linux/g' file1
```

Without `g`, only the first match on each line is replaced.

### Ignore case with `i`

`i` enables case-insensitive matching. Combined with `g`, all matches on each line are replaced regardless of letter case, such as `ansible`, `Ansible`, or `ANSIBLE`.

```bash
sed 's/ansible/linux/ig' file1
```

### Modify the original file with `-i`

`-i` means in-place editing and writes the changes into the original file.

```bash
sed -i 's/ansible/linux/' file1
sed -i 's/ansible/linux/g' file1
sed -i 's/ansible/linux/ig' file1
```

> Without `-i`, `sed` only prints the transformed output. With `-i`, it writes the changes into the file.

### Print a selected range of lines

Use `-n` to suppress normal automatic output, then `p` to print only the selected range. A filename or piped input is needed; without one, `sed` waits for standard input.

General syntax:

```bash
sed -n 'START,ENDp' filename
```

For example, print lines 5 through 10:

```bash
sed -n '5,10p' file1
```

### Remove selected lines from output

`d` deletes the selected lines from the displayed output. Without `-i`, the original file is not changed.

```bash
sed 'START,ENDd' filename
sed '5,10d' file1
```

### Replace another character or pattern

`sed` can replace any matching text pattern, not only whole words. This example replaces the first `#` on each line with a space in the output:

```bash
sed 's/#/ /' file1
```

### Practical examples

```bash
sed 's/old/new/' file.txt
sed 's/old/new/g' file.txt
sed 's/old/new/ig' file.txt

sed -i 's/old/new/' file.txt
sed -i 's/old/new/g' file.txt

sed -n '5,10p' file.txt
sed '5,10d' file.txt
```

| Command | Purpose |
| --- | --- |
| `sed 's/old/new/' file` | Replace first match on each line in output |
| `sed 's/old/new/g' file` | Replace all matches on each line |
| `sed 's/old/new/ig' file` | Replace all matches, case-insensitive |
| `sed -i 's/old/new/' file` | Modify the original file in place |
| `sed -n '5,10p' file` | Print only lines 5–10 |
| `sed '5,10d' file` | Omit lines 5–10 from output |

### Safety note

> Be careful with `sed -i`, because it modifies the original file directly. Testing the same expression without `-i` first is a useful way to preview the result.
