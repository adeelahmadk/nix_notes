# Linux Command Reference



## Filters

### more

**`more [-dlfpcsu] [-num] [+/pattern] [+linenum] [file ...]`**

More is a filter for paging through text one screen-full at a time.

| Option | Description |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `-num` | This option specifies an integer which is the screen size (in lines). |
| `-d`   | more will prompt the user with the message "[Press space to continue, 'q' to quit.]" and will display "[Press 'h' for instructions.]" instead of ringing the bell when an illegal key is pressed |
| `-l`   | more usually treats ^L (form feed) as a special character, and will pause after any line that contains a form feed.  The `-l` option will prevent this behavior. |
| `-f`   | Causes more to count logical, rather than screen lines (i.e., long lines are not folded). |
| `-p`   | Do not scroll.  Instead, clear the whole screen and then display the text. |
| `-c`   | Do not scroll. Instead, paint each screen from the top, clearing the remainder of each line as it is displayed. |
| `-s`   | Squeeze multiple blank lines into one. |
| `-u`   | Suppress underlining. |
| `+/`   | The +/ option specifies a string that will be searched for before each file is displayed. |
| `+num` | Start at line number num. |

### less - opposite of more

Less is a program similar to more (1), but which allows backward movement in the file as well as forward movement.

### zless

Usage: **`/bin/zless [OPTION]... [FILE]...`**
Like 'less', but operate on the uncompressed contents of any compressed FILEs. Options are the same as for 'less'.

### cat

Concatenate FILE(s), or standard input, to standard output.

| Option                    | Description                                   |
| -------------------------- | --------------------------------------------- |
| `-A`, `--show-all`         | equivalent to -vET                            |
| `-b`, `--number-nonblank`  | number nonempty output lines                  |
| `-e`                       | equivalent to -vE                             |
| `-E`, `--show-ends`        | display $ at end of each line                 |
| `-n`, `--number`           | number all output lines                       |
| `-s`, `--squeeze-blank`    | suppress repeated empty output lines          |
| `-t`                       | equivalent to -vT                             |
| `-T`, `--show-tabs`        | display TAB characters as ^I                  |
| `-u`                       | (ignored)                                     |
| `-v`, `--show-nonprinting` | use ^ and M- notation, except for LFD and TAB |

### zcat

Usage: **`/bin/zcat [OPTION]... [FILE]...`**
Uncompress FILEs to standard output.
| Option                    | Description                                   |
| -------------------------- | --------------------------------------------- |
| `-f`, `--force` | force; read compressed data even from a terminal |
| `-l`, `--list` | list compressed file contents |
| `-q`, `--quiet` | suppress all warnings |
| `-r`, `--recursive` | operate recursively on directories |
| `-S`, `--suffix=SUF` | use suffix SUF on compressed files |
| `--synchronous` | synchronous output (safer if system crashes, but slower) |
| `-t`, `--test` | test compressed file integrity |
| `-v`, `--verbose` | verbose mode |
| `--help` | display this help and exit |
| `--version` | display version information and exit |

With no FILE, or when FILE is -, read standard input.

### head

**`head [OPTION]... [FILE]...`**

Print the first 10 lines of each FILE to standard output.

| Option                    | Description                                   |
| -------------------------- | --------------------------------------------- |
| `-n`, `--lines=[-]N` | print the first N lines instead of the first 10; with the leading `-', print all but the last N lines of each file |

### tail

**`tail [OPTION]... [FILE]...`**

Print the last 10 lines of each FILE to standard output.

| Option                    | Description                                   |
| -------------------------- | --------------------------------------------- |
| `-n`, `--lines=N` | output the last N lines, instead of the last 10; or use +N to output lines starting with the Nth |

## Text Processing

### grep
**`grep [OPTION]... PATTERN [FILE]...`**

Search for PATTERN in each FILE or standard input. PATTERN is, by default, a basic regular expression (BRE).
Example: `grep -i 'hello world' menu.h main.c`

  Regexp selection and interpretation:
| Option                    | Description                                   |
| -------------------------- | --------------------------------------------- |
| `-E`, `--extended-regexp` | PATTERN is an extended  regular expression (ERE) |
| `-F`, `--fixed-strings` | PATTERN is a set of newline-separated fixed strings |
| `-G`, `--basic-regexp` | PATTERN is a basic regular expression (BRE) |
| `-P`, `--perl-regexp` | PATTERN is a Perl regular expression |
| `-e`, `--regexp=PATTERN` | use PATTERN for matching |
| `-f`, `--file=FILE` | obtain PATTERN from FILE |
| `-i`, `--ignore-case` |         ignore case distinctions |
| `-w`, `--word-regexp` | force PATTERN to match only whole words |
| `-x`, `--line-regexp` | force PATTERN to match only whole lines |
| `-z`, `--null-data` | a data line ends in 0 byte, not newline |

  Miscellaneous:
| Option                    | Description                                   |
| -------------------------- | --------------------------------------------- |
| `-s`, `--no-messages` | suppress error messages |
| `-v`, `--invert-match` | select non-matching lines |

#### Examples
Search for a VGA or 3D Controller in the ouput of previous command on stdin:
```sh
sudo lspci -nn | grep -i 'vga\|3d[ A-Za-z0-9]*controller'
00:02.0 VGA compatible controller [0300]: Intel Corporation xxxxx
01:00.0 3D controller [0302]: NVIDIA Corporation xxxxx
```
Search for storage devices `sda`,`sdb`,... or `hda`,`hdb`,... in kernel message ring:
```sh
sudo dmesg | egrep '(s|h)d[a-z]'
```
For more examples, refer to the [shell script](sh-script.md) reference.

### cut
**`cut OPTION... [FILE]...`**

Print selected parts of lines from each FILE to standard output. With no FILE, or when FILE is -, read standard input.

Mandatory arguments to long options are mandatory for short options too.
| Option                    | Description                                   |
| -------------------------- | --------------------------------------------- |
| `-b`, `--bytes=LIST` | select only these bytes |
| `-c`, `--characters=LIST` | select only these characters |
| `-d`, `--delimiter=DELIM` | use DELIM instead of TAB for field delimiter |
| `-f`, `--fields=LIST` | select only these fields;  also print any line that contains no delimiter character, unless the -s option is specified |
| `-n` | (ignored) |
| `--complement` | complement the set of selected bytes, characters or fields |
| `-s`, `--only-delimited` | do not print lines not containing delimiters |
| `--output-delimiter=STRING` | use STRING as the output delimiter the default is to use the input delimiter |
| `-z`, `--zero-terminated` | line delimiter is NUL, not newline |
| `--help` | display this help and exit |
| `--version` | output version information and exit |

Use one, and only one of `-b`, `-c` or `-f`.  Each LIST is made up of one range, or many ranges separated by commas.  Selected input is written in the same order that it is read, and is written exactly once.

Each range is one of:
| Option                    | Description                                   |
| -------------------------- | --------------------------------------------- |
| `N` | N'th byte, character or field, counted from 1 |
| `N-` | from N'th byte, character or field, to end of line |
| `N-M` | from N'th to M'th (included) byte, character or field |
| `-M` | from first to M'th (included) byte, character or field |

#### Examples
Print `username` field from `passwd` file with `:` as field separator:
```sh
cut -d ":" -f1 /etc/passwd
```
### echo

**`echo [SHORT-OPTION]... [STRING]...`**

**`echo LONG-OPTION`**

Echo the STRING(s) to standard output.
| Option                    | Description                                   |
| -------------------------- | --------------------------------------------- |
| -n | do not output the trailing newline |
| -e | enable interpretation of backslash escapes |
| -E | disable interpretation of backslash escapes (default) |
| --help | display this help and exit |
| --version | output version information and exit |

  If -e is in effect, the following sequences are recognized:
| Sequence                    | Description                                   |
| -------------------------- | --------------------------------------------- |
| \0NNN | the character whose ASCII code is NNN (octal) |
| \\ | backslash |
| \a | alert (BEL) |
| \b | backspace |
| \c | produce no further output |
| \f | form feed |
| \n | new line |
| \r | carriage return |
| \t | horizontal tab |
| \v | vertical tab |

#### Examples

```sh
echo this is sentence.
this is sentence.
```

```sh
echo -e 'my new file!\nredirecting standard output...' > myfile
cat myfile
my new file!
redirecting standard output...
```

### xxd: Hex dump

**`xxd [options] [infile [outfile]]`**

**`xxd -r [-s [-]offset] [-c cols] [-ps] [infile [outfile]]`**

xxd  creates a hex dump of a given file or standard input.  It can also convert a hex dump back to its original binary form.  Like  uuencode(1) and  uudecode(1)  it allows the transmission of binary data in a `mail-
safe' ASCII representation, but has the advantage of decoding to  standard output.  Moreover, it can be used to perform binary file patching.

