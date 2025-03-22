# Input/Output Redirection


Input/Output (I/O) redirection is a powerful feature in Bash that allows you to control where input comes from and where output goes.

## Standard Streams

In Unix-like operating systems, there are three standard streams:

- **Standard Input (stdin)**: Stream for input, file descriptor 0
- **Standard Output (stdout)**: Stream for output, file descriptor 1
- **Standard Error (stderr)**: Stream for error messages, file descriptor 2

## Basic Redirection Operators

- **`>`**: Redirect stdout to a file (overwrite)
- **`>>`**: Redirect stdout to a file (append)
- **`<`**: Redirect stdin from a file
- **`2>`**: Redirect stderr to a file
- **`&>`**: Redirect both stdout and stderr to a file

## Examples of Redirection

**Redirecting Output**
```bash
# Overwrite
echo "Hello, World!" > output.txt

# Append
echo "Another line" >> output.txt
```

**Redirecting Input:**
```bash
sort < unsorted_list.txt
```

**Redirecting Error**
`ls non_existent_directory 2> error.log`

**Redirecting Both Output and Error**
`command &> output_and_error.log`

## Piping
The pipe operator `|` allows you to send the output of one command as input to another:  
`ls -l | grep ".txt"`

## Here Documents
A here document allows you to pass multiple lines of input to a command.
```bash
cat << EOF > myfile.txt
This is line 1
This is line 2
EOF
```

## More Examples

**Combining stdout and stderr:**  
`(ls /existing_dir; ls /non_existent_dir) &> output.log`

**Discarding output:**  
`command > /dev/null 2>&1`

**Using tee to write to a file and stdout:**  
`echo "Hello, World!" | tee output.txt`

