# Inputs

## `read` Command Basics

The simplest form of the `read` command:
```bash
read <variable_name>
```

Example:
```bash
echo "Enter your name:"
read name
echo "Hello, $name!"
```

## `read` Command Options

### Prompt with `-p`
```bash
read -p "Enter your age: " age
```


### Silent Input with `-s`  
Useful for passwords:
```bash
read -sp "Enter password: " password
echo
```

### Timeout with `-t`
```bash
read -t 5 -p "You have 5 seconds to answer: " answer
```


### Read a Specific Number of Characters with `-n`
```bash
read -n 1 -p "Do you agree? (y/n) " response
echo
```

### Using a Delimiter with `-d`
```bash
read -d "," -p "Enter comma-separated values: " values
```


### Reading Multiple Variables
```bash
read -p "Enter first and last name: " first_name last_name
echo "First Name: $first_name, Last Name: $last_name"
```