Options:
| Option                    | Description                                   |
| -------------------------- | --------------------------------------------- |
| -a | toggle autoskip: A single '*' replaces nul-lines. Default off. |
| -b | binary digit dump (incompatible with -ps,-i,-r). Default hex. |
| -c cols | format <cols> octets per line. Default 16 (-i: 12, -ps: 30). |
| -E | show characters in EBCDIC. Default ASCII. |
| -e | little-endian dump (incompatible with -ps,-i,-r). |
| -g | number of octets per group in normal output. Default 2 (-e: 4). |
| -h | print this summary. |
| -i | output in C include file style. |
| -l len | stop after <len> octets. |
| -o off | add <off> to the displayed file position. |
| -ps | output in postscript plain hexdump style. |
| -r | reverse operation: convert (or patch) hexdump into binary. |
| -r -s off | revert with <off> added to file positions found in hexdump. |
| -s [+][-]seek |  start at <seek> bytes abs. (or +: rel.) infile offset. |
| -u | use upper case hex letters. |
| -v | show version: "xxd V1.10 27oct98 by Juergen Weigert". |


## File/Directory Processing

### diff

**`diff [OPTION]... FILES`**

Compare files line by line.

| Option                    | Description                                   |
| -------------------------- | --------------------------------------------- |
| -i  --ignore-case | Ignore case differences in file contents. |
| --ignore-file-name-case | Ignore case when comparing file names. |
| --no-ignore-file-name-case | Consider case when comparing file names. |
| -E  --ignore-tab-expansion | Ignore changes due to tab expansion. |
| -b  --ignore-space-change | Ignore changes in the amount of white space. |
| -w  --ignore-all-space | Ignore all white space. |
| -B  --ignore-blank-lines | Ignore changes whose lines are all blank. |
| -a  --text | Treat all files as text. |
| -u  -U NUM  --unified[=NUM] | Output NUM (default 3) lines of unified context. |
| -q  --brief | Output only whether files differ. |

### Determine type: file

**`file [OPTION...] [FILE...]`**

Determine type of FILEs.

| Option                    | Description                                   |
| -------------------------- | --------------------------------------------- |
| --help | display this help and exit |
| -v, --version | output version information and exit |
| -m, --magic-file LIST | use LIST as a colon-separated list of magic number files |
| -z, --uncompress | try to look inside compressed files |
| -b, --brief | do not prepend filenames to output lines |

### Make directory: mkdir

**`mkdir [OPTION]... DIRECTORY...`**

Create the DIRECTORY(ies), if they do not already exist.

Mandatory arguments to long options are mandatory for short options too.
  -m, --mode=MODE   set file mode (as in chmod), not a=rwx - umask
  -p, --parents     no error if existing, make parent directories as needed
  -v, --verbose     print a message for each created directory
  -Z, --context=CTX  set the SELinux security context of each created
                      directory to CTX
      --help     display this help and exit
      --version  output version information and exit

### Remove directory: rmdir

**`rmdir [OPTION]... DIRECTORY...`**

Remove the DIRECTORY(ies), if they are empty.

      --ignore-fail-on-non-empty
                  ignore each failure that is solely because a directory
                    is non-empty
  -p, --parents   remove DIRECTORY and its ancestors; e.g., `rmdir -p a/b/c' is
                    similar to `rmdir a/b/c a/b a'
  -v, --verbose   output a diagnostic for every directory processed
      --help     display this help and exit
      --version  output version information and exit

### Remove file(s) or directories: rm

**`rm [OPTION]... FILE...`**

Remove (unlink) the FILE(s) or directories.

  -f, --force           ignore nonexistent files, never prompt
  -i                    prompt before every removal
  -I                    prompt once before removing more than three files, or
                          when removing recursively.  Less intrusive than -i,
                          while still giving protection against most mistakes
      --interactive[=WHEN]  prompt according to WHEN: never, once (-I), or
                          always (-i).  Without WHEN, prompt always
      --one-file-system  when removing a hierarchy recursively, skip any
                          directory that is on a file system different from
                          that of the corresponding command line argument
      --no-preserve-root  do not treat `/' specially
      --preserve-root   do not remove `/' (default)
  -r, -R, --recursive   remove directories and their contents recursively
  -v, --verbose         explain what is being done
      --help     display this help and exit
      --version  output version information and exit

### Copy: cp

**`cp [OPTION]... [-T] SOURCE DEST`**

**`cp [OPTION]... SOURCE... DIRECTORY`**

**`cp [OPTION]... -t DIRECTORY SOURCE...`**

Copy SOURCE to DEST, or multiple SOURCE(s) to DIRECTORY.

