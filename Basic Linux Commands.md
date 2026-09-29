# Basic Linux Commands

The Linux commands from the Introduction slides, with the names we use in the course. Run them in a terminal in the Positron window that is connected to your droplet (Terminal → New Terminal). See the slides for the details.

## Find your way around

| Command | What it does |
|---|---|
| `pwd` | Shows the directory you are in (the working directory): `/root` in your home directory |
| `ls` | Lists the files and directories in the current directory |
| `ls -l` | Lists the files and directories with details such as owner, size and date (a `d` first on the line means a directory) |
| `cd /usr/local` | Goes to `/usr/local` with an absolute path: it starts with `/` and works wherever you are |
| `cd usr/` | Goes to `usr` with a relative path: it starts from where you are, so from `/` it goes to `/usr` |
| `cd ..` | Goes one level up (mind the space) |
| `cd ~` | Goes to your home directory (`/root` on the droplet) |

## Create files and directories

| Command | What it does |
|---|---|
| `mkdir NewFolder` | Creates the directory `NewFolder` in the directory you are in |
| `touch example.txt` | Creates the empty file `example.txt` (an existing file keeps its content) |

## Print in the terminal

| Command | What it does |
|---|---|
| `echo "Hello, World!"` | Prints the text in the terminal |
| `echo $HOME` | Prints the value of the variable `HOME` (`/root` on the droplet) |
| `cat example.txt` | Prints the content of the file |

## Delete files and directories

| Command | What it does |
|---|---|
| `rm example.txt` | Deletes the file: there is no recycle bin! |
| `rmdir NewFolder` | Deletes an empty directory (delete the files in it first, or you get an error) |
