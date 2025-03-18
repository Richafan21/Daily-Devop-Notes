# If Statements in Bash

---

## Basic Syntax

--- 

```bash
if [ condition ]
then
  # commands to exeute when condition is met

elif [ condition2 ]
then
    # commands to exeute when condition2 is met
else
    # commands if no conditions are true
fi
```

The square brackets( `[]` ) actually represent the `test` command, so `if [ condition ]`, is the same as `if test condition`.


## Comparison Operators

---

### For numeric comparisons:
```bash
-eq # Equal to
-ne # Not equal to
-gt # Greater than
-ge # Greater than or equal to
-lt # Less than
-le # Less than or equal to
```
Example:
```bash
#!/bin/bash
age=21
if [ $age -ge 21 ]
then
    echo "You are an adult."
else
    echo "You are a not an adult yet."
fi
```

### For string comparisons:
```bash
= # Equal to
!= # Not equal to
-z # String is empty
-n # String is not empty
```

Example:
```
#!/bin/bash
name="Richard"
if [ "$name" = "Richard" ]
then
    echo "Hello, Richard!"
else
    echo "You're not Richard."
fi
```


### For file tests:
```bash
-e file # File exists
-f file # File is a regular file
-d file # File is a directory
-r file # File is readable
-w file # File is writable
-x file # File is executable
```

Example:

```bash
#!/bin/bash
if [ -f "example.txt" ]
then
    echo "example.txt exists."
else
    echo "example.txt does not exist."
fi
```