Mandatory arguments to long options are mandatory for short options too.
      --backup[=CONTROL]       make a backup of each existing destination file
  -b                           like --backup but does not accept an argument
  -a, --archive                same as -dR --preserve=all
  -d                           same as --no-dereference --preserve=links
  -f, --force                  if an existing destination file cannot be
                                 opened, remove it and try again (redundant if
                                 the -n option is used)
  -i, --interactive            prompt before overwrite (overrides a previous -n
                                  option)
  -H                           follow command-line symbolic links in SOURCE
  -l, --link                   link files instead of copying
  -L, --dereference            always follow symbolic links in SOURCE
  -n, --no-clobber             do not overwrite an existing file (overrides
                                 a previous -i option)
  -P, --no-dereference         never follow symbolic links in SOURCE
  -p                           same as --preserve=mode,ownership,timestamps
      --preserve[=ATTR_LIST]   preserve the specified attributes (default:
                                 mode,ownership,timestamps), if possible
                                 additional attributes: context, links, xattr,
                                 all
  -R, -r, --recursive          copy directories recursively
  -s, --symbolic-link          make symbolic links instead of copying
  -S, --suffix=SUFFIX          override the usual backup suffix
  -t, --target-directory=DIRECTORY  copy all SOURCE arguments into DIRECTORY
  -u, --update                 copy only when the SOURCE file is newer
                                 than the destination file or when the
                                 destination file is missing
  -v, --verbose                explain what is being done

### Move or Rename: mv

**`mv [OPTION]... [-T] SOURCE DEST`**

**`mv [OPTION]... SOURCE... DIRECTORY`**

**`mv [OPTION]... -t DIRECTORY SOURCE...`**

Rename SOURCE to DEST, or move SOURCE(s) to DIRECTORY.

Mandatory arguments to long options are mandatory for short options too.
      --backup[=CONTROL]       make a backup of each existing destination file
  -b                           like --backup but does not accept an argument
  -f, --force                  do not prompt before overwriting
  -i, --interactive            prompt before overwrite
  -n, --no-clobber             do not overwrite an existing file
If you specify more than one of -i, -f, -n, only the final one takes effect.
  -S, --suffix=SUFFIX          override the usual backup suffix
  -u, --update                 move only when the SOURCE file is newer
                                 than the destination file or when the
                                 destination file is missing
  -v, --verbose                explain what is being done
      --help     display this help and exit


### Concatenate File(s): pv

pv [OPTION] [FILE]...

Concatenate FILE(s), or standard input, to standard output,
with monitoring.

  -p, --progress           show progress bar
  -t, --timer              show elapsed time
  -e, --eta                show estimated time of arrival (completion)
  -I, --fineta             show absolute estimated time of arrival 
                           (completion)
  -r, --rate               show data transfer rate counter
  -a, --average-rate       show data transfer average rate counter
  -b, --bytes              show number of bytes transferred
  -T, --buffer-percent     show percentage of transfer buffer in use
  -A, --last-written NUM   show NUM bytes last written
  -F, --format FORMAT      set output format to FORMAT
  -n, --numeric            output percentages, not visual information
  -q, --quiet              do not output any transfer information at all

  -W, --wait               display nothing until first byte transferred
  -D, --delay-start SEC    display nothing until SEC seconds have passed
  -s, --size SIZE          set estimated data size to SIZE bytes
  -l, --line-mode          count lines instead of bytes
  -0, --null               lines are null-terminated
  -i, --interval SEC       update every SEC seconds
  -w, --width WIDTH        assume terminal is WIDTH characters wide
  -H, --height HEIGHT      assume terminal is HEIGHT rows high
  -N, --name NAME          prefix visual information with NAME
  -f, --force              output even if standard error is not a terminal
  -c, --cursor             use cursor positioning escape sequences

  -L, --rate-limit RATE    limit transfer to RATE bytes per second
  -B, --buffer-size BYTES  use a buffer size of BYTES
  -C, --no-splice          never use splice(), always use read/write
  -E, --skip-errors        skip read errors in input
  -S, --stop-at-size       stop after --size bytes have been transferred
  -R, --remote PID         update settings of process PID

  -P, --pidfile FILE       save process ID in FILE

  -d, --watchfd PID[:FD]   watch file FD opened by process PID

  -h, --help               show this help and exit
  -V, --version            show version information and exit


### Change Permissions: chmod

**`chmod [OPTION]... MODE[,MODE]... FILE...`**

**`chmod [OPTION]... OCTAL-MODE FILE...`**

**`chmod [OPTION]... --reference=RFILE FILE...`**

Change the mode of each FILE to MODE.

  -c, --changes           like verbose but report only when a change is made
      --no-preserve-root  do not treat `/' specially (the default)
      --preserve-root     fail to operate recursively on `/'
  -f, --silent, --quiet   suppress most error messages
  -v, --verbose           output a diagnostic for every file processed
      --reference=RFILE   use RFILE's mode instead of MODE values
  -R, --recursive         change files and directories recursively
      --help     display this help and exit
      --version  output version information and exit

Each MODE is of the form `[ugoa]*([-+=]([rwxXst]*|[ugo]))+`.

#### SETUID and SETGID Permissions

Setting set user ID permissions sets the effective user ID of the current process or file, the process executing the file takes on the privileges of the file's owner. Similarly, a setgid permissions gives the process executing the file the privileges of the group the file is associated with. For example:

```sh
ls -l
-rwxr-xr-x 1 root root 0 2012-02-15 17:09 myprog1
-rwxr-xr-x 1 root root 0 2012-02-15 17:09 myprog2
chomd u+s myprog1
chmod g+s myprog2
```

Or,

```sh
chomd 4755 myprog1
chmod 2755 myprog2
```

in case of four digit permissions, first digit sets/unsets following:
  - 1: Sticky bit
  - 2: setgid permission
  - 4: setuid permission

as a result, file permissions look like:

```sh
ls -l
-rwsr-xr-x 1 root root 0 2012-02-15 17:09 myprog1
-rwxr-sr-x 1 root root 0 2012-02-15 17:09 myprog2
```

Executable files that are setuid and owned by root have root privileges when they run, even if they are not run by root. Similarly executable files that are setgid and belong to the group root have extensive privileges.

#### Examples

make file inaccessible for others and all users:
```sh
-rw-r--r-- 1 root root 135K 2012-02-15 13:11 bin.lst
chmod 600 bin.lst
-rw------- 1 root root 135K 2012-02-15 13:11 bin.lst
```

grant read/write access to all users using symbolic arguments:
```sh
chmod a+rw bin.lst
-rw-rw-rw- 1 root root 135K 2012-02-15 13:11 bin.lst
```

File access permissions are read as `ugo` i.e. `u`: User/Owner, `g`: Group `o`: Others. `a` refers to All users.

### Change group membership: chgrp

**`chgrp [OPTION]... GROUP FILE...`**

**`chgrp [OPTION]... --reference=RFILE FILE...`**

Change the group of each FILE to GROUP.
With --reference, change the group of each FILE to that of RFILE.

  -c, --changes          like verbose but report only when a change is made
      --dereference      affect the referent of each symbolic link (this is
                         the default), rather than the symbolic link itself
  -h, --no-dereference   affect each symbolic link instead of any referenced
                         file (useful only on systems that can change the
                         ownership of a symlink)
      --no-preserve-root  do not treat `/' specially (the default)
      --preserve-root    fail to operate recursively on `/'
  -f, --silent, --quiet  suppress most error messages
      --reference=RFILE  use RFILE's group rather than specifying a
                         GROUP value
  -R, --recursive        operate on files and directories recursively
  -v, --verbose          output a diagnostic for every file processed

