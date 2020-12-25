# Development Environments



## bash

### xclip [OPTION] [FILE]...

Reads  from  standard in, or from one or more files, and makes the data
available as an X selection for pasting  into  X  applications.  Prints
current X selection to standard out.

       -i, -in
              read  text  into  X  selection  from  standard  input  or  files
              (default)
    
       -o, -out
              print the selection to standard out (generally for piping  to  a
              file or program)
    
       -f, -filter
              when  xclip  is  invoked in the in mode with output level set to
              silent (the defaults), the filter option  will  cause  xclip  to
              print the text piped to standard in back to standard out unmodi‐
              fied
    
       -l, -loops
              number of X selection requests (pastes into X  applications)  to
              wait  for  before  exiting,  with a value of 0 (default) causing
              xclip to wait for an unlimited number of requests until  another
              application  (possibly another invocation of xclip) takes owner‐
              ship of the selection
    
       -t, -target
              specify a particular data format using the  given  target  atom.
              With  -o  the  special target atom name "TARGETS" can be used to
              get a list of valid target atoms for this selection.   For  more
              information about target atoms refer to ICCCM section 2.6.2
    
       -d, -display
              X  display  to  use  (e.g. "localhost:0"), xclip defaults to the
              value in $DISPLAY if this option is omitted
    
       -h, -help
              show quick summary of options
    
       -selection
              specify which X selection to use, options are "primary"  to  use
              XA_PRIMARY  (default),  "secondary"  for  XA_SECONDARY or "clip‐
              board" for XA_CLIPBOARD
    
       -version
              show version information
    
       -silent
              fork into the background to wait for requests, no  informational
              output, errors only (default)
    
       -quiet show informational messages on the terminal and run in the fore‐
              ground
    
       -verbose
              provide a running commentary of what xclip is doing
    
       -noutf8
              operate in legacy (i.e. non UTF-8) mode for  backwards  compati‐
              bility  (Use  this option only when really necessary, as the old
              behavior was broken)

#### Examples

Copy a file to clipboard: 

```sh
xclip -sel clip < ~/.ssh/id_rsa_bb.pub
```

Put  your uptime in the X selection:

```sh
uptime | xclip
```

Put the contents of the selection into a file:

```sh
xclip -o > helloworld.c
```

Middle click in an X application supporting HTML to paste the contents of the given file as HTML:

```sh
xclip -t text/html index.html
```



## Profiling



## Debugging

### strace

`strace` runs the specified command until it exits.  It intercepts and records the system calls which are called by a process and the signals which  are  received by a process.  The name of each system call, its arguments and its return value are printed on standard error or to the file specified with the `-o` option.

```sh
strace -o cp_strace cp .bashrc bashrc
```

#### Examples




## python

### Package management

To generate a list of all outdated packages:

```sh
pip list --outdated
```

#### pip ModuleNotFoundError

Sometimes after upgrade pip gives following error:

```sh
~ pip3 -V
Traceback (most recent call last):
  File "/usr/local/bin/pip", line 7, in <module>
    from pip._internal import main
ModuleNotFoundError: No module named 'pip._internal'
```

A simple fix for this is to force reinstall from **pypa.org**:

```sh
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py --force-reinstall
```



#### Update all Python Packages

##### Windows

Open a command shell by typing ‘powershell’ in the Search Box of the Task bar and enter: 

```powershell
pip freeze | %{$_.split('==')[0]} | %{pip install --upgrade $_}
```

This will upgrade all packages system-wide to the latest version available in the Python Package Index (PyPI).

##### Linux

Linux provides a number of ways to use pip in order to upgrade Python packages, including grep and awk.

To upgrade all packages using pip with grep on Ubuntu Linux:

```sh
pip3 list --outdated --format=freeze | grep -v '^\-e' | cut -d = -f 1 | xargs -n1 pip3 install -U 
```

To upgrade all packages using pip with awk on Ubuntu Linux:

```sh
pip3 list -o | cut -f1 -d' ' | tr " " "\n" | awk '{if(NR>=3)print)' | cut -d' ' -f1 | xargs -n1 pip3 install -U 
```

##### System Agnostic

Pip can be used to upgrade all packages on either Windows or Linux:

1.  Output a list of installed packages into a requirements file (requirements.txt): 

```sh
pip freeze > requirements.txt
```

1.  Edit requirements.txt, and replace all ‘==’ with ‘>=’. Use the ‘Replace All’ command in the editor.
2.  Upgrade all outdated packages: 

```sh
pip install -r requirements.txt --upgrade
```



##### Virtual Environment

The easiest way to update unpinned  packages (i.e., packages that do not require a specific version) in a  virtual environment is to run the following Python script that makes use of pip:

```sh
import pkg_resources
from subprocess import call

for dist in pkg_resources.working_set:
    call("python -m pip install --upgrade " + dist.<projectname>, shell=True)
```

##### Pipenv

The simplest way to update all the  unpinned packages in a specific virtual environment created with pipenv  is to do the following steps:

1.  Activate the Pipenv shell that contains the packages to be upgraded:

```sh
pipenv shell
```

1.  Upgrade all packages:

```sh
pipenv update
```

### Command Line



### Virtual Environements

#### Virtualenv

Create a virtual environment in your project directory:

```sh
virtualenv [-p python[3]] venv
```

Activate your virtual env:

```sh
source venv/bin/activate
```

Deactivate your virtual env:

```
deactivate
```

Add requirements file in project root for deployment purpose:

```
requests
bs4
Flask==0.11.1
```

and reproduce your dev env on production machine as:


```sh
pip install -r require.txt
pip list
pip freeze > frozen_req.txt
```

Don't forget to Git ignore `venv` directory!!!

#### Pipenv



## IDEs

### vim/neovim

