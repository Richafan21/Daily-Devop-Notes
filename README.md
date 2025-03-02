# Daily-Devop-Notes

*Keeping track of what I learn each day on my way to become a DevOps Engineer. Commands will be represented by a $ sign before it.*

## Day 1: Linux Basics

Linux shell is a program that allows text-based interactions between the program and the user.  
The home directory (`/home/richard`) is like a dedicated locker assigned to you. Other users cannot access anything within your own home directory. By default, it is represented by `~`.

### Command and Arguments

- `echo`: A **command** that acts like a print statement. It does not do anything unless you add an argument (e.g., `$echo hello`).
- `uptime`: A **command** that does not require an argument. It tells us how long the system has been running since the last reboot.

Linux also has **options** (or **flags**), which are added after a command to alter its behavior. Options are usually represented by a single letter preceded by a hyphen (e.g., `-n`).  
It can be challenging to memorize all the options, so it’s better to refer to the command’s help options.  

There are two types of commands: **Internal** and **External**. Built-in commands are internal, while external commands are binary programs or scripts, usually located in distinct files in the system. These can come preinstalled or be installed by the user. You can use `type` followed by another command as the argument to determine whether that command is internal or external.  
Example: `$type cd` (output: `cd is a shell builtin`).

### Basic Linux Commands
- `cd`: Change directory. Add the directory you want to move to as the argument.  
  Example: `$cd /home/richard/Food`.  
  This can only go down the current working directory. In order to go up, use `cd ..` or to go straight to the home directory, use `cd ` without any arguments.
- `mkdir`: Make directory. Add the name of the directory as the argument, and it will become a directory under the working directory.
  Example: `$mkdir Food`.  
  If you want to add a directory into another directory that is not the current working one, you can do `$mkdir Food/Burger` to make a directory called Burger under the already made directory called Food. If the Food directory has not yet been made, add the option `-p` to make the parent directory as well under the working directory(`mkdir -p Food/burger`) 
- `pwd`: Print working directory.  
  Example: `$pwd` (output: `/home/richard`).
- `ls`: List Storage command, lists everything inside a directory, no argument needed if you want to list storage from current, otherwise you can add argument to specify which directory you want to list out.
You can add the option `-l` for long list, which provides more details such as who owns it and when it was last opened.
You can also use the option `-a` which will list all files including the hidden ones. The single dot represents the current directory, and the double dot is the directory before it.
The option `-lt` will list file in order created and `ltr` the reverse order. 
- `pushd` and `popd`: Instead of `cd`, you can use `pushd` to move to another directory, but save the original working directory, no matter how many times you `cd` afterwards, using `popd` will imemdiately change to the original working directory.
- `mv`: Move file or directory, requires two arguments, first one is the directory you want to move, and the second is the directory you want to move it into.
Example: `mv /home/richard/Food/Fanta /Home/richard/Drink`  (Don't need /home/richard for either argument if you are already in that path.
You can also use `mv` to rename a file/directory,
Example: `mv Food/Burga Food/Burger`
- `cp`: Copy file command. Takes in two arguments, the file first and then the directory you want to copy it to.  
Example: `cp Food/Burger/Ingredients.txt Food/Sandwhich`  
The option `-r` standing for recursive may be used to copy and then delete the previous directory.  
- `rm`: Remove file or directory command. Simply add the file or directory you want to remove as the argument.  
- `cat`: Read contents in a file, file name as the argument. The contents will be printed on screen.
To change what is in a file, use the redirect option `>` after the cat. The system will then wait for an input from the user and you may type what you want to add in the file.
Example: cat > Food/Burger/Ingredients.txt     (input: Lettuce) You then **ctrl + d** to save the data and exit the prompt.
- `touch`: To add a new empty file. Name of file and what directory it is in for the argument.
Example: `touch /home/richard/Food/Burger/Recipe.txt`
- `more`: File as argument, used if the file is large and has a lot of content as it will enter a mode where certain buttons will do certain things.  
**SpaceBar** will scroll the display one screenful of data at a time.  
  **Enter** scrolls the display one line  
  **b** scrolls the display backwards one screenful of data at a time  
  **/** for searching text
  **q** Quit out
- `less`: Use file as argument, doesnt load everything at once.  
**UPArrow** scrolls up the display one line  
  **DOWNArrow** scrolls down the display one line  
  **/** searches text  
  **q** to quit out

This is a lot to take in, luckily there are a few commands within linux to help remind us what a command does or how to use it.  
`whatis`: takes a command as an argument and will display a one line description of what the command does.  
`man`: Man page takes a command as an argument and gives a more detailed description.    
Most commands comes with an option `--help` with a description and how to use it.    
`apropos`: takes a keyword as an argument and will search through the man page names and descriptions for this keyword.
  