The following options modify how a hierarchy is traversed when the -R
option is also specified.  If more than one is specified, only the final
one takes effect.

  -H                     if a command line argument is a symbolic link
                         to a directory, traverse it
  -L                     traverse every symbolic link to a directory
                         encountered
  -P                     do not traverse any symbolic links (default)

      --help     display this help and exit
      --version  output version information and exit

Examples:

```sh
#Change the group of /u to "staff"
chgrp staff /u
#Change the group of /u and subfiles to "staff"
chgrp -hR staff /u
```

### Update the access and modification times: touch

**`touch [OPTION]... FILE...`**

Update the access and modification times of each FILE to the current time.

A FILE argument that does not exist is created empty.

A FILE argument string of - is handled specially and causes touch to
change the times of the file associated with standard output.

Mandatory arguments to long options are mandatory for short options too.
  -a                     change only the access time
  -c, --no-create        do not create any files
  -d, --date=STRING      parse STRING and use it instead of current time
  -f                     (ignored)
  -m                     change only the modification time
  -r, --reference=FILE   use this file's times instead of current time
  -t STAMP               use [[CC]YY]MMDDhhmm[.ss] instead of current time
  --time=WORD            change the specified time:
                           WORD is access, atime, or use: equivalent to -a
                           WORD is modify or mtime: equivalent to -m
      --help     display this help and exit
      --version  output version information and exit

Note that the -d and -t options accept different time-date formats.

### Copy standard input to file(s): tee

**`tee [OPTION]... [FILE]...`**

Copy standard input to each FILE, and also to standard output.

-a, --append
    append to the given FILEs, do not overwrite
-i, --ignore-interrupts
    ignore interrupt signals
--help
    display this help and exit
--version
    output version information and exit

Examples:
	echo "Hello world!" | tee -a out.dmp

### Format Conversion

#### To DOS: todos

**`todos [options] [file...]`**

Converts text files between DOS and Unix formats.

  -a      Always convert (DOS to Unix: kill all CRs;
	  Unix to DOS: convert all LFs to CRLFs)
  -b      Make backup of original file (.bak).
  -d      Convert DOS to Unix.
  -e      Abort processing files on error in any file.
  -f      Force: convert even if file is not writeable.
  -h      Display help on usage and quit.
  -l file Log most errors and verbose messages to <file>
  -o      Overwrite original file (no backup).
  -p      Preserve file owner and time.
  -u      Convert Unix to DOS.

#### UNIX to DOS/MAC: unix2dos

**`unix2dos [-fhkLlqV] [-c convmode] [-o file ...] [-n infile outfile ...]`**

UNIX to DOS/MAC text file format converter.

  -c --convmode    conversion mode
    convmode       ascii, 7bit, iso, mac, default to ascii
  -f --force       force conversion of all files
  -h --help        give this help
  -k --keepdate    keep output file date
  -L --license     display software license
  -l --newline     add additional newline
  -n --newfile     write to new file
    infile         original file in new file mode
    outfile        output file in new file mode
  -o --oldfile     write to old file
    file ...       files to convert in old file mode
  -q --quiet       quiet mode, suppress all warnings
		  always on in stdio mode

#### Between DOS and Unix: fromdos

**`fromdos [options] [file...]`**

Converts text files between DOS and Unix formats.

  -a      Always convert (DOS to Unix: kill all CRs;
	  Unix to DOS: convert all LFs to CRLFs)
  -b      Make backup of original file (.bak).
  -d      Convert DOS to Unix.
  -e      Abort processing files on error in any file.
  -f      Force: convert even if file is not writeable.
  -h      Display help on usage and quit.
  -l file Log most errors and verbose messages to <file>
  -o      Overwrite original file (no backup).
  -p      Preserve file owner and time.
  -u      Convert Unix to DOS.
  -v      Verbose.
  -V      Show version and quit.

#### DOS/MAC to UNIX: dos2unix

**`dos2unix [-fhkLlqV] [-c convmode] [-o file ...] [-n infile outfile ...]`**

DOS/MAC to UNIX text file format converter.

  -c --convmode    conversion mode
    convmode       ascii, 7bit, iso, mac, default to ascii
  -f --force       force conversion of all files
  -h --help        give this help
  -k --keepdate    keep output file date
  -L --license     display software license
  -l --newline     add additional newline
  -n --newfile     write to new file
    infile         original file in new file mode
    outfile        output file in new file mode
  -o --oldfile     write to old file
    file ...       files to convert in old file mode
  -q --quiet       quiet mode, suppress all warnings
		    always on in stdio mode
  -V --version     display version number

### Archiving

#### tar: Tape Archive

**`tar [OPTION...] [FILE]...`**

GNU `tar' saves many files together into a single tape or disk archive, and can
restore individual files from the archive.

Examples:
  tar -cf archive.tar foo bar  # Create archive.tar from files foo and bar.
  tar -tvf archive.tar         # List all files in archive.tar verbosely.
  tar -xf archive.tar          # Extract all files from archive.tar.
  tar xvjf archive.tar.bz2     # Extract all files from bz2 compressed archive
  tar xvzf archive.tar.gz      # Extract all files from gz compressed archive

 Main operation mode:

  -A, --catenate, --concatenate   append tar files to an archive
  -c, --create               create a new archive
  -d, --diff, --compare      find differences between archive and file system
      --delete               delete from the archive (not on mag tapes!)
  -r, --append               append files to the end of an archive
  -t, --list                 list the contents of an archive
      --test-label           test the archive volume label and exit
  -u, --update               only append files newer than copy in archive
  -x, --extract, --get       extract files from an archive

Device selection and switching:

  -f, --file=ARCHIVE         use archive file or device ARCHIVE
      --force-local          archive file is local even if it has a colon

Compression options:

  -a, --auto-compress        use archive suffix to determine the compression
                             program
  -I, --use-compress-program=PROG
                             filter through PROG (must accept -d)
  -j, --bzip2                filter the archive through bzip2
      --lzma                 filter the archive through lzma
      --no-auto-compress     do not use archive suffix to determine the
                             compression program
  -z, --gzip, --gunzip, --ungzip   filter the archive through gzip
  -Z, --compress, --uncompress   filter the archive through compress

  -J, --xz                   filter the archive through xz
      --lzop                 filter the archive through lzop

Examples:

  To create a tar.gz archive from a given folder

```sh
tar -zcvf tar-archive-name.tar.gz source-folder-name.
```

  To extract a tar.gz compressed archive you can use

```sh
tar -zxvf tar-archive-name.tar.gz.
```

To remove leading dir tree and exclude a file while extracting an archive:

```sh
tar -xzf /tmp/myarch.tar.gz myarch/docker --exclude=myarch/docker/Makefile --strip-components=3
```



#### gzip

**`gzip [OPTION]... [FILE]...`**

Compress or uncompress FILEs (by default, compress FILES in-place).

Mandatory arguments to long options are mandatory for short options too.

  -c, --stdout      write on standard output, keep original files unchanged
  -d, --decompress  decompress
  -f, --force       force overwrite of output file and compress links
  -h, --help        give this help
  -l, --list        list compressed file contents
  -L, --license     display software license
  -n, --no-name     do not save or restore the original name and time stamp
  -N, --name        save or restore the original name and time stamp
  -q, --quiet       suppress all warnings
  -r, --recursive   operate recursively on directories
  -S, --suffix=SUF  use suffix SUF on compressed files
  -t, --test        test compressed file integrity
  -v, --verbose     verbose mode
  -V, --version     display version number
  -1, --fast        compress faster
  -9, --best        compress better
    --rsyncable   Make rsync-friendly archive

#### bzip2

bzip2 [flags and input files in any order]

A block-sorting file compressor.

   -h --help           print this message
   -d --decompress     force decompression
   -z --compress       force compression
   -k --keep           keep (don't delete) input files
   -f --force          overwrite existing output files
   -t --test           test compressed file integrity
   -c --stdout         output to standard out
   -q --quiet          suppress noncritical error messages
   -v --verbose        be verbose (a 2nd -v gives more)
   -L --license        display software version & license
   -V --version        display software version & license
   -s --small          use less memory (at most 2500k)
   -1 .. -9            set block size to 100k .. 900k
   --fast              alias for -1
   --best              alias for -9

   If invoked as `bzip2', default action is to compress.
              as `bunzip2',  default action is to decompress.
              as `bzcat', default action is to decompress to stdout.

