# Functions

---

Functions in Bash allow you to group commands into reusable units. 

## Defining Functions
There are two common ways to define functions in Bash:

### Using the `function` keyword:
```bash
function # function_name {
    # commands
}
```

### Without the `function` keyword:
```bash
function_name() {
    # commands
}
```

Both methods are the same so just choose the one that you prefer.

## Calling Functions
To call a function, simply use its name:
```bash
function_name
```

### Function Parameters
Functions can accept parameters, which are accessed using `$1`, `$2`, etc., similar to script arguments:
```bash
greet() {
    echo "Hello, $1!"
}

greet "Funmi"  # Outputs: Hello, Funmi!
```

## Return Values
Bash functions don't return values like in other programming languages. Instead, they can:

### Use the `return` command to exit the function with a status code:
```bash
is_even() {
    if (( $1 % 2 == 0 )); then
        return 0  # Success (true)
    else
        return 1  # Failure (false)
    fi
}
```

### Echo a result that can be captured:
```bash
get_square() {
    echo $(($1 * $1))
}

result=$(get_square 5)
echo "The square is $result"
```

### Local Variables
Use the `local` keyword to declare variables that are local to the function:
```bash
my_function() {
    local my_var="local value"
    echo "$my_var"
}
```

## More Examples

### Simple greeting function:
```bash
greet() {
    echo "Hello, $1! Welcome to Bash scripting."
}

greet "Chuks"
```

### Function with multiple parameters:
```bash
calculate() {
    case $2 in
        +) echo $(($1 + $3));;
        -) echo $(($1 - $3));;
        *) echo $(($1 * $3));;
        /) echo $(($1 / $3));;
    esac
}

result=$(calculate 10 + 5)
echo "Result: $result"
```

### Function using return value:
```bash
is_file() {
    if [ -f "$1" ]; then
        return 0
    else
        return 1
    fi
}

if is_file "example.txt"; then
    echo "example.txt exists"
else
    echo "example.txt does not exist"
fi
