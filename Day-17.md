# Loops

---

The purpose of loops is to execute commands or change values repeatedly. There are two main types of loops, **For** and **While** loops.


## For loops

This loop iterates over a sequence of values or elements:

```bash
for variable in list
do
    # commands to execute
done
```

Example:

**Iterating over a list of values:**
```bash
for sports in basketball football tennis 
do 
  echo "I like $sports" 
done
```

**Iterating over a range of numbers:**
```bash
for i in {1..21} 
do 
  echo "Number: $i" 
done
```

**Iterating over files in a directory:**
```bash
for file in *.txt 
do 
  echo "Processing $file" 
done
```

**C-style For Loop:**
```bash
for ((i=0; i<21; i++)) 
do 
  echo "Iteration $i" 
done
```

## Loop Control
- `break`: Exits the loop immediately
- `continue`: Skips the rest of the current iteration and moves to the next

Example:
```bash
for i in {1..10}
do
    if [ $i -eq 5 ]
    then
        continue
    fi
    if [ $i -eq 8 ]
    then
        break
    fi
    echo "Number: $i"
done
```

## While loop

While loops keep iterating a block of code as long as a condition is true:

### Basic Syntax
```bash
while [ condition ] 
do 
  # commands to execute 
done
```

Examples:

**Basic while loop:**
```bash
count=1 
while [ $count -le 21 ] 
do 
  echo "Count: $count" 
  ((count++)) 
done
```

#### Reading input:
```bash
while read -p "Enter a name (or 'q' to quit): " name 
do 
  if [ "$name" = "q" ] 
  then 
    break 
  fi 
  echo "Hello, $name!" 
done
```

#### Infinite loop with break:
```bash
while true 
do 
  echo "This will run forever unless interrupted" 
  sleep 1 
  if [ "$RANDOM" -eq 0 ] 
  then 
    echo "Lucky break!" 
    break 
  fi 
done
```