##### Examples

unpack a .tar.bz2

```sh
bunzip2 -c all.tar.bz2 | tar -xvf -
```

#### zip

**`zip  [-aABcdDeEfFghjklLmoqrRSTuvVwXyz!@$]  [--longoption ...]  [-b path] [-n suffixes] [-t date]
[-tt date] [zipfile [file ...]]  [-xi list]
OR: zip [-options] [-b path] [-t mmddyyyy] [-n suffixes] [zipfile list] [-xi list]`**

zip - package and compress (archive) files
  The default action is to add or replace zipfile entries from list, which
  can include the special name - to compress standard input.
  If zipfile and list are omitted, zip compresses stdin to stdout.
  -f   freshen: only changed files  -u   update: only changed or new files
  -d   delete entries in zipfile    -m   move into zipfile (delete OS files)
  -r   recurse into directories     -j   junk (don't record) directory names
  -0   store only                   -l   convert LF to CR LF (-ll CR LF to LF)
  -1   compress faster              -9   compress better
  -q   quiet operation              -v   verbose operation/print version info
  -c   add one-line comments        -z   add zipfile comment
  -@   read names from stdin        -o   make zipfile as old as latest entry
  -x   exclude the following names  -i   include only the following names
  -F   fix zipfile (-FF try harder) -D   do not add directory entries
  -A   adjust self-extracting exe   -J   junk zipfile prefix (unzipsfx)
  -T   test zipfile integrity       -X   eXclude eXtra file attributes
  -y   store symbolic links as the link instead of the referenced file
  -e   encrypt                      -n   don't compress these suffixes
  -h2  show more help

Basic command line:
```sh
zip options archive_name file1 file2 ...
```
Some examples:
```sh
#Add file.txt to z.zip (create z if needed):
zip z file.txt
#Zip all files in current dir:
zip z *
#Zip files in current dir and subdirs also:
zip -r z .
```

Basic modes:
 External modes (selects files from file system):
        add      - add new files/update existing files in archive (default)
  -u    update   - add new files/update existing files only if later date
  -f    freshen  - update existing files only (no files added)
  -FS   filesync - update if date or size changed, delete if no OS match
 Internal modes (selects entries in archive):
  -d    delete   - delete files from archive (see below)
  -U    copy     - select files in archive to copy (use with --out)

Basic options:
  -r        recurse into directories (see Recursion below)
  -m        after archive created, delete original files (move into archive)
  -j        junk directory names (store just file names)
  -q        quiet operation
  -v        verbose operation (just "zip -v" shows version information)
  -c        prompt for one-line comment for each entry
  -z        prompt for comment for archive (end with just "." line or EOF)
  -@        read names to zip from stdin (one path per line)
  -o        make zipfile as old as latest entry

Deletion, File Sync:
  -d        delete files
  Delete archive entries matching internal archive paths in list
    zip archive -d pattern pattern ...
  Can use -t and -tt to select files in archive, but NOT -x or -i, so
    zip archive -d "*" -t 2005-12-27
  deletes all files from archive.zip with date of 27 Dec 2005 and later

  -FS       file sync
  Similar to update, but files updated if date or size of entry does not
  match file on OS.  Also deletes entry from archive if no matching file
  on OS.
    zip archive_to_update -FS -r dir_used_before
  WARNING:  -FS deletes entries so make backup copy of archive first

Compression:
  -0        store files (no compression)
  -1 to -9  compress fastest to compress best (default is 6)
  -Z cm     set compression method to cm:
              store   - store without compression, same as option -0
              deflate - original zip deflate, same as -1 to -9 (default)
            if bzip2 is enabled:
              bzip2 - use bzip2 compression (need modern unzip)

Encryption:
  -e        use standard (weak) PKZip 2.0 encryption, prompt for password
  -P pswd   use standard encryption, password is pswd

Splits (archives created as a set of split files):
  -s ssize  create split archive with splits of size ssize, where ssize nm
              n number and m multiplier (kmgt, default m), 100k -> 100 kB
  -sp       pause after each split closed to allow changing disks
      WARNING:  Archives created with -sp use data descriptors and should
                work with most unzips but may not work with some
  -sb       ring bell when pause
  -sv       be verbose about creating splits
      Split archives CANNOT be updated, but see --out and Copy Mode below

#### unzip

**`unzip [-Z] options archive[.zip] [file ...] [-x xfile ...] [-d exdir]`**

UnZip lists and extracts files in zip archives.

Some examples:
  unzip -l foo.zip        - list files in short format in archive foo.zip

  unzip -t foo            - test the files in archive foo

  unzip -Z foo            - list files using more detailed zipinfo format

  unzip foo               - unzip the contents of foo in current dir

  unzip -a foo            - unzip foo and convert text files to local OS

unzip options:
  -Z   Switch to zipinfo mode.  Must be first option.
  -hh  Display extended help.
  -A   [OS/2, Unix DLL] Print extended help for DLL.
  -c   Extract files to stdout/screen.  As -p but include names.  Also,
         -a allowed and EBCDIC conversions done if needed.
  -f   Freshen by extracting only if older file on disk.
  -l   List files using short form.
  -p   Extract files to pipe (stdout).  Only file data is output and all
         files extracted in binary mode (as stored).
  -t   Test archive files.
  -T   Set timestamp on archive(s) to that of newest file.  Similar to
       zip -o but faster.
  -u   Update existing older files on disk as -f and extract new files.
  -v   Use verbose list format.  If given alone as unzip -v show version
         information.  Also can be added to other list commands for more
         verbose output.
  -z   Display only archive comment.

unzip modifiers:
  -a   Convert text files to local OS format.  Convert line ends, EOF
         marker, and from or to EBCDIC character set as needed.
  -b   Treat all files as binary.  [Tandem] Force filecode 180 ('C').
         [VMS] Autoconvert binary files.  -bb forces convert of all files.
  -B   [UNIXBACKUP compile option enabled] Save a backup copy of each
         overwritten file in foo~ or foo~99999 format.
  -C   Use case-insensitive matching.
  -D   Skip restoration of timestamps for extracted directories.  On VMS this
         is on by default and -D essentially becames -DD.
  -DD  Skip restoration of timestamps for all entries.
  -E   [MacOS (not Unix Apple)]  Display contents of MacOS extra field during
         restore.
  -F   [Acorn] Suppress removal of NFS filetype extension.  [Non-Acorn if
         ACORN_FTYPE_NFS] Translate filetype and append to name.
  -i   [MacOS] Ignore filenames in MacOS extra field.  Instead, use name in
         standard header.
  -j   Junk paths and deposit all files in extraction directory.
  -J   [BeOS] Junk file attributes.  [MacOS] Ignore MacOS specific info.
  -K   [AtheOS, BeOS, Unix] Restore SUID/SGID/Tacky file attributes.
  -L   Convert to lowercase any names from uppercase only file system.
  -LL  Convert all files to lowercase.
  -M   Pipe all output through internal pager similar to Unix more(1).
  -n   Never overwrite existing files.  Skip extracting that file, no prompt.
  -N   [Amiga] Extract file comments as Amiga filenotes.
  -o   Overwrite existing files without prompting.  Useful with -f.  Use with
         care.
  -P p Use password p to decrypt files.  THIS IS INSECURE!  Some OS show
         command line to other users.
  -q   Perform operations quietly.  The more q (as in -qq) the quieter.
  -s   [OS/2, NT, MS-DOS] Convert spaces in filenames to underscores.
  -S   [VMS] Convert text files (-a, -aa) into Stream_LF format.
  -U   [UNICODE enabled] Show non-local characters as #Uxxxx or #Lxxxxxx ASCII
         text escapes where x is hex digit.  [Old] -U used to leave names
         uppercase if created on MS-DOS, VMS, etc.  See -L.
  -UU  [UNICODE enabled] Disable use of stored UTF-8 paths.  Note that UTF-8
         paths stored as native local paths are still processed as Unicode.
  -V   Retain VMS file version numbers.
  -W   [Only if WILD_STOP_AT_DIR] Modify pattern matching so ? and * do not
         match directory separator /, but ** does.  Allows matching at specific
         directory levels.
  -X   [VMS, Unix, OS/2, NT, Tandem] Restore UICs and ACL entries under VMS,
         or UIDs/GIDs under Unix, or ACLs under certain network-enabled
         versions of OS/2, or security ACLs under Windows NT.  Can require
         user privileges.
  -XX  [NT] Extract NT security ACLs after trying to enable additional
         system privileges.
  -Y   [VMS] Treat archived name endings of .nnn as VMS version numbers.
  -$   [MS-DOS, OS/2, NT] Restore volume label if extraction medium is
         removable.  -$$ allows fixed media (hard drives) to be labeled.
  -/ e [Acorn] Use e as extension list.
  -:   [All but Acorn, VM/CMS, MVS, Tandem] Allow extract archive members into
         locations outside of current extraction root folder.  This allows
         paths such as ../foo to be extracted above the current extraction
         directory, which can be a security problem.
  -^   [Unix] Allow control characters in names of extracted entries.  Usually
         this is not a good thing and should be avoided.
  -2   [VMS] Force unconditional conversion of names to ODS-compatible names.
         Default is to exploit destination file system, preserving cases and
         extended name characters on ODS5 and applying ODS2 filtering on ODS2.

Wildcards:
  Internally unzip supports the following wildcards:
    ?       (or %% or #, depending on OS) matches any single character
    *       matches any number of characters, including zero
    [list]  matches char in list (regex), can do range [ac-f], all but [!bf]
  If port supports [], must escape [ as [[]

Include and Exclude:
  -i pattern pattern ...   include files that match a pattern
  -x pattern pattern ...   exclude files that match a pattern
  Patterns are paths with optional wildcards and match paths as stored in
  archive.  Exclude and include lists end at next option or end of line.
    unzip archive -x pattern pattern ...

Multi-part (split) archives (archives created as a set of split files):
  Currently split archives are not readable by unzip.  A workaround is
  to use zip to convert the split archive to a single-file archive and
  use unzip on that. A split archive can also be converted into a
  single-file archive using a split size of 0 or negating the -s option:

    zip -s 0 split.zip --out single.zip



## System Administration

### Users & Groups

In Linux, `/etc/passwd` & `/etc/group` files hold information about users & groups in the system respectively.

#### useradd

To add/create a new user, all you’ve to follow the command ‘**useradd**‘ or ‘**adduser**‘ with ‘username’.

```sh
useradd newuser
# check passwd file for newuser
grep 'newuser' /etc/passwd
newuser:x:501:501:newuser,,,:/home/newuser:/bin/bash
```

The colon-separated file `/etc/passwd` has following fields:

1. **Username**: User login name used to login into system. It should be between 1 to 32 charcters long.
2. **Password**: User password (or x character) stored in `/etc/shadow` file in encrypted format.
3. **User ID (UID)**: Every user must have a User ID (UID) User Identification Number. By default UID 0 is reserved for root user and UID’s ranging from 1-99 are reserved for other predefined accounts. Further UID’s ranging from 100-999 are reserved for system accounts and groups.
4. **Group ID (GID)**: The primary Group ID (GID) Group Identification Number stored in `/etc/group` file.
5. **User Info**: This field is optional and allow you to define extra information about the user. For example, user full name. This field is filled by ‘finger’ command.
6. **Home Directory**: The absolute location of user’s home directory.
7. **Shell**: The absolute location of a user’s shell i.e. `/bin/bash`.

Some examples are as follows:

```sh
# Create a User with Different Home Directory
useradd -d /data/projects lea
# Create a User with Specific User ID
useradd -u 999 navin
# Create a User with Specific Group ID
useradd -u 1000 -g 500 ali
# Add a User to Multiple Groups
useradd -G admins,webadmin,developers mak
# Add a User without Home Directory
useradd -M arxivman
# Create a User with Account Expiry Date
useradd -e 2021-03-27 nyle
# Create a User with Password Expiry Date
useradd -f 45 nyle
# Change User Login Shell
useradd -s /sbin/nologin amir

```

Some advanced examples are:

```sh
# Add a User with Specific Home Directory, Default Shell, Custom Comment & a group with the same name as the user.
useradd -m -d /var/www/rami -s /bin/bash -c "Robotic Club Head" -U rami
# Add a User without Home Directory, No Shell, No Group and Custom Comment
useradd -M -N -r -s /bin/false -c "Disabled Member" mamnu
```



#### passwd

To assign or change the password of a user.

```sh
passwd mak
```

#### id

Print user and group information for the specified USER, or (when USER omitted) for the current user.

```sh
id lea
uid=996(lea) gid=996(lea) groups=996(lea),4(adm),27(sudo)
```

#### chage

The `chage` command changes the number of days between password changes and the date of the last password change. This information is used by the system to determine when a user must change their password.

| Option                       | Description                                                  |
| ---------------------------- | ------------------------------------------------------------ |
| -d, --lastday LAST_DAY       | set date of last password change to LAST_DAY                 |
| -E, --expiredate EXPIRE_DATE | set account expiration date to EXPIRE_DATE                   |
| -i, --iso8601                | use YYYY-MM-DD when printing dates                           |
| -I, --inactive INACTIVE      | set password inactive after expiration to INACTIVE           |
| -l, --list                   | show account aging information                               |
| -m, --mindays MIN_DAYS       | set minimum number of days before password                                change to MIN_DAYS |
| -M, --maxdays MAX_DAYS       | set maximum number of days before password                                change to MAX_DAYS |
| -R, --root CHROOT_DIR        | directory to chroot into                                     |
| -W, --warndays WARN_DAYS     | set expiration warning days to WARN_DAYS                     |



#### groupadd & groupdel

The quickest way to add a group in the command-line is with the `groupadd` command.

```sh
sudo groupadd newgroup
# or
sudo groupadd newgroup, newgroup2, newgroup3
```

If you have no use for a certain group then use the `groupdel` command to get rid of it.

```sh
sudo groupdel newgroup
```



#### usermod

`usermod` command is used to modify a user such as adding existing users to a newly created group.

```sh
# add a user to a group
sudo usermod -a -G newgroup username

groups
username sudo newgroup newgroup1

# remove a user from a group (assign the list of remaining groups)
sudo usermod -G sudo,newgroup1 username

groups
username sudo newgroup1
```
Some useful options are:

| Option                       | Description                                                  |
| ---------------------------- | ------------------------------------------------------------ |
| -c, --comment COMMENT        | new value of the GECOS field                                 |
| -d, --home HOME_DIR          | new home directory for the user account                      |
| -m, --move-home              | move contents of the home directory to the new location (use only with -d) |
| -e, --expiredate EXPIRE_DATE | set account expiration date to EXPIRE_DATE                   |
| -f, --inactive INACTIVE      | set password inactive after expiration to INACTIVE           |
| -g, --gid GROUP              | force use GROUP as new primary group                         |
| -G, --groups GROUPS          | new list of supplementary GROUPS                             |
| -a, --append                 | append the user to the supplemental GROUPS mentioned by the -G option without removing the user from other groups |
| -l, --login NEW_LOGIN        | new value of the login name                                  |
| -L, --lock                   | lock the user account                                        |
| -s, --shell SHELL            | new login shell for the user account                         |
| -u, --uid UID                | new UID for the user account                                 |
| -U, --unlock                 | unlock the user account                                      |



#### gpasswd

To remove a user from one specific group:

```sh
sudo gpasswd -d username newgroup1
```




#### userdel

To delete the user but preserve the Home directory:

```sh
userdel newuser
```

To delete the user and it Home directory:

```sh
userdel -r newuser
```



### Environment

#### date



#### type

**`type [-afptP] name [name ...]`**

Display information about command type. For each NAME, indicate how it would be interpreted if used as a
command name.

| Option                    | Description                                   |
| -------------------------- | --------------------------------------------- |
| `-a` |	display all locations containing an executable named NAME; includes aliases, builtins, and functions, if and only ifthe `-p` option is not also used |
| `-f` |	suppress shell function lookup |
| `-P` |	force a PATH search for each NAME, even if it is an alias, builtin, or function, and returns the name of the disk file that would be executed |
| `-p` |	returns either the name of the disk file that would be executed, or nothing if `type -t NAME' would not return `file' |
| `-t` |	output a single word which is one of `alias', `keyword', `function', `builtin', `file' or `', if NAME is an alias, shell reserved word, shell function, shell builtin, disk file, or not found, respectively |

| Argument                    | Description                                   |
| -------------------------- | --------------------------------------------- |
| NAME | Command name to be interpreted. |

Exit Status:

Returns success if all of the NAMEs are found; fails if any are not found.



### Security & Encryption

#### SSH Keys

##### Generate SSH keys

To generate your SSH keys, type the following command:

```sh
ssh-keygen
```

You will be asked where you wish your SSH keys to be stored (generally in `/home/user/.ssh/id_rsa` by default). Press the Enter key to accept the default location. The permissions on the folder will secure it for your use only. You will now be asked for a passphrase. Then SSH keys are generated and stored for you.

##### Installing the Public Key

We need to install your public key on the remote computer so that it knows that the public key belongs to you.

```sh
#ssh-copy-id [-h|-?|-f|-n] [-i [identity_file]] [-p port] [[-o <ssh -o options>] ...] [user@]hostname
ssh-copy-id -i rpi_id_rsa pi@raspberrypi.home
# or (x: host number in the subnet)
ssh-copy-id -i rpi_id_rsa pi@192.168.1.x
```



#### MD5

**`md5sum [OPTION]... [FILE]...`**

Print or check MD5 (128-bit) checksums.

With no FILE, or when FILE is -, read standard input.

  -b, --binary         read in binary mode
  -c, --check          read MD5 sums from the FILEs and check them
      --tag            create a BSD-style checksum
  -t, --text           read in text mode (default)

The following five options are useful only when verifying checksums:
      --ignore-missing  don't fail or report status for missing files
      --quiet          don't print OK for each successfully verified file
      --status         don't output anything, status code shows success
      --strict         exit non-zero for improperly formatted checksum lines
  -w, --warn           warn about improperly formatted checksum lines

      --help     display this help and exit
      --version  output version information and exit

##### Examples

```sh
#Get md5 check sum in a file:
$ md5sum file > file.md5

#Verify md5 checksum of a file, needs md5 sum stored in a file as input:
$ md5sum -c file.md5
```

#### SHA256

**`sha256sum [OPTION]... [FILE]...`**

Print or check SHA256 (256-bit) checksums.

With no FILE, or when FILE is -, read standard input.

  -b, --binary         read in binary mode
  -c, --check          read SHA256 sums from the FILEs and check them
      --tag            create a BSD-style checksum
  -t, --text           read in text mode (default)

The following five options are useful only when verifying checksums:
      --ignore-missing  don't fail or report status for missing files
      --quiet          don't print OK for each successfully verified file
      --status         don't output anything, status code shows success
      --strict         exit non-zero for improperly formatted checksum lines
  -w, --warn           warn about improperly formatted checksum lines

      --help     display this help and exit
      --version  output version information and exit

##### Examples

```sh
#Get sha256 check sum in a file:
sha256sum file > file.sha256

#Verify sha256 checksum of a file, needs sha256 sum stored in a file as input:
sha256sum -c file.sha256
```



### Package Management

#### `apt-key`

`Usage: apt-key [--keyring file] [command] [arguments]`

Manage `apt`'s list of trusted keys

| Option                   | Description                                         |
| ------------------------ | --------------------------------------------------- |
| apt-key add <`file`>     | add the key contained in <`file`> ('-' for `stdin`) |
| apt-key del <`keyid`>    | remove the key <`keyid`>                            |
| apt-key export <`keyid`> | output the key <`keyid`>                            |
| apt-key exportall        | output all trusted keys                             |
| apt-key update           | update keys using the keyring package               |
| apt-key net-update       | update keys using the network                       |
| apt-key list             | list keys                                           |
| apt-key finger           | list fingerprints                                   |
| apt-key adv              | pass advanced options to gpg (download key)         |

##### Examples

Add a gpg key for an upstream repository

```sh
curl -sS https://example.com/debian/pubkey.gpg | sudo apt-key add -
# example
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
```

Refresh/update all keys from a key server

```sh
sudo apt-key adv --refresh-keys --keyserver keyserver.ubuntu.com
```

Above command relies on package repository maintainers uploading their public keys to the **Ubuntu** *keyserver*, resort to the original `apt-key add ` command if they don't.

### Backup & Archiving

#### dd

Copy a file, converting and formatting according to the operands.



#### `rsync`

`rsync` stands for the term *remote sync*. Despite the name, it can handle file synchronization remotely and locally. The term `rsync` is also used to refer to the **rsync protocol** that `rsync` uses for syncing. Unlike other syncing tools, rsync uses an interesting algorithm that minimizes bandwidth consumption. It simply moves the portion of the file(s) that have changed (*i.e.* **differential sync**).

`rsync` uses the following command structure:

```
rsync <option> <src> <destination>
```

Tell `rsync` to sync the contents of both the directories recursively:

```sh
rsync -v -r source/ targe
```

`-a`: A combination flag that stands for “archive”, equals `-rlptgoD` 

| Option | Description                                                  |
| ------ | ------------------------------------------------------------ |
| `-r`   | recursive                                                    |
| `-l`   | symlinks as symlinks                                         |
| `-p`   | preserve permissions                                         |
| `-t`   | preserve modification times                                  |
| `-g`   | preserve group                                               |
| `-o`   | preserve owner (super-user only)                             |
| `-D`   | same as --devices --specials preserve device files (super-user only) & preserve special files |

If your backup location is somewhere remote, you can easily configure `rsync` to perform backup on the remote location through SSH. However, both the machines must have `rsync` installed. Moreover, both systems also need to have SSH keys set.

Here, this operation is called a “**push**” because it pushes a directory from the local system to a remote system. The opposite is known as “**pull**”.

```sh
# push: rsync -a <local_dir> <username>@<remote_host>:<destination_dir>
rsync -av backup/ user@node0.home:/home/user

# pull: rsync -a <username>@<remote_host>:<source_dir> <local_dir>
rsync -av user@node0.home:/home/user backup/
```

Rsync offers compression by default. To perform compressed sync, use the “-z” flag. `-P` flag combines the function of both “-progress” and “-partial” flags. The first one shows a progress bar and the second one is to enable resuming interrupted transfer. To force `rsync` delete files, use the “`–-delete`” flag. `--exclude`, `--exclude`, `--max-size` flags are self expressive:

```sh
rsync -avz <source> <destination>
rsync -avzP <source> <destination>
rsync -av --delete <source> <destination>
rsync -av --exclude=<pattern> --exclude=<pattern>
rsync -av --max-size='10k' <source> <destination>
```



### Processes & Protection

#### Capabilities

For the purpose of performing permission checks, traditional UNIX implementations distinguish two categories of processes: privileged processes (whose effective user ID is 0, referred to as superuser or root), and unprivileged processes (whose effective UID is nonzero). Privileged processes bypass all kernel permission checks, while unprivileged processes are subject to full permission checking based on the process's credentials (usually: effective UID, effective GID, and supplementary group list). 

Starting with kernel 2.2, Linux divides the privileges traditionally associated with superuser into distinct units, known as capabilities, which can be independently enabled and disabled. Capabilities are a per-thread attribute. 

For detailed list of capabilities refer to [man page](https://linux.die.net/man/7/capabilities).

You can give any program (its executable binary file) partial root  privileges to performing specific functions (eg. network  tasks, filesystem operations, etc). So, we need to delegate (to binary file of a network utility) the appropriate root privileges to perform network-related tasks and to capture raw packets from wire/wireless adapter.

#### setcap

**`setcap [-q] [-n <rootid>] [-v] {capabilities|-|-r} filename [ ... capabilitiesN fileN ]`**

set file capabilities.

| Option        | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| `-q`          | Make the program less verbose in its output.                 |
| `-n <rootid>` | Set  the  file capability for use only in a namespace with this rootid owner |
| `-v`          | Sets the capabilities of each specified filename to the capabilities specified |
| `-r`          | Remove a capability set from a file                          |
|               |                                                              |

##### Example

Grant `cap_net_admin` and `cap_net_raw` capabilities to conky monitor which makes it capable of performing admin functions & use RAW and PACKET sockets.

```shell
sudo setcap cap_net_raw,cap_net_admin=eip /usr/bin/conky

# Check the capabilities of a file
getcap /usr/bin/conky

# Remove capabilities
sudo setcap -r /usr/bin/conky
```

#### meaning of the =eip suffix

Each process has three sets of capabilities — **inheritable**, **permitted** and **effective**:

**Effective** capabilities are those which define what a process can actually do. For example, it can’t deal with raw sockets unless CAP_NET_RAW is in the effective set.
**Permitted** capabilities are those which a process is allowed to have should it ask for them with the appropriate call. These don’t allow a process to actually do anything unless it’s been specially written to ask for the capability specified. This allows processes to be written to add particularly sensitive capabilities to the effective set only for the duration when they’re actually required.
**Inheritable** capabilities are those which can be inherited into the permitted set of a spawned child process. During a fork() or clone() operation the child process is always given a duplicate of the capabilities of the parent process, since at this point it’s still running the same executable. The inheritable set is used when an exec() (or similar) is called to replace the running executable with another. At this point the permitted set of the process is masked with the inheritable set to obtain the permitted set that will be used for the new process.

## Network Administration

### `ip`



| Command   | Description |
| --------- | ----------- |
| link      |             |
| addr[ess] |             |
| route     |             |
| rule      |             |
| tunnel    |             |



```sh
# list ifaces with ip & broadcast ip assigned
ip addr show | awk '/inet.*brd/{print $NF;}'

# list ifaces with broadcast or ppp link enabled.
ip link | grep -Po --regexp "(?<=[0-9]: ).*(?=: <BROADCAST|: <POINTOPOINT)"

# list ip addresses of active interfaces.
ip addr show | grep -Po --regexp "(?<=inet ).*(?=/[0-9]+ brd)"

# active & up iface:
ip link | grep " UP " | grep -Po --regexp "(?<=[0-9]: ).*(?=: <)"
```

