# Shell Script Cheat Sheet



## Bash Syntax



### Special Parameters

For detailed description refer to the [shell parameters]([Shell Parameters](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameters.html) ) section in [Bash Reference Manual](https://www.gnu.org/software/bash/manual/html_node/index.html#SEC_Contents).

| Parameter | Description                                                  |
| --------- | ------------------------------------------------------------ |
| *         | ($*) Expands to the positional parameters, starting from one. When the expansion occurs within double quotes, it expands to a single word with the value of each parameter separated by the first character of the `IFS` special variable. |
| @         | ($@) Expands to the positional parameters, starting from one. When the expansion occurs within double quotes, and word splitting is performed, each parameter expands to a separate word. |
| #         | ($#) Expands to the number of positional parameters in decimal. |
| ?         | ($?) Expands to the exit status of the most recently executed foreground pipeline. |
| -         | ($-, a hyphen.) Expands to the current option flags as specified upon invocation, by the `set` builtin command, or those set by the shell itself |
| $         | ($$) Expands to the process ID of the shell. In a `()` subshell, it expands to the process ID of the invoking shell, not the subshell. |
| !         | ($!) Expands to the process ID of the job most recently placed into the background, whether executed as an asynchronous command or using the `bg` builtin |
| 0         | ($0) Expands to the name of the shell or shell script.       |



### `sh` ( `dash` ) Arguments

Shell can be launched with number of arguments.

| Option           | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| `-a allexport`   | Export all variables assigned to.                            |
| `-c`             | Read commands from the command_string operand instead of from the standard input. |
| `-C noclobber`   | Don't overwrite existing files with “>”.                     |
| `-e errexit`     | If not interactive, exit immediately if any untested command fails. |
| `-f noglob`      | Disable pathname expansion.                                  |
| `-n noexec`      | If not interactive, read commands but do not execute them.  This is useful for checking the syntax of shell scripts. |
| `-u nounset`     | Write a message to standard error when attempting to expand a variable that is not set, and if the shell is not interactive, exit immediately. |
| `-v verbose`     | The shell writes its input to standard error as it is read.  Useful for debugging. |
| `-x xtrace`      | Write each command to standard error (preceded by a ‘+ ’) before it is executed.  Useful for debugging. |
| `-I ignoreeof`   | Ignore EOF's from input when interactive.                    |
| `-i interactive` | Force the shell to behave interactively.                     |
| `-l`             | Make dash act as if it had been invoked as a login shell.    |
| `-m monitor`     | Turn on job control (set automatically when interactive).    |
| `-s stdin`       | Read commands from standard input (set automatically if no file arguments are present).  This option has no effect when set after the shell has already started running (i.e. with set). |
| `-V vi`          | Enable the built-in vi(1) command line editor (disables -E if it has been set). |
| `-b notify`      | Enable asynchronous notification of background job completion.(UNIMPLEMENTED for 4.4alpha) |



### Modifying `bash` Shell Behavior

The built-in `set` allows you to change the values of shell options and set the positional parameters, or to display the names and values of shell variables. For further detail refer to [the set builtin](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html) section in [Bash Reference Manual](https://www.gnu.org/software/bash/manual/html_node/index.html#SEC_Contents).

| Option | Description                                                  |
| ------ | ------------------------------------------------------------ |
| `-a`   | Each variable or function that is created or modified is given the export attribute and marked for export to the environment of subsequent commands. |
| `-b`   | Cause the status of terminated background jobs to be reported immediately, rather than before printing the next primary prompt. |
| `-e`   | Exit immediately if a pipeline (see [Pipelines](https://www.gnu.org/software/bash/manual/html_node/Pipelines.html)), which may consist of a single simple command (see [Simple Commands](https://www.gnu.org/software/bash/manual/html_node/Simple-Commands.html)), a list (see [Lists](https://www.gnu.org/software/bash/manual/html_node/Lists.html)), or a compound command (see [Compound Commands](https://www.gnu.org/software/bash/manual/html_node/Compound-Commands.html)) returns a non-zero status. |
| `-f`   | Disable filename expansion (globbing).                       |
| `-t`   | Exit after reading and executing one command.                |
| `-u`   | Treat unset variables and parameters other than the special parameters ‘@’ or ‘*’ as an error when performing parameter expansion. |
| `--`   | If no arguments follow this option, then the positional parameters are unset. Otherwise, the positional parameters are set to the arguments, even if some of them begin with a ‘-’. |
| `-`    | Signal the end of options, cause all remaining arguments to be assigned to the positional parameters. |



### Expansions

#### Brace Expansion



#### Tilde Expansion



#### Parameter Expansion

The `$' character introduces parameter expansion, command substitution, or  arithmetic expansion. The  parameter  name  or symbol to be expanded may be enclosed in braces, which are optional but serve to protect the variable to be  expanded  from  characters immediately following it.

|            Expansion             | *parameter* **Set and Not Null** | *parameter* **Set But Null** | *parameter* **Unset** |
| :------------------------------: | :------------------------------: | :--------------------------: | :-------------------: |
| **${**parameter***:-***word**}** |      substitute *parameter*      |      substitute *word*       |   substitute *word*   |
| **${**parameter***-***word**}**  |      substitute *parameter*      |       substitute null        |   substitute *word*   |
| **${**parameter***:=***word**}** |      substitute *parameter*      |        assign *word*         |     assign *word*     |
| **${**parameter***=***word**}**  |      substitute *parameter*      |       substitute null        |     assign *word*     |
| **${**parameter***:?***word**}** |      substitute *parameter*      |         error, exit          |      error, exit      |
| **${**parameter***?***word**}**  |      substitute *parameter*      |       substitute null        |      error, exit      |
| **${**parameter***:+***word**}** |        substitute *word*         |       substitute null        |    substitute null    |
| **${**parameter***+***word**}**  |        substitute *word*         |      substitute *word*       |    substitute null    |

For `bash` shell expansions refer to parameter expansions section in  man page.

| Expansion                                          | Description          |
| -------------------------------------------------- | -------------------- |
| `${parameter:offset}` `${parameter:offset:length}` | Substring Expansion. |
|                                                    |                      |
|                                                    |                      |
|                                                    |                      |



### Flow Control

#### Logical expressions

| Expression               | Description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| str1 = str2              | Returns true if the strings are equal                        |
| str1 != str2             | Returns true if the strings are not equal                    |
| -n str1                  | Returns true if the string is not null                       |
| -z str1                  | Returns true if the string is null                           |
| -d file                  | True if the file is a directory                              |
| -e file                  | True if the file exists (note that this is not particularly portable, thus -f is generally used) |
| -f file                  | True if the provided string is a file                        |
| -g file                  | True if the group id is set on a file                        |
| -r file                  | True if the file is readable                                 |
| -s file                  | True if the file has a non-zero size                         |
| -S file                  | True if the file exists and is a socket file                 |
| -u file                  | True if the user id is set on a file                         |
| -w file                  | True if the file is writable                                 |
| -x file                  | True if the file is an executable                            |
| -p file                  | True if a pipe exists on stdin                               |
| expr1 -eq expr2          | Returns true if the expressions are equal                    |
| expr1 -ne expr2          | Returns true if the expressions are not equal                |
| expr1 -gt expr2          | Returns true if expr1 is greater than expr2                  |
| expr1 -ge expr2          | Returns true if expr1 is greater than or equal to expr2      |
| expr1 -lt expr2          | Returns true if expr1 is less than expr2                     |
| expr1 -le expr2          | Returns true if expr1 is less than or equal to expr2         |
| expr1 -a expr2           | Logical AND of the expressions                               |
| expr1 -o expr2           | Logical OR of the expressions                                |
| ! expr1                  | Negates the result of the expression                         |
| <`cond1`> && <`cond2`>   | Logical AND of two conditional expressions                   |
| <`cond1`> \|\| <`cond2`> | Logical OR of two conditional expressions                    |



#### Selection

```sh
if [ <Condition> ]
then
	# Do if true
elif [ <Condition> ]
then
	# Do if true
else
	# Otherwise
fi

# one-liner
if [ -f "/usr/bin/wine" ]; then export WINEARCH=win32; fi
# or
[ -f "/usr/bin/wine" ] && export WINEARCH=win32
```

Switch/case statement:

```sh
case "" in
    a) ;;
    b) ;;
    *) ;;
esac
```



#### Iteration

```sh
while [ condition ]
do
    command1
    command2
    command3
done

# one-liner
while [ condition ]; do commands; done
    
while control-command; do COMMANDS; done

```

Examples:

```sh
x=1
while [ $x -le 5 ]
do
	echo "Welcome $x times"
	x=$(( $x + 1 ))
done
```

For loop:

```sh
# iterate on a list of values
for VAR in 1 2 3 4 5 .. N
do
    # command 1
    # command 2
    # command N
done

# iterate on a list of files
for VAR in file1 file2 file3
do
    # command1 on $VAR
    # command2
    # commandN
done

# iterate on results of a command
for OUTPUT in $(Linux-Or-Unix-Command)
do
    # command1 on $OUTPUT
    # command2 on $OUTPUT
    # commandN
done

# iterate on array elements
for i in {0..10..2}; do 
	echo "Welcome $i times"
done

# classic 3-statement for loop
for (( EXP1; EXP2; EXP3 ))
do
    command1
    command2
    command3
done

```



### I/O

#### File reading

Use `read -r line` in bash and `read line` in dash.

```sh
while IFS= read -r line
do
  printf 'Line: %s\n' "$line"
done < file
```

reading from multiple files in a single loop:

```sh
while IFS= read -r -u 4 line1 && IFS= read -r -u 5 line2; do
  echo "Line from first file: $line1"
  echo "Line from second file: $line2"
done 4<file1 5<file2
```



#### stdin

checking if the script received input from stdin:

```sh
[ $# -ge 1 -a -f "$1" ] && input="$1" || input="-"
cat $input
```

To check whether a pipe exists on stdin:

```sh
# Check to see if a pipe exists on stdin.
if [ -p /dev/stdin ]; then
        echo "Data was piped to this script!"
        # If we want to read the input line by line
        while IFS= read line; do
                echo "Line: ${line}"
        done
        # Or if we want to simply grab all the data, we can simply use cat instead
        # var="$(cat)"
else
        echo "No input was found on stdin, skipping!"
        # Checking to ensure a filename was specified and that it exists
        if [ -f "$1" ]; then
                echo "Filename specified: ${1}"
                echo "Doing things now.."
        else
                echo "No input given!"
        fi
fi
```

To check all possible inputs:

```sh
if readlink /proc/$$/fd/0 | grep -q "^pipe:"; then
    # Pipe input (echo abc | myscript)
elif file $( readlink /proc/$$/fd/0 ) | grep -q "character special"; then
    # Terminal input (keyboard)
else
    # File input (myscript < file.txt)
fi
```

Alternative syntax:

```sh
# In Bash you can also use test -t to check for a terminal:
if [ -t 0 ]; then
    # Terminal input (keyboard) - interactive
else
    # File or pipe input - non-interactive
fi
```



#### Temp files

```sh
TMPFILE=$(mktemp /tmp/$0.$$.XXXXXX)
cat > "$TMPFILE"
# Script works with $TMPFILE and its contents,
# ultimately writing everything to stdout.
rm -f "$TMPFILE"
```



### Functions



## Bash Env

### Aliases

#### common commands
```sh
alias ls='ls --color=auto'
alias ll='ls -lh'
alias la='ls -A'
alias lla='ls -Alh'
alias llt='ls -lts'
alias lsd='ls -d */'
alias lsf='ls -lh | egrep -v "^d"'
```



#### custom application functions
```sh
alias mergepdf='gs -dBATCH -dNOPAUSE -q -sDEVICE=pdfwrite -sOutputFile=merged_file.pdf'
```



#### package manager (Debian/Ubuntu)
```sh
alias aptud='sudo apt update'
alias aptug='sudo apt upgrade'
alias aptrm='sudo apt remove'
alias aptarm='sudo apt autoremove'
alias aptin='sudo apt install'
alias aptls='apt list --upgradable'
alias apts='apt search'
```



#### system admin commands

```sh
# Search and List a process by name
alias lsproc='ps -ef | grep'
# List mounted disks
alias ldsk='mount|grep /dev/sd|cut -f1-3 -d" "|sort'
alias usage='du -hxd1'
alias dsz='du -hxd0'
alias filesfx='echo `date "+%F"`_`date "+%s"`'
alias rmspc='find -name "* *" -type f | rename "s/ /_/g"'
```



#### network admin commands

```sh
alias lshosts='fping -aAqgn -r 0'
alias scansub='sudo nmap -sP -PB'
alias pingg='ping 8.8.8.8 -c'
alias intip="ifconfig `route -n | grep -m1 -e ^'0\.0\.0\.0' |awk '{print \$NF}'` | grep 'inet ' | awk '{print \$2}' | sed 's/addr://1'"
alias pubip='curl -s "https://api.ipify.org" ; echo'
alias hdrchk='curl -o /dev/null --max-time 3 --silent --write-out "HTTP Status: %{http_code}\n"'
alias lslp='netstat -lntup'
```



## Net admin scripts

### Clean dangling network interfaces

Some applications create virtual network interfaces to accomplish certain tasks but end without cleaning them. A simple shell script can do the cleanup:

```sh
for veth in $(ifconfig | grep "^veth" | cut -d' ' -f1)
do
    ifconfig $veth down
    # OR: ip link set $veth down
done
```

Or by a one-liner:

```sh
for veth in $(ifconfig | grep "^veth" | cut -d' ' -f1); do ifconfig $veth down; done
```

 

## development environment

Use one of the following one-liners to update all pip packages on your system. Add a `--user` option to `pip install` in order to install for current user only:

```sh
pip list --outdated --format=freeze | grep -v '^\-e' | cut -d = -f 1  | xargs -n1 pip install -U

pip3 list -o | cut -f1 -d' ' | tr " " "\n" | awk '{if(NR>=3)print)' | cut -d' ' -f1 | xargs -n1 pip3 install -U 
```