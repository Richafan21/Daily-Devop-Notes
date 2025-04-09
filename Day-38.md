
# Debugging Bash Scripts


## Basic Debugging Techniques

1. Echo Statements
Use `echo` to print variable values or mark script execution:

```bash
echo "Debug: Variable value is $variable"
echo "Debug: Entering function foo()"
```

2. `set -x` Option
Prints each command before executing it:

```bash
#!/bin/bash
set -x
# Commands to debug
set +x
# Commands after debugging
```

3. `set -e` Option
Exits the script immediately if any command fails (non-zero exit code):

```bash
#!/bin/bash
set -e
# Your script here
```


## Advanced Debugging Techniques

### Using `trap`
Catch errors and print custom messages, including the line number:

```bash
trap 'echo "Error on line $LINENO"' ERR
```

### Bash Debug Mode
Run script in debug mode directly from the shell:

```bash
bash -x your_script.sh
```

### `set -v`
Print each line of the script as it's read:

```bash
#!/bin/bash
set -v
# Your script
```

### Redirect Debug Output
Send debug output to a file:

```bash
bash -x your_script.sh 2> debug.log
```


