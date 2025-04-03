# Linux BASH Catchup

## **String Manipulation**

To print the length of a string:

```bash
string="Hello, World!"
echo ${#string}  # Output: 13
```

## **Splitting a Sentence into Words**  

Bash can split a string into words using `read -a`:  

```bash
sentence="Hello world"
read -a words <<< "$sentence"
echo "${words[0]}"  # Output: Hello
echo "${words[1]}"  # Output: world
```

**How it works:**  
- `read -a words <<< "$sentence"` stores the words of 'sentence' in an array called 'words'.  
- `${words[index]}` accesses individual words.  

## **Reversing a String Using `rev`**  
The `rev` command reverses a string:  

```bash
echo "Hello" | rev  # Output: olleH
```


 

## **Using `grep` to Count Vowels**  
```bash
echo "hello" | grep -o '[aeiouAEIOU]' | wc -l  
```
**How it works:**  
- `grep -o '[aeiouAEIOU]'` extracts vowels one by one.  
- `wc -l` word count the number of lines (i.e., vowels).  

## **Using `tr` to Remove Non-Vowel Characters**  
```bash
echo "hello world" | tr -cd 'aeiouAEIOU' | wc -c  
```
**How it works:**
- `tr` translate is used for text transformation
- `-c` compliment to do invert the character set(select everything **except** the specified characters)
- `-d` delete the selected characters
- `wc` word count, with `-c` flag will count bytes
**`tr -cd` Removes All Characters Except Vowels**  




## **Using String Concatenation**
```bash
filename="document.txt"
new_filename="processed_$filename"
echo "$new_filename"  # Output: processed_document.txt
```


## **Extracting a Filename from a Path**  
To extract only the filename (without path):  
```bash
filepath="/home/user/document.txt"
filename="${filepath##*/}"
echo "$filename"  # Output: document.txt
```
**`##*/` Removes Everything Before the Last `/`**  



## **Generating Random Characters**
```bash
cat /dev/urandom | tr -dc 'A-Za-z0-9!@#$%^&*()_+' | head -c 12
```
**How it works:**  
- `/dev/urandom` generates random bytes.  
- `tr -dc 'A-Za-z0-9!@#$%^&*()_+'` filters only valid password characters.  
- `head -c 12` limits output to 12 characters.



### **Comparing a String with Its Reverse**  
```bash
word="racecar"
if [[ "$word" == "$(echo $word | rev)" ]]
then
  echo "Palindrome"
else
  echo "Not a palindrome"
fi
```
 

### **Summary Table**  

| **Task**           | **Command / Method**  | **Use Case**  |
|--------------------|----------------------|--------------|
| **Reverse Words** | `rev` | Reverse individual words |
| **Count Vowels**  | `grep -o '[aeiou]'` | Extract and count vowels |
| **Add Prefix**    | `"prefix_$filename"` | Modify filenames |
| **Random Password** | `tr -dc 'A-Za-z0-9!@#$%^&*()_+'` | Generate secure passwords |
| **Palindrome Check** | `[[ "$str" == "$(echo $str | rev)" ]]` | Compare with reversed string |




