# VI editor and Networking

---

## Vi editor

The VI editor is a console-based text editor used for editing files and writing code. To open a file in VI editor, simply use the command `vi` and the path to the file as the argument.  

### The Three Operation Modes

- **Command Mode**
- Editor goes to the **Command Mode** by default, which only understands commands.
- You can switch to **Insert Mode** using "i"
- You can switch to the **Last Line Mode** using colon ":".

- **Insert Mode**
- You can enter this mode by pressing "i" when in the **Command mode**.
- In this mode, you can edit the file such as add or remove text.
- Press "esc" to leave and go back to "Command Mode"

- **Last Line Mode**
- Enter this mode by using ":" during **Command Mode**
- In this mode, you can:
- Save file
- Discard Changes
- Exit vi editor
- "esc" to go back to **Command Mode**

### More on the Three Operation Modes

- **Command Mode**
- In this mode, you will see a cursor.
- You can use the direction keys(or k,j,h,l) to move around the cursor.
- Allows you to use the mouse to highlight, copy, paste, or move texts.
- To copy: Move the cursor to the intended line and use "y y" command.
- To paste: Move the cursor above the line you want to paste and press "p"
- To save the file: Use uppercase "Z Z"
- To delete a letter: Move the cursor to the intended location and type "x"
- To delete a line: Move the cursor to any location on the line and type "d d"
- To delete a specific amount of lines: Use "d `number_of_lines` d"
- To undo: Type "u"
- To redo: Press "ctrl r"
- To find a string: Use "/" followed by the pattern you want to search for. It will search the file from the current line downwards. Example: `/line`.
- To find a string: Use "?", which does the same thing as "/" except searches upwards.
- To find next and previous: Use "n" and "N". 

- **Insert Mode**
- After entering this mode from Command Mode using either "i", "a", "o", or "A".
- There should be "Insert" at the bottom, telling you that you're in this mode.
- Behaves similar to notepad, just use backspace and enter.

- **Last Line Mode**
- ":" To enter this mode.
- To save: Type in ":w"
- To exit: Type in ":q"
- To save and exit: Type in ":wq"
- To exit but not save(without Confirmation): Use ":q!"

### VIM

VIM stands for VI Improved  
- Open a file with vim the same way as vi but with `vim`.

