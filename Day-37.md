# Linux File Permissions Exercises


## Recap of File Permissions

In Linux, file permissions are represented like this:

```
-rwxrwxrwx
```

- **1st character**: File type  
  - `-` → regular file  
  - `d` → directory  
  - `l` → symbolic link  
- **Next 9 characters**: Permissions split into 3 groups:
  - `rwx` (Owner)
  - `rwx` (Group)
  - `rwx` (Others)

| Symbol | Meaning            |
|--------|--------------------|
| `r`    | Read permission    |
| `w`    | Write permission   |
| `x`    | Execute permission |


## View Permissions

```bash
ls -l filename
```

Displays file type, permissions, owner, group, size, and modification time.


## Changing Permissions

## Symbolic Mode

```bash
chmod [who][+,-,=][permissions] filename
```

| Who  | Meaning     |
|------|-------------|
| `u`  | user (owner)|
| `g`  | group       |
| `o`  | others      |
| `a`  | all         |

| Operator | Meaning              |
|----------|----------------------|
| `+`      | Add permission       |
| `-`      | Remove permission    |
| `=`      | Set exact permission |

### Examples

```bash
chmod u+x script.sh       # Add execute permission for user
chmod go-w file.txt       # Remove write permission for group & others
chmod a=r file.txt        # Read-only for everyone
```

---

## Numeric Mode

| Value | Permission |
|-------|------------|
| `4`   | read       |
| `2`   | write      |
| `1`   | execute    |

- Add values to combine:  
  `7 = 4 + 2 + 1` → `rwx`

### Examples

```bash
chmod 755 script.sh   # rwxr-xr-x
chmod 644 file.txt    # rw-r--r--
```

---

## Changing Ownership

```bash
chown user:group filename
```

### Example

```bash
chown john:users file.txt
```

---

## Special Permissions

| Permission | Octal | Description                                                 |
|------------|-------|-------------------------------------------------------------|
| setuid     | 4000  | Executes file with owner's privileges                       |
| setgid     | 2000  | Executes file with group's privileges (or inherits group)   |
| sticky bit | 1000  | Prevents others from deleting files in shared directories   |

### Examples

```bash
chmod 4755 file    # setuid
chmod 2755 file    # setgid
chmod 1755 dir     # sticky bit
```


## Exercises

1. **Check if a file is executable; if not, make it so and print a message:**

   ```bash
   #!/bin/bash
   if [[ ! -x "$1" ]]
   then
     chmod +x "$1"
     echo "$1 is now executable."
   else
     echo "$1 is already executable."
   fi
   ```

2. **Change all `.txt` files to read-only for everyone:**

   ```bash
   chmod a=r *.txt
   ```

3. **Print symbolic and numeric permissions for a file:**

   ```bash
   #!/bin/bash
    file="$1"
    symbolic=$(stat -f "%Sp" "$file")
    numeric=$(stat -f "%Mp%Lp" "$file")
    echo "Symbolic: $symbolic"
    echo "Numeric: $numeric"                     
   ```

4. **Set setuid if file is owned by root and executable:**

   ```bash
   #!/bin/bash
   if [[ $(stat -f "%Su" "$1") == "root" && -x "$1" ]]
   then
     chmod u+s "$1"
     echo "setuid bit set on $1"
   else
     echo "Conditions not met."
   fi
   ```

5. **Change ownership of all files in a directory to current user if they have write access:**

   ```bash
   #!/bin/bash
   dir="$1"
   if [[ -w "$dir" ]]
   then
     for file in "$dir"/*
     do
       chown "$(whoami)":"$(id -gn)" "$file"
     done
     echo "Ownership changed."
   else
     echo "You don't have write permission for $dir"
   fi
   ```
