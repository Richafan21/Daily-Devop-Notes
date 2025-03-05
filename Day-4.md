# Working with Shell

---

## File Compression and Archival

- **Checking the Size of Files**

Use the command `du` which stands for disk usage.  

### Viewing File Sizes
To display a file size in kilobytes:
```bash
[~]$ du -sk test.img
100000
```
For a human-readable format:
```bash
[~]$ du -sh test.img
98M     test.img
```
Alternatively, use `ls -lh` to view file sizes:
```bash
[~]$ ls -lh test.img
```

### Using `tar` for Archival
The `tar`(Tape Archive) command is commonly used to group multiple files or directories into a single archive file (tarball). This is useful for data backup, transfer, and archiving.

To create a tar archive:
```bash
[~]$ tar -cf test.tar file1 file2 file3
[~]$ ls -ltr test.tar
-rw-rw-r--  1 1281054720 Mar 13 19:48 test.tar
```
List the contents of a tarball:
```bash
[~]$ tar -tf test.tar
./file1
./file2
./file3
```
Extract files from a tar archive:
```bash
[~]$ tar -xf test.tar
```
To create a compressed tarball, include the `-z` option:
```bash
[~]$ tar -zcf test.tar.gz file1 file2 file3
```

### Compression Commands in Linux
Linux offers several commands for compressing files, with different formats and compression levels.

### Common Compression Utilities
| Utility  | Command Example                | File Extension |
|----------|--------------------------------|---------------|
| **bzip2**  | `bzip2 test.img`             | `.bz2`        |
| **gzip**   | `gzip test1.img`             | `.gz`         |
| **xz**     | `xz test2.img`               | `.xz`         |

Each command appends a specific file extension indicating the compression format.

### Decompressing Files
To uncompress these files, use:
- **bzip2**: `bunzip2`
- **gzip**: `gunzip`
- **xz**: `unxz`

Examples:
```bash
# Decompress using bzip2
[~]$ bunzip2 test.img.bz2
[~]$ du -sh test.img
99M     test.img

# Decompress using gzip
[~]$ gunzip test1.img.gz
[~]$ du -sh test1.img
99M     test1.img

# Decompress using xz
[~]$ unxz test2.img.xz
[~]$ du -sh test2.img
99M     test2.img
```

### Viewing Compressed Files Without Decompression
You can read compressed files without manually decompressing them:
- `zcat` for `.gz`
- `bzcat` for `.bz2`
- `xzcat` for `.xz`

---

## Searching for Files and Patterns in Linux

### Using `locate` for Quick File Searches
The `locate` command quickly finds files by querying a pre-built database (`mlocate.db`). This makes searches almost instantaneous.

Example:
```bash
locate City.txt
```
Output:
```
/home/user/Documents/City.txt
/home/user/Projects/Backup/City.txt
```
However, if the file was recently created and doesn't appear in the results, update the database manually:
```bash
sudo updatedb
```
Then rerun `locate`.

---

### Using `find` for Granular Searches
Unlike `locate`, the `find` command searches directories in real time and provides advanced filtering options.

### Find a file by name:
```bash
find /home/user -name "City.txt"
```
This searches for `City.txt` within `/home/user` and all its subdirectories.

### Find files by size:
```bash
find /var/log -size +50M  # Finds files larger than 50MB
```

### Find files modified in the last 7 days:
```bash
find /home/user/Documents -mtime -7
```

---

### Searching Within Files Using `grep`
`grep` is a powerful tool for searching text inside files.

#### Basic search:
```bash
grep "error" logs.txt  # Finds lines containing 'error'
```

### Case-insensitive search:
```bash
grep -i "warning" logs.txt
```

### Recursive search in a directory:
```bash
grep -R "failed login" /var/log
```

### Exclude matching lines:
```bash
grep -v "debug" application.log  # Shows lines that do NOT contain 'debug'
```

### Match whole words:
```bash
grep -w "failed" logs.txt
```

### Display lines before and after a match:
```bash
grep -A2 -B2 "ERROR" logs.txt  # Shows 2 lines before and after each match
```

---

### Summary of Key Commands
| Command | Purpose |
|---------|---------|
| `locate filename` | Quickly finds files using a database |
| `sudo updatedb` | Updates the `locate` database |
| `find /path -name filename` | Searches for a file by name in real time |
| `find /path -size +50M` | Finds files larger than 50MB |
| `grep "pattern" file.txt` | Searches for a text pattern in a file |
| `grep -R "pattern" /dir` | Searches recursively in a directory |

---

## IO Redirection

Every Linux command ran is automaticaly associated with three primary data streams.  

### The Three Primary Data Streams

- **STDIN**: The standard input stream that accepts text input.
- **STDOUT**: The standard output stream that displays text output on your screen.
- **STDERR**: The standard error stream that shows error messages.

Example of using the `cat` command and working:

```sh
[~]$ cat sample_text.txt
This is the file contents
```

If you attempt to access a file that does not exist, the error message is directed to STDERR:

```sh
[~]$ cat sample_text.txt
This is the file contents
cat: sample_text.txt: No such file or directory
```

### Redirecting Output and Errors
To control where output goes, use redirection:

- `>` writes STDOUT to a file (overwriting it).
- `>>` appends STDOUT to a file.
- `2>` writes STDERR to a file.
- `2>>` appends STDERR to a file.

Example:

```sh
[~]$ echo $SHELL > shell.txt  # Save output
[~]$ echo "Using Bash shell" >> shell.txt  # Append message
[~]$ cat shell.txt
/bin/bash
Using Bash shell
```

To capture errors separately:

```sh
[~]$ cat missing_file 2> errors.txt  # Save errors to file
[~]$ cat errors.txt
cat: missing_file: No such file or directory
```

To discard errors, redirect STDERR to `/dev/null`:

```sh
[~]$ cat missing_file 2> /dev/null
```

### Using Pipes (`|`) to Chain Commands
Pipes let you send the output of one command directly to another. For example:

```sh
command1 | command2
```

Instead of saving output to a file and then viewing it, use a pipe:

```sh
[~]$ grep "Hello" sample.txt | less
```

### The `tee` Command: Save and Display Simultaneously
Use `tee` to send output to both a file and the screen:

```sh
[~]$ echo $SHELL | tee shell.txt
/bin/bash
```

To append instead of overwrite:

```sh
[~]$ echo "Another line" | tee -a shell.txt
```

### Quick Reference Table

| Technique | Purpose | Example Command |
|-----------|---------|----------------|
| Redirect STDOUT | Send command output to a file | `echo $SHELL > shell.txt` |
| Append STDOUT | Append command output to an existing file | `echo $SHELL >> shell.txt` |
| Redirect STDERR | Send error messages to a file | `cat missing_file 2> error.txt` |
| Append STDERR | Append error messages to an existing file | `cat missing_file 2>> error.txt` |
| Use Pipes | Pass output from one command as input to another | `grep Hello sample.txt | less` |
| Utilize `tee` Command | Duplicate output to file and screen simultaneously | `echo $SHELL | tee shell.txt; echo $SHELL | tee -a shell.txt` |
