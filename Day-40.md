# Command-Line Arguments in Bash

## Basic Command-Line Arguments

In Bash, command-line arguments are accessible through special variables:

- `$0`: The name of the script itself
- `$1`, `$2`, `$3`, etc.: The first, second, third, etc. arguments
- `$#`: The number of arguments passed to the script
- `$@`: All the arguments passed to the script

## Handling Multiple Arguments

To process all arguments, you can use a loop:

```bash
#!/bin/bash

for arg in "$@"
do
    echo "Argument: $arg"
done
```


## The `shift` Command

The `shift` command moves the arguments to the left, discarding the first argument:

```bash
#!/bin/bash

while [ "$1" != "" ]; do
  echo "Processing: $1"
  shift
done
```

## Handling Options and Flags

For more complex argument handling, you can use `getopts`:

```bash
#!/bin/bash

while getopts "a:b:" opt
do
  case $opt in
    a) echo "Option -a was triggered, Parameter: $OPTARG" ;;
    b) echo "Option -b was triggered, Parameter: $OPTARG" ;;
    ?) echo "Invalid option: -$OPTARG" ;;
  esac
done
```


## Default Values for Arguments

You can provide default values for arguments:

```bash
#!/bin/bash

name=${1:-"World"}
echo "Hello, $name!"
```


## Checking for Required Arguments

To ensure required arguments are provided:

```bash
#!/bin/bash

if [ $# -eq 0 ]
then
  echo "No arguments provided"
  exit 1
fi
```

