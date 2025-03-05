# Working with Shell

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

## Using `tar` for Archival
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

## Compression Commands in Linux
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
