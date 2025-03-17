# Bash Script Exercise on Variables

--- 

## Variables

Variables in bash is the same as other programming languages. It stores data, and allows you to manipulate in within the script.  

Some key points:  

- Variable names can contain letters, numbers, and underscores, but they must start with a letter or underscore, not a number.
- Bash is case-sensitive, so `name` and `Name` are different variables.
- When assigning values to a variable, there should be no spaces around the equals sign(`name="richard"`).
- To use a variable, add `$` to the front of the variable name(`echo $name`), or with curly braces(`echo ${name}`).

## Data Types

Like other programming languages, Bash has three main data types that can be assigned to variables:
- Strings: `echo "Hello $name"`
- Intergers: `echo "In 10 years it will be $((year + 10))`
- Arrays: There are two types of arrays, **indexed** and **associative**

### Arrays

**Indexed** Arrays:
To assign an array to a variable: 
```bash
sports=("basketball" "football" "tennis")
echo ${sports}
# Output = basketball
```
As you can see, when calling the variable, it will only output what is on the first index as bash defaults to `${sports[0]}

To print all elements use the @ or * signs: 
```bash
sports=("basketball" "football" "tennis")
echo ${sports[*]}
# Output = basketball football tennis
```
* will keep the elements in a single string, while @ will keep them seperate.




