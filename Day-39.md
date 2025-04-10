# Error Handling in Bash

## Basic Error Handling Techniques

1. Exit on Error
Use `set -e` to make your script exit immediately when a command fails:

```bash
#!/bin/bash
set -e
# Script continues only if all commands succeed
```

2. Checking Return Values
Always check the return value of commands:

```bash
if ! command_that_might_fail
then
  echo "Error: Command failed"
  exit 1
fi
```

3. Using `||` (OR) Operator
Execute a command if the previous one fails:

```bash
command_that_might_fail || echo "Error: Command failed"
```

## Advanced Error Handling

### Custom Error Function
Create a function to handle errors consistently:

```bash
error_exit() {
  echo "Error: $1" >&2
  exit 1
}
```

Example:
```bash
some_command || error_exit "Failed to execute some_command"
```

## Trap Command
Use `trap` to catch signals and execute code when they occur:

```bash
trap 'echo "Error: Script failed on line $LINENO"' ERR
```

## Handling Specific Error Codes
Check for specific error codes:

```bash
if ! some_command
then
  case $? in
    1) echo "Error: Type 1" ;;
    2) echo "Error: Type 2" ;;
    *) echo "Unknown error" ;;
  esac
fi
```


