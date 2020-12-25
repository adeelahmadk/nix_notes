# Shell Script Cheat Sheet



## Bash Syntax



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