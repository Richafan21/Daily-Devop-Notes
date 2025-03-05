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

