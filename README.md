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
  This can only navigate down from the current working directory. To move up, use `cd ..`. To return to the home directory, use `cd` without any arguments.
- `mkdir`: Make directory. Add the name of the directory as the argument, and it will become a directory under the working directory.  
  Example: `$mkdir Food`.  
  To create a directory inside another directory that doesn’t exist yet, use the `-p` option:  
  Example: `$mkdir -p Food/Burger`.
- `pwd`: Print working directory.  
  Example: `$pwd` (output: `/home/richard`).
- `ls`: List storage. Lists everything inside a directory. No argument is needed to list the current directory.  
  Example: `$ls /home/richard/Food`.  
  Options:
  - `-l`: Long list format (shows details like ownership and modification time).
  - `-a`: Lists all files, including hidden ones.
  - `-lt`: Lists files in order of creation.
  - `-ltr`: Lists files in reverse order of creation.
- `pushd` and `popd`: Save and navigate directories.  
  - `pushd`: Saves the current directory and moves to a new one.  
    Example: `$pushd /home/richard/Food` (saves `/home/richard` and moves to `/home/richard/Food`).
  - `popd`: Returns to the saved directory.  
    Example: `$popd` (returns to `/home/richard`).
- `mv`: Move or rename files/directories. Requires two arguments: the source and the destination.  
  Example: `$mv /home/richard/Food/Fanta /home/richard/Drink`.  
  To rename: `$mv Food/Burga Food/Burger`.
- `cp`: Copy files/directories. Requires two arguments: the source and the destination.  
  Example: `$cp Food/Burger/Ingredients.txt Food/Sandwich`.  
  Use `-r` (recursive) to copy directories.
- `rm`: Remove files/directories. Add the file or directory as the argument.  
  Example: `$rm Food/Burger/Ingredients.txt`.  
  Use `-r` to remove directories.
- `cat`: Read file contents. Add the file name as the argument.  
  Example: `$cat Food/Burger/Ingredients.txt`.  
  To edit: `$cat > Food/Burger/Ingredients.txt` (input: `Lettuce`), then press `Ctrl + D` to save.
- `touch`: Create an empty file. Add the file name as the argument.  
  Example: `$touch /home/richard/Food/Burger/Recipe.txt`.
- `more`: View file contents page by page. Add the file name as the argument.  
  Navigation:
  - **Spacebar**: Scroll one screenful.
  - **Enter**: Scroll one line.
  - **b**: Scroll backward.
  - **/**: Search text.
  - **q**: Quit.
- `less`: View file contents without loading the entire file. Add the file name as the argument.  
  Navigation:
  - **Up/Down Arrow**: Scroll one line.
  - **/**: Search text.
  - **q**: Quit.

### Help Commands
- `whatis`: Provides a one-line description of a command.  
  Example: `$whatis ls`.
- `man`: Displays the manual page for a command.  
  Example: `$man ls`.
- `--help`: Displays a brief description and usage options for a command.  
  Example: `$ls --help`.
- `apropos`: Searches the man pages for a keyword.  
  Example: `$apropos list`.

### Bash Shell
Also known as Bourne Again Shell. Use `echo $SHELL` to check what shell is being used. Use `chsh` to change the default shell. You will be prompted for your password and then the new shell.  
Other types of shells: Bourne Shell(sh), C Shell(csh or tcsh), Korn Shell(ksh), Z Shell(zsh)  
Some features of the Bash shell include:
- **Auto-completion**: Press `Tab` to auto-complete commands or file paths.
- **Aliases**: Set shortcuts for commands.  
  Example: `alias dt=date`.  
  To make this persist we can do `echo 'alias dt=date' >> /home/richard/.profile`  
- Use `history` to check previously used commands and aliases.
- **Environment Variables**: Use `env` to see all environment variables.  
  Example: `export NEW_VAR="value"`.
- **Path Variable**: Use `echo $PATH` to check the PATH variable.  
  Use `which` to find the location of a program.  
  Example: `which ls`.  
  Add a new directory to PATH:  
  Example: `export PATH=$PATH:/opt/obs/bin`.
- **Custom Bash Prompt**: Customize the Bash prompt using `PS1`.  
  Example: `PS1="[\d \t \u@\h:\w ] $ "`  
  Output: `Mon Mar 3 11:12:44 richard@ubuntu:~ ] $`.  
  Note: These changes may not be persistent across sessions, so we should use the command line `>> ~/.profile` to the end of it.  
  Example: ` echo 'PS1="[\d \t \u@\h:\w ] $ "' >> /home/richard/.profile` 