Refer to [vim-sheet](vim-sheet.md) for details.

| command / key                  | mode | description                                                  |
| ------------------------------ | ---- | ------------------------------------------------------------ |
| :help [keyword]                |      | Performs a search of help documentation                      |
| :e [file]                      |      | Opens a file                                                 |
| :w                             |      | Saves the file you are working on                            |
| :w [filename]                  |      | save your file with the name                                 |
| :wq                            |      | Save your file and close Vim                                 |
| :q!                            |      | Quit without first saving the file                           |
| **MOVEMENT**                   |      |                                                              |
| h/l                            | n    | Moves the cursor to the left/right                           |
| j/k                            | n    | Moves the cursor up/down one line                            |
| H                              | n    | Puts the cursor at the top of the screen                     |
| M                              | n    | Puts the cursor in the middle of the screen                  |
| L                              | n    | Puts the cursor at the bottom of the screen                  |
| w                              |      | Puts the cursor at the start of the next word                |
| b                              |      | Puts the cursor at the start of the previous word            |
| e                              |      | Puts the cursor at the end of a word                         |
| O                              |      | Places the cursor at the beginning of a line                 |
| $                              |      | Places the cursor at the end of a line                       |
| )                              |      | Takes you to the start of the next sentence                  |
| (                              |      | Takes you to the start of the previous sentence              |
| }                              |      | Takes you to the start of the next paragraph or block of text |
| {                              |      | Takes you to the start of the previous paragraph or block of text |
| C-f                            |      | Takes you one page forward                                   |
| C-b                            |      | Takes you one page back                                      |
| gg                             |      | Places the cursor at the start of the file                   |
| G                              |      | Places the cursor at the end of the file                     |
| #                              |      | takes you to the line specified (#: line number.)            |
| **Editing**                    |      |                                                              |
| a                              |      | Append text after the cursor [count] times.                  |
| A                              |      | Append text at the end of the line [count] times.            |
| i                              |      | Insert text before the cursor [count] times.                 |
| I                              |      | Insert text before the first non-blank in the line [count] times. |
| yy                             |      | Copies a line. `5yy` copies 5 lines                          |
| yw                             |      | Copies a word                                                |
| y$                             |      | Copies from cursor to the end of a line                      |
| v                              |      | Highlight one character at a time                            |
| V                              |      | Highlights one line                                          |
| p                              |      | Paste whatever has been copied to the unnamed register       |
| d                              |      | Deletes highlighted text                                     |
| dd                             |      | Deletes a line of text. `5dd` deletes 5 lines from cursor position |
| dw                             |      | Deletes a word                                               |
| D                              |      | Deletes everything from cursor to the end of the line        |
| d0                             |      | Deletes everything from cursor to the beginning of the line  |
| dgg                            |      | Deletes everything from cursor to the beginning of the file  |
| dG                             |      | Deletes everything from cursor to the end of the file        |
| x                              |      | Deletes a single character                                   |
| u                              |      | Undo the last operation; `u#` will undo multiple actions     |
| C-r                            |      | Redo last undo                                               |
| .                              |      | Repeat the last action                                       |
| **Searching**                  |      |                                                              |
| /[keyword]                     |      | Searches forward for the *keyword* in the document           |
| ?[keyword]                     |      | Searches backwards for the *keyword* in the document         |
| n                              |      | Searches again *in the direction* of your last search        |
| N                              |      | Searches again in the opposite direction of your last search |
| :%s/[pattern]/[replacement]/g  |      | This replaces all occurrences of a pattern without confirming each one |
| :%s/[pattern]/[replacement]/gc |      | Replaces all occurrences of a pattern and confirms each one  |
| **Multiple files**             |      |                                                              |
| :bn                            | n    | Switch to next buffer                                        |
| :bp                            | n    | Switch to previous buffer                                    |
| :bd                            | n    | Close a buffer                                               |
| :sp [filename]                 | n    | Opens a new file and splits your screen horizontally         |
| :vsp [filename]                | n    | Opens a new file and splits your screen vertically           |
| :ls                            | n    | Lists all open buffers                                       |
| <`C-w`>s                       | n    | Split windows horizontally                                   |
| <`C-w`>v                       | n    | Split windows vertically                                     |
| <`C-w`>w                       | n    | Switch between windows                                       |
| <`C-w`>q                       | n    | Quit a window                                                |
| <`C-w`>h / <`C-w`>l            | n,i  | Moves your cursor to the window to the left/right            |
| <`C-w`>j / <`C-w`>k            | n,i  | Moves your cursor to the window below/above                  |
| **Marking Text**               |      |                                                              |
| v                              | v    | starts visual mode, then select a range of text with hjkl    |
| V                              | v    | starts linewise visual mode                                  |
| C-v                            | v    | starts visual block mode (selects columns)                   |
| ab                             | v    | a block with ()                                              |
| aB                             | v    | a block with {}                                              |
| ib                             | v    | inner block with ()                                          |
| iB                             | v    | inner block with {}                                          |
| aw                             | v    | mark a word                                                  |
| Esc                            | v    | exit visual mode                                             |
| *For selected text*            |      |                                                              |
| d                              | v    | delete marked text                                           |
| y                              | v    | yank (copy) marked text                                      |
| >                              | v    | shift text right                                             |
| <                              | v    | shift text left                                              |
| ~                              | v    | swap case (upper or lower)                                   |
| **Tab pages**                  |      |                                                              |
| :tabedit file                  |      | opens a new tab and will take you to edit "file"             |
| gt                             |      | move to the next tab                                         |
| gT                             |      | move to the previous tab                                     |
| #gt                            |      | move to a specific tab number #                              |
| :tabs                          |      | list all tabs                                                |
| :tabclose                      |      | close a single tab                                           |

