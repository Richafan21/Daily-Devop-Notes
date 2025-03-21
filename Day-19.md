# My first Bash Script

---

```bash
#!/bin/bash

# Script: my_first_script.sh
# Description: This script demonstrates basic Bash scripting concepts
# Author: Your Name
# Date: YYYY-MM-DD

# Global variables
NAME=${1:-"World"}  # Default to "World" if no name is provided
GREETING="Hello, $NAME!"
MAX_COUNT=5
LOG_FILE="script_output.log"

# Functions
print_greeting() {
    echo "$GREETING"
}

count_down() {
    local count=$1
    if ! [[ "$count" =~ ^[1-9][0-9]*$ ]]; then
        echo "Error: count_down requires a positive integer."
        return 1
    fi
    while [ $count -gt 0 ]; do
        echo $count
        count=$((count - 1))
        sleep 1
    done
    echo "Blast off!"
}

generate_random_number() {
    echo $(( ( RANDOM % 10 ) + 1 ))
}

show_menu() {
    echo "Select an option:"
    echo "1) Print Greeting"
    echo "2) Count Down"
    echo "3) Generate Random Number"
    echo "4) Exit"
}

read_choice() {
    local choice
    read -p "Enter choice [1-4]: " choice
    case $choice in
        1) print_greeting ;;
        2)
            read -p "Enter a positive integer to count down from: " num
            count_down $num
            ;;
        3)
            RANDOM_NUMBER=$(generate_random_number)
            echo "Generated random number: $RANDOM_NUMBER
```
