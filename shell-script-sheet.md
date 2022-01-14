# Shell Script Cheat Sheet



## Contents

1. [Shell Syntax](#Shell-Syntax)
    1. [Special Parameters](#Special-Parameters)
    2. [Shell Arguments](#Shell-Arguments)
    3. [Modifying Shell Behavior](#Modifying-Shell-Behavior)
    4. [File information](#File-information)
    5. [Expansions](#Expansions)
    6. [Redirections](#Redirections)
    7. [Substitutions](#Substitution)
    8. [Command Line Arguments](Command-Line-Arguments)
    9. [Flow Control](#Flow-Control)
    10. [Input Output](#Input-Output)
    11. [Functions](#Functions)
2. [Shell Environment](#Shell-Environment)
    1. [Aliases](#Aliases)
3. [Multilingual One-liners](#Multilingual-One-liners)
    1. [Shell](#Shell)
        1. [Read n lines from anywhere in a file](#Read-n-lines-from-anywhere-in-a-file)
        2. [Generate a random alpha-numeric string](#Generate-a-random-alpha-numeric-string)
    2. [Python](#Python)
        1. [Run a script in a string](#Run-a-script-in-a-string)
        2. [Update all packages](#Update-all-packages)
4. [Useful scripts](#Useful-Scripts)
    1. [Clean dangling network interfaces](#Clean-dangling-network-interfaces)
    2. [Background Ping  Job with log rotation](#Background-Ping-Job-with-log-rotation)
    3. [String Trim](#String-Trim)



## Shell Syntax



### Special Parameters

For detailed description refer to the [shell parameters]([Shell Parameters](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameters.html) ) section in [Bash Reference Manual](https://www.gnu.org/software/bash/manual/html_node/index.html#SEC_Contents).

| Parameter | Description                                                  |
| --------- | ------------------------------------------------------------ |
| *         | ($*) Expands to the positional parameters, starting from one. When the expansion occurs within double quotes, it expands to a single word with the value of each parameter separated by the first character of the `IFS` special variable. |
| s@        | ($@) Expands to the positional parameters, starting from one. When the expansion occurs within double quotes, and word splitting is performed, each parameter expands to a separate word. |
| #         | ($#) Expands to the number of positional parameters in decimal. |
| ?         | ($?) Expands to the exit status of the most recently executed foreground pipeline. |
| -         | ($-, a hyphen.) Expands to the current option flags as specified upon invocation, by the `set` builtin command, or those set by the shell itself |
| $         | ($$) Expands to the process ID of the shell. In a `()` subshell, it expands to the process ID of the invoking shell, not the subshell. |
| !         | ($!) Expands to the process ID of the job most recently placed into the background, whether executed as an asynchronous command or using the `bg` builtin |
| 0         | ($0) Expands to the name of the shell or shell script.       |



### The Set Built-in

Reference: [Bash Manual: The Set Builtin](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html)

#### Shell Arguments

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



#### Modifying Shell Behavior

The built-in `set` allows you to change the values of shell options and set the positional parameters, or to display the names and values of shell variables. For further detail refer to [the set builtin](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html) section in [Bash Reference Manual](https://www.gnu.org/software/bash/manual/html_node/index.html#SEC_Contents).

| Option         | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `-a`           | Each variable or function that is created or modified is given the export attribute and marked for export to the environment of subsequent commands. |
| `-b`           | Cause the status of terminated background jobs to be reported immediately, rather than before printing the next primary prompt. |
| `-e`           | Exit immediately if a pipeline (see [Pipelines](https://www.gnu.org/software/bash/manual/html_node/Pipelines.html)), which may consist of a single simple command (see [Simple Commands](https://www.gnu.org/software/bash/manual/html_node/Simple-Commands.html)), a list (see [Lists](https://www.gnu.org/software/bash/manual/html_node/Lists.html)), or a compound command (see [Compound Commands](https://www.gnu.org/software/bash/manual/html_node/Compound-Commands.html)) returns a non-zero status. |
| `-f`           | Disable filename expansion (globbing).                       |
| `-t`           | Exit after reading and executing one command.                |
| `-u`           | Treat unset variables and parameters other than the special parameters ‘@’ or ‘*’ as an error when performing parameter expansion. |
| `--`           | If no arguments follow this option, then the positional parameters are unset. Otherwise, the positional parameters are set to the arguments, even if some of them begin with a ‘-’. |
| `-`            | Signal the end of options, cause all remaining arguments to be assigned to the positional parameters. |
| -o option-name | Set the option corresponding to *option-name*:               |

##### Option names for -o

| Option   | Description                                                  |
| -------- | ------------------------------------------------------------ |
| history  | Enable command history (useful for non-interactive sessions) |
| pipefail | If set, the return value of a pipeline is the value of the last (rightmost) command to exit with a non-zero status, or zero if all commands in the pipeline exit successfully. This option is disabled by default. |
| posix    | Change the behavior of Bash where the default operation differs from the POSIX standard to match the standard. |



### File information



| Command    | Option               | Description                                                  |
| ---------- | -------------------- | ------------------------------------------------------------ |
| `basename` | `-s` to strip suffix | Print NAME with any leading directory components removed.  If specified, also remove a trailing SUFFIX. e.g. `basename $0` |
| `dirname`  |                      | Output each NAME with its last non-slash component and trailing slashes removed; if NAME contains no /'s, output '.' (meaning the current directory). e.g. `dirname $0` or `dirname $(readlink -f $SHELL)` |
| `realpath` |                      | Print the resolved absolute file name; all but the last component must exist. |
| `readlink` |                      | Print resolved symbolic links or canonical file names. e.g. `readlink -f $SHELL` |



### Bash Variables

Reference: [Bash Manual: Bash Variables](https://www.gnu.org/software/bash/manual/html_node/Bash-Variables.html)

These variables are set or used by Bash, but other shells do not normally treat them specially.

| Variable       | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `BASH`         | The full pathname used to execute the current instance of Bash. |
| `BASHOPTS`     | A colon-separated list of enabled shell options.             |
| `BASHPID`      | Expands to the process ID of the current Bash process.       |
| `BASH_COMMAND` | The command currently being executed or about to be executed |
| `BASH_LINENO`  | An array variable whose members are the line numbers in source files where each corresponding member of `FUNCNAME` was invoked. |
| `BASH_SOURCE`  | An array variable whose members are the source filenames where the corresponding shell function names in the `FUNCNAME` array variable are defined. |
| `FUNCNAME`     | An array variable containing the names of all shell functions currently in the execution call stack. The element with index 0 is the name of any currently-executing shell function. The bottom-most element (the one with the highest index) is "main". Each element of `FUNCNAME` has corresponding elements in `BASH_LINENO` and `BASH_SOURCE` to describe the call stack. For instance, `${FUNCNAME[$i]}` was called from the file `${BASH_SOURCE[$i+1]}` at line number `${BASH_LINENO[$i]}`. |
| `HISTFILE`     | The name of the file to which the command history is saved. The default value is ~/.bash_history. |
| `HOSTNAME`     | The name of the current host.                                |
| `HOSTTYPE`     | A string describing the machine Bash is running on.          |
|                |                                                              |

For more variables check the reference link.

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



#### Operators

| Operator | Description                                               |
| -------- | --------------------------------------------------------- |
| `^`      | Converts a single character to uppercase. e.g. `${name^}` |
| `^^`     | Converts entire string to uppercase. e.g. `${name^^}`     |
| `,`      | Converts a single character to lowercase. e.g. `${name,}` |
| `,,`     | Converts entire string to lowercase. e.g. `${name,,}`     |

##### Example

Change case of entire string

```sh
# Converts any case into lower
# Joe, jOe, JOE, etc to joe
name=${name,,}
```

Change case of the occurrences of a character

```sh
# Converts first character to uppercase
# joe to Joe
name=${name^}
# Converts first occurrence of a character to uppercase
# joe to jOe
name=${name^o}
# Converts all occurrence of a character to uppercase
# john doe to jOhn dOe
name=${name^^o}
```

 

### Redirections

[Ref: Bash documentation](https://www.gnu.org/software/bash/manual/html_node/Redirections.html#Redirections)

Redirection allows commands’ file handles to be duplicated, opened, closed, made to refer to different files, and can change the files the command reads from and writes to. Redirection may also be used to modify file handles in the current shell execution environment. if the file descriptor number is omitted, and the first character of the redirection operator is ‘<’, the redirection refers to the standard input (file descriptor 0). If the first character of the redirection operator is ‘>’, the redirection refers to the standard output (file descriptor 1). 

Bash handles several filenames specially when they are used in redirections, as described in the following table. If the operating system on which Bash is running provides these special files, bash will use them; otherwise it will emulate them internally with the behavior described below.

- `/dev/fd/fd`

    If fd is a valid integer, file descriptor fd is duplicated.

- `/dev/stdin`

    File descriptor 0 is duplicated.

- `/dev/stdout`

    File descriptor 1 is duplicated.

- `/dev/stderr`

    File descriptor 2 is duplicated.

- `/dev/tcp/host/port`

    If host is a valid hostname or Internet address, and port is an integer port number or service name, Bash attempts to open the corresponding TCP socket.

- `/dev/udp/host/port`

    If host is a valid hostname or Internet address, and port is an integer port number or service name, Bash attempts to open  the corresponding UDP socket.

A failure to open or create a file causes the redirection to fail.

#### stdin

Redirection of input causes the file whose name results from the expansion of word to be opened for reading on file descriptor `n`, or the standard input (file descriptor 0) if `n` is not specified.

The general format for redirecting input is:

```sh
# [n]<word
<word | cat
```

#### stdout

Redirection of output causes the file whose name results from the expansion of word to be opened for writing on file descriptor n, or the standard output (file descriptor 1) if n is not specified.  If the file does not exist it is created; if it does exist it is truncated to zero size.

The general format for redirecting output is:

```sh
# [n]>[|]word
```

#### Appending redirected output

Redirection of output in this fashion causes the file whose name results from the expansion of word to be opened for appending on file descriptor n, or the standard output (file descriptor 1) if n is not specified.  If the file does not exist it is created.

The general format for appending output is:

```sh
[n]>>word
```

#### Redirecting stdout and stderr

This construct allows both the standard output (file descriptor 1) and the standard error output (file descriptor 2) to be redirected to the file whose name is the expansion of word.

There are two formats for redirecting standard output and standard error:

```sh
&>word
```

and

>&word

Of the two forms, the first is preferred. This is semantically equivalent to

>word 2>&1

When using the second form, word may not expand to a number or ‘-’. If it does, other redirection operators apply (see Duplicating File Descriptors below) for compatibility reasons. 

#### Appending Standard Output and Standard Error

This construct allows both the standard output (file descriptor 1) and the standard error output (file descriptor 2) to be appended to the file whose name is the expansion of word.

The format for appending standard output and standard error is:

```
&>>word
```

This is semantically equivalent to

```
>>word 2>&1
```

#### Here Documents

[Ref: Bash documentation](https://www.gnu.org/software/bash/manual/html_node/Redirections.html#Here-Documents)

This type of redirection instructs the shell to read input from the current source until a line containing only word (with no trailing blanks) is seen.  All of the lines read up to that point are then used as the standard input (or file descriptor n if n is specified) for a command.

The format of here-documents is:

```
[n]<<[-]word
        here-document
delimiter
```

No parameter and variable expansion, command substitution, arithmetic expansion, or filename expansion is performed on word.

For example:

```sh
wc << EOF
> one two three
> four five
> EOF
 2  5 24
```

In bash these are implemented via temp files, usually in the form `/tmp/sh-thd.<random string>`, while in dash they are implemented as anonymous pipes. This can be observed via tracing system calls with `strace` command. Replace `bash` with `sh` to see how `/bin/sh` performs this redirection.

```bash
$ strace -e open,dup2,pipe,write -f bash -c 'cat <<EOF
> test
> EOF'
```

#### Here Strings

A variant of here documents, the format is:

```
[n]<<< word
```

The word undergoes [tilde expansion](#Tilde-Expansion), [parameter](#Parameter-Expansion) and variable expansion, command substitution, arithmetic expansion, and quote removal. Filename expansion and word splitting are not performed. The result is supplied as a single string, with a newline appended, to the command on its standard input (or file descriptor n if n is specified).

`<<<` is known as `here-string` . Instead of typing in text, you give a pre-made string of text to a program. For example, with such program as `bc` we can do `bc <<< 5*4` to just get output for that specific case, no need to run `bc` interactively.

Here-strings in bash are implemented via temporary files, usually  in the format `/tmp/sh-thd.<random string>`, which are later unlinked, thus making them occupy some memory space temporarily but not show up in the list of `/tmp` directory entries, and effectively exist as anonymous files, which may  still be referenced via file descriptor by the shell itself, and that  file descriptor being inherited by the command and later duplicated onto file descriptor 0 (stdin) via `dup2()` function. This can be observed via

```bash
$ ls -l /proc/self/fd/ <<< "TEST"
total 0
lr-x------ 1 adeel adeel 64 Mar  8 20:08 0 -> '/tmp/sh-thd.DmCvsz (deleted)'
lrwx------ 1 adeel adeel 64 Mar  8 20:08 1 -> /dev/pts/1
lrwx------ 1 adeel adeel 64 Mar  8 20:08 2 -> /dev/pts/1
lr-x------ 1 adeel adeel 64 Mar  8 20:08 3 -> /proc/434533/fd
```

And via tracing syscalls (output shortened for readability; notice  how temp file is opened as fd 3, data written to it, then it is  re-opened with `O_RDONLY` flag as fd 4 and later unlinked, then `dup2()` onto fd 0, which is inherited by `cat` later ):

```bash
$ strace -f -e open,read,write,dup2,unlink,execve bash -c 'cat <<< "TEST"'
```

### Substitution

#### Process Substitution

Process substitution feeds the output of a process (or processes) into the `stdin` of another process. So in effect this is similar to piping `stdout` of one command to the other , e.g. `echo foobar barfoo | wc`, but in the bash manpage it's denoted as `<(list)`. So basically you can redirect output of multiple (!) commands. Technically when you say `< <` you aren't referring to one thing, but two redirections with single  `<` and process redirection of output from `<( . . .)`.

what happens if we do just process substitution?

```bsh
$ echo <(echo bar)
/dev/fd/63
```

As you can see, the shell creates temporary file descriptor `/dev/fd/63` where the output goes. That means  `<` redirects that file descriptor as input into a command. simple example would be to make process substitution of output from two echo commands into `wc`:

```bsh
$ wc < <(echo bar;echo foo)
      2       2       8
```

So here we make shell create a file descriptor for all the output that happens in the parenthesis and redirect that as input to `wc` .As expected, `wc` receives that stream from two echo commands, which by  itself would output two lines, each having a word, and appropriately we  have 2 words, 2 lines, and 6 characters plus two newlines counted.

How is process substitution implemented, we can find out using the trace as below:

```bash
$ strace -e clone,execve,pipe,dup2 -f bash -c 'cat <(/bin/true) <(/bin/false) <(/bin/echo)'
```

### Command Line Arguments

These are also known as special variables provided by the shell.

| Variable     | Details                                                      |
| ------------ | ------------------------------------------------------------ |
| `$1` to `$n` | `$1` is the first argument, `$2` is second argument, till `$n` n-th argument. From 10th argument, you must enclose them in braces like `${10}`, `${11}` and so on. |
| `$0`         | The script name.                                             |
| `$$`         | Process id of current shell.                                 |
| `$*`         | Values of all the arguments. All arguments are double quoted. |
| `$#`         | Total number of arguments passed to script.                  |
| `$@`         | Values of all the arguments.                                 |
| `$?`         | Exit status id of last command.                              |
| `$!`         | Process id of last command.                                  |



### Flow Control

[Conditional Constructs](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html#Conditional-Constructs)

#### Logical expressions

| Expression                     | Description                                                  |
| ------------------------------ | ------------------------------------------------------------ |
| str1 = str2                    | Returns true if the strings are equal                        |
| str1 != str2                   | Returns true if the strings are not equal                    |
| -n str1                        | Returns true if the string is not null                       |
| -z str1                        | Returns true if the string is null                           |
| -d file                        | True if the file is a directory                              |
| -e file                        | True if the file exists (note that this is not particularly portable, thus -f is generally used) |
| -f file                        | True if the provided string is a file                        |
| -g file                        | True if the group id is set on a file                        |
| -r file                        | True if the file is readable                                 |
| -s file                        | True if the file has a non-zero size                         |
| -S file                        | True if the file exists and is a socket file                 |
| -u file                        | True if the user id is set on a file                         |
| -w file                        | True if the file is writable                                 |
| -x file                        | True if the file is an executable                            |
| -p file                        | True if a pipe exists on stdin                               |
| expr1 -eq expr2                | Returns true if the expressions are equal                    |
| expr1 -ne expr2                | Returns true if the expressions are not equal                |
| expr1 -gt expr2                | Returns true if expr1 is greater than expr2                  |
| expr1 -ge expr2                | Returns true if expr1 is greater than or equal to expr2      |
| expr1 -lt expr2                | Returns true if expr1 is less than expr2                     |
| expr1 -le expr2                | Returns true if expr1 is less than or equal to expr2         |
| expr1 -a expr2                 | Logical AND of the expressions                               |
| expr1 -o expr2                 | Logical OR of the expressions                                |
| ! expr1                        | Negates the result of the expression                         |
| <`cond1`> && <`cond2`>         | Logical AND of two conditional expressions                   |
| <`cond1`> \|\| <`cond2`>       | Logical OR of two conditional expressions                    |
| <`regex-expr`> =~ `expression` | the string to the right of the operator is considered a POSIX extended regular expression and matched accordingly (using the POSIX `regcomp` and `regexec` interfaces usually described in *regex*(3)). |



#### Selection

```shell
if test-commands; then
  consequent-commands;
[elif more-test-commands; then
  more-consequents;]
[else alternate-consequents;]
fi

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

```shell
case word in
    [ [(] pattern [| pattern]…) command-list ;;]…
esac

case "" in
    a) ;;
    b) ;;
    *) ;;
esac
```



Select

The `select` construct allows the easy generation of menus. It has almost the same syntax as the `for` command:

```shell
select name [in words …]; do commands; done

select fname in *;
do
	echo you picked $fname \($REPLY\)
	break;
done
```

The set of expanded words is printed on the standard error output stream, each preceded by a number. The `PS3` prompt is then displayed and a line is read from the standard input. If the line consists of a number corresponding to one of the displayed words, then the value of name is set to that word. If the line is empty, the words and prompt are displayed again. If `EOF` is read, the select command completes. Any other value read causes name to be set to null. The line read is saved in the variable `REPLY`.

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



#### ((…))

The arithmetic expression is evaluated according to the rules described in see [Shell Arithmetic](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html#Shell-Arithmetic).

```shell

let "expression"
```

Return a status of 0 or 1 depending on the evaluation of the conditional expression expression. Expressions are composed of the primaries described below in [Bash Conditional Expressions](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html#Bash-Conditional-Expressions). Word splitting and filename expansion are not performed on the words between the `[[` and `]]`; tilde expansion, parameter and variable expansion, arithmetic expansion, command substitution, process substitution, and quote removal are performed. Conditional operators such as ‘-f’ must be unquoted to be recognized as primaries.

#### [[..]]

For example, the following will match a line (stored in the shell variable line) if there is a sequence of characters anywhere in the value consisting of any number, including zero, of characters in the `space` character class, zero or one instances of ‘a’, then a ‘b’:

```shell
[[ $line =~ [[:space:]]*(a)?b ]]
```

### Input Output

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

#### Return Value

##### Global variables

The simplest way to return a value from a bash function is to just set a global variable to the result. Since all variables in bash are global by default this is easy:

```bash
function myfunc()
{
    myresult='some value'
}

myfunc
echo $myresult
```

##### Command Substitution

A better approach is to use local variables in your functions. The problem then becomes how do you get the result to the caller. One mechanism is to use command substitution:

```bash
function myfunc()
{
    local  myresult='some value'
    echo "$myresult"
}

result=$(myfunc)   # or result=`myfunc`
echo $result
```

##### Return Parameter

The other way to return a value is to write your function so that it accepts a variable name as part of its command line and then set that variable to the result of the function:

```bash
function myfunc()
{
    local  __resultvar=$1
    local  myresult='some value'
    eval $__resultvar="'$myresult'"
}

myfunc result
echo $result
```



Reference: [Returning Values from Bash Functions](https://www.linuxjournal.com/content/return-values-bash-functions)

## Shell Environment

This section is about `bash` environment until or unless specified otherwise.

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
alias pingjob='rotlog $HOME/.var/log/pinglog && for (( ; ; )); do echo; echo "Starting ping at: $(date "+%c")"; echo; pingg 20; echo; echo "Sleeping..."; echo; sleep 15; done &>> $HOME/.var/log/pinglog &'
```



### System env



## Multilingual One-liners

### Shell

#### Read n lines from anywhere in a file

There may be multiple ways to read n lines from a file. Simplest method is to use `head` and `tail` commands. To print five lines, from line number 21 to 25:

```sh
head -25 myfile | tail +21
# or
head -25 myfile | tail -5
```

`sed` also provides `-n` option to perform such an operation:

```sh
# from line no. 21 to 25
sed -n '21,25p' myfile
# from line no. 10 to the last line
sed -n '10,$p' file.txt
```

You can also use `awk` to print lines between two line numbers:

```shell
# from line no. 5 to 10
awk 'NR>4&&NR<11' file.txt
```

`awk` provides the most efficient and highly programmable interface with it's `NR` (number of records) variable. To print `N` lines form a random location in a file:

```sh
S=$RANDOM; N=9; E=$((S+N)); awk "NR>=$S && NR<=$E" /usr/share/dict/american-english
```

for lines 10 to last line:

```sh
awk 'NR>9' file.txt
```

#### Generate random numbers



#### Generate a random alpha-numeric string

There are multiple ways to achieve this goal. One simple way is to get a long stream of byte from the random device and then filter it with `tr`, `fold` and `head` as follows:

```sh
head -60 /dev/urandom | tr -dc 'a-z0-9' | fold -w 3 | head -n 1
```



#### Generate a random password



```sh
# using openssl
openssl rand -base64 14
# using urandom device
< /dev/urandom tr -dc A-Za-z0-9 | head -c14; echo
# using gpg
gpg --gen-random --armor 1 14
# using SHA256 hash
date +%s | sha256sum | base64 | head -c 30 ; echo
# using MD5 hash
date | md5sum
cal | md5sum

```



### Python

#### Run a script in a string

Python interpreter give option `-c` to execute a program passed in as string.

```sh
echo -n "https://en.wikipedia.org/w/index.php?title=C%2B%2B&printable=yes" | python3 -c "import sys; from urllib.parse import unquote; print(unquote(sys.stdin.read()));"
# https://en.wikipedia.org/w/index.php?title=C++&printable=yes
```

#### Update all packages

Use one of the following one-liners to update all pip packages on your system. Add a `--user` option to `pip install` in order to install for current user only:

```sh
pip list --outdated --format=freeze | grep -v '^\-e' | cut -d = -f 1  | xargs -n1 pip install -U

pip3 list -o | cut -f1 -d' ' | tr " " "\n" | awk '{if(NR>=3)print)' | cut -d' ' -f1 | xargs -n1 pip3 install -U 
```



## Useful Scripts

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

 ### Background Ping Job with log rotation

First, define a bash function in your `bashrc`

```bash
function rotlog() {
  file="$1"
  MaxFileSize=$((1024*1024))
  file_size=`du -b $file | tr -s '\t' ' ' | cut -d' ' -f1`
  if [ $file_size -ge $MaxFileSize ]; then
    for i in {9..1}; do
      if [[ -f $file.${i} ]]; then
        mv -f $file.${i} $file.$((i+1))
      fi
    done
    mv -f $file $file.1
    touch $file
  fi
}
```

Now, either start a background job from terminal window:

```bash
rotlog $HOME/.var/log/pinglog
for (( ; ; )); do echo; echo "Starting ping at: $(date "+%c")"; echo; ping -c 20 8.8.8.8; echo; echo "Sleeping..."; echo; sleep 15; done &>> $HOME/.var/log/pinglog &
```

or define an alias for it

```bash
alias pingjob='rotlog $HOME/.var/log/pinglog && for (( ; ; )); do echo; echo "Starting ping at: $(date "+%c")"; echo; pingg 20; echo; echo "Sleeping..."; echo; sleep 15; done &>> $HOME/.var/log/pinglog &'
```

### Unique ID Generation

Check the `uuidgen` program which is part of the [e2fsprogs](http://e2fsprogs.sourceforge.net/) package.

According to [Wikipedia UUID article](http://en.wikipedia.org/wiki/Universally_unique_identifier#Implementations), `libuuid` is now part of [util-linux](https://github.com/karelzak/util-linux) and the inclusion in e2fsprogs is being phased out. However, on new Ubuntu systems, `uuidgen` is now in the `uuid-runtime` package.

To create a uuid and save it in a variable:

```sh
uuid=$(uuidgen)
```

On Ubuntu systems, the alpha characters are output as lower case and on OS X systems, they are output as upper case.

To switch to all upper case (after generating it as above):

```sh
uuid=${uuid^^}
```

To switch to all lower case:

```sh
uuid=${uuid,,}
```

If, for example, you have two UUIDs and you want to compare them in `bash`, ignoring their case, you can change both sides to lower case like this:

```sh
if [[ ${uuid1,,} == ${uuid2,,} ]]
```

[Reference: Serverfault answer](https://serverfault.com/a/103366)

### String Trim

Using bash script, we can trim a string from left/right as

```bash
# Declare a variable with a string data.
myVar="  everyone out there  "
# This shows the spaces at the starting and end of the variable.
echo "Hello $myVar!"
# Print the string after left trim.
echo "Hello ${myVar##*( )}"
# Print the string after right trim.
echo "${myVar%%*( )} is welcome to our site"
```

Trim left & right with `sed`

```shell
# The following `sed` command removes the trailing spaces from the variable
myVar=`echo $myVar | sed 's/ *$//g'`
# or
myVar=`echo $myVar | sed 's/^ *//g'`
# Print the output after removing the spaces
echo "Hello $myVar!"
```

`awk` can also be used for this purpose

```shell
# Declare a variable with a string data
Input_text=" Desiginning Website with CSS3 "
# Print the value of the variable before trimming
echo "${Input_text}"
# Print the string after the left trim
echo "${Input_text}" | awk '{gsub(/^[ \t]+/,""); print $0, " JQuery" }'
# Print the string after right trim
echo "${Input_text}" | awk '{gsub(/[ \t]+$/,""); print $0, " JQuery" }'
# Print the string after left + right trim
echo "${Input_text}" | awk '{gsub(/^[ \t]+| [ \t]+$/,""); print $0, " JQuery" }'
```

using `xargs`

```shell
# Remove the spaces from the string data using `xargs`
echo " Bash Scripting Language " | xargs
```

