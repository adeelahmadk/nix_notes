# Development Environments

[TOC]



## Terminals

### CLI Apps

#### Ranger

Text-based file manager

#### FZF

Fuzzy Finder



### Alacritty



## Languages & Scripts

### bash

#### xclip [OPTION] [FILE]...

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

##### Examples

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



#### cheat.sh

[`cheat.sh`](https://cheat.sh/) or `cht.sh` is a community driven documentation repository that can be accessed as a web API with a simple curl/browser/editor interface. It serves as a unified cheat-sheet for a plethora of technologies. According the [GitHub repo](https://github.com/chubin/cheat.sh):

> Covers 56 programming languages, several DBMSes, and more than 1000 most important UNIX/Linux commands.



Examples:

```sh
# Shell
curl cheat.sh/tar
curl cht.sh/curl
curl https://cheat.sh/rsync
curl https://cht.sh/tr
# Programming Languages
curl cht.sh/go/Pointers
curl cht.sh/scala/Functions
curl cht.sh/python/lambda
# List of available programming language cheat sheets
curl cht.sh/go/:list
curl cht.sh/go/:learn
# 
```



##### Command line client, cht.sh

The cheat.sh service has its own command line client (`cht.sh`) that has several useful features compared to querying the service directly with `curl`:

- Special shell mode with a persistent queries context and readline support.
- Queries history.
- Clipboard integration.
- Tab completion support for shells (bash, fish, zsh).
- Stealth mode.

###### Installation

To install the client:

```sh
PATH_DIR="$HOME/bin"  # or another directory on your $PATH
mkdir -p "$PATH_DIR"
curl https://cht.sh/:cht.sh > "$PATH_DIR/cht.sh"
chmod +x "$PATH_DIR/cht.sh"
```

or to install it globally (for all users):

```sh
curl https://cht.sh/:cht.sh | sudo tee /usr/local/bin/cht.sh
chmod +x /usr/local/bin/cht.sh
```

Note: The package "rlwrap" is a required dependency to run in shell mode. Install this using `sudo apt install rlwrap`

##### Client usage

Now, you can use `cht.sh` instead of `curl`, and write your queries in more natural way, with spaces instead of `+`:

```sh
$ cht.sh go reverse a list
$ cht.sh python random list elements
$ cht.sh js parse json
```

It is even more convenient to start the client in a special shell mode:

```sh
$ cht.sh --shell
cht.sh> go reverse a list
```

If all your queries are about the same language, you can change the context and spare repeating the programming language name:

```sh
$ cht.sh --shell
cht.sh> cd go
cht.sh/go> reverse a list
```

or even start the client in this context:

```sh
$ cht.sh --shell go
cht.sh/go> reverse a list
...
cht.sh/go> join a list
...
```

If you want to change the context, you can do it with the `cd` command, or if you want do a single query for some other language, just prepend it with `/`:

```sh
$ cht.sh --shell go
...
cht.sh/go> /python dictionary comprehension
...
```

If you want to copy the last answer into the clipboard, you can use the `c` (`copy`) command, or `C` (`ccopy`, without comments).

```
cht.sh/python> append file
#  python - How do you append to a file?

with open("test.txt", "a") as myfile:
myfile.write("appended text")
cht.sh/python> C
copy: 2 lines copied to the selection
```

Type `help` for other internal `cht.sh` commands.

```
cht.sh> help
help    - show this help
hush    - do not show the 'help' string at start anymore
cd LANG - change the language context
copy    - copy the last answer in the clipboard (aliases: yank, y, c)
ccopy   - copy the last answer w/o comments (cut comments; aliases: cc, Y, C)
exit    - exit the cheat shell (aliases: quit, ^D)
id [ID] - set/show an unique session id ("reset" to reset, "remove" to remove)
stealth - stealth mode (automatic queries for selected text)
update  - self update (only if the scriptfile is writeable)
version - show current cht.sh version
/:help  - service help
QUERY   - space separated query staring (examples are below)
cht.sh> python zip list
cht.sh/python> zip list
cht.sh/go> /python zip list
```

The `cht.sh` client has its configuration file which is located at `~/.cht.sh/cht.sh.conf` (location of the file can be overridden by the environment variable `CHTSH_CONF`). Use it to specify query options that you would use with each query. For example, to switch syntax highlighting off create the file with the following content:

```
CHTSH_QUERY_OPTIONS="T"
```

Or if you want to use a special syntax highlighting theme:

```
CHTSH_QUERY_OPTIONS="style=native"
```

(`curl cht.sh/:styles-demo` to see all supported styles).

Other cht.sh configuration parameters:

```
CHTSH_CURL_OPTIONS="-A curl"        # curl options used for cht.sh queries
CHTSH_URL=https://cht.sh            # URL of the cheat.sh server
```

##### Tab completion

###### Bash Tab completion

To activate tab completion support for `cht.sh`, add the `:bash_completion` script to your `~/.bashrc`:

```sh
curl https://cheat.sh/:bash_completion > ~/.bash.d/cht.sh
. ~/.bash.d/cht.sh
# and add . ~/.bash.d/cht.sh to ~/.bashrc
```

###### ZSH Tab completion

To activate tab completion support for `cht.sh`, add the `:zsh` script to the *fpath* in your `~/.zshrc`:

```sh
curl https://cheat.sh/:zsh > ~/.zsh.d/_cht
echo 'fpath=(~/.zsh.d/ $fpath)' >> ~/.zshrc
# Open a new shell to load the plugin
```



##### References

- [Cheat.sh](https://cheat.sh)
- 



#### `watch`

`watch`  runs  command  repeatedly, displaying its output and errors (the first screen‐full).  This allows you to watch the program output change over  time.   By  default, command is run every 2 seconds and watch will run until interrupted.

```
Usage:
 watch [options] command

Options:
  -b, --beep             beep if command has a non-zero exit
  -c, --color            interpret ANSI color and style sequences
  -d, --differences[=<permanent>]
                         highlight changes between updates
  -e, --errexit          exit if command has a non-zero exit
  -g, --chgexit          exit when output from command changes
  -n, --interval <secs>  seconds to wait between updates
  -p, --precise          attempt run command in precise intervals
  -t, --no-title         turn off header
  -x, --exec             pass command to exec instead of "sh -c"

 -h, --help     display this help and exit
 -v, --version  output version information and exit

EXIT STATUS
  0      Success.
  1      Various failures.
  2      Forking the process to watch failed.
  3      Replacing child process stdout with write side pipe failed.
  4      Command execution failed.
  5      Closing child process write pipe failed.
  7      IPC pipe creation failed.
  8      Getting  child  process return value with waitpid(2) failed, or command exited 
  			 up on error.
  other  The watch will propagate command exit status as child exit status.
```



### C

#### Input double values

[Ref: How to make sure input is a double in the C programming language](https://stackoverflow.com/a/2833980/5892622)

```c
/*
    Copyright (C) 2010  S. Randall Sawyer

This code is in the public domain.  It is intended to be usefully illustrative.
It is not intended to be used for any particular application. If this code is
used in any application - whether open source or proprietary, then please give
credit to the author.

*/

#include  <ctype.h>

#define FAILURE (0)
#define SUCCESS (!FAILURE)

enum {
  END_EXPRESSION        = 0x00,
  ALLOW_POINT           = 0x01,
  ALLOW_NEGATIVE        = 0x02,
  REQUIRE_DIGIT         = 0x04,
  ALLOW_EXPONENT        = 0x08,
  HAVE_EXPONENT         = 0x10,
  EXPECT_EXPRESSION     = ALLOW_POINT | ALLOW_NEGATIVE | REQUIRE_DIGIT,
  EXPECT_INTEGER        = ~ALLOW_POINT,
  EXPECT_POS_EXPRESSION = ~ALLOW_NEGATIVE,
  EXPECT_POS_INTEGER    = EXPECT_INTEGER & EXPECT_POS_EXPRESSION,
  EXPECT_EXPONENT       = EXPECT_INTEGER ^ HAVE_EXPONENT,
  EXPONENT_FLAGS        = REQUIRE_DIGIT | ALLOW_EXPONENT | HAVE_EXPONENT
} DoubleParseFlag;

int double_parse_long ( const char * str, const char ** err_ptr )
{
  int ret_val, flags;
  const char * ptr;

  flags = EXPECT_EXPRESSION;
  ptr   = str;

  do  {

    if ( isdigit ( *ptr ) ) {
      ret_val = SUCCESS;
      /*  The '+' here is the key:  It toggles 'ALLOW_EXPONENT' and
          'HAVE_EXPONENT' successively  */
      flags = ( flags + ( flags & REQUIRE_DIGIT ) ) & EXPECT_POS_EXPRESSION;
    }
    else if ( (*ptr & 0x5f) == 'E' )  {
      ret_val = ( ( flags & ( EXPONENT_FLAGS ) ) == ALLOW_EXPONENT );
      flags = EXPECT_EXPONENT;
    }
    else if ( *ptr == '.' ) {
      ret_val = ( flags & ALLOW_POINT );
      flags = ( flags & EXPECT_POS_INTEGER );
    }
    else if ( *ptr == '-' ) {
      ret_val = ( flags & ALLOW_NEGATIVE );
      flags = ( flags & EXPECT_POS_EXPRESSION );
    }
    else if ( iscntrl ( *ptr ) )  {
      ret_val = !( flags & REQUIRE_DIGIT );
      flags = END_EXPRESSION;
    }
    else  {
      ret_val = FAILURE;
      flags = END_EXPRESSION;
    }

    ptr++;

  } while (ret_val && flags);

  if (err_ptr != NULL)
    *err_ptr = ptr - 1;

  return ret_val;
}
```

Slim version of above code:

```c
/*
    Copyright (C) 2010  S. Randall Sawyer

This code is in the public domain.  It is intended to be usefully illustrative.
It is not intended to be used for any particular application. If this code is
used in any application - whether open source or proprietary, then please give
credit to the author.

*/

#include  <ctype.h>

int double_parse_short ( const char * str, const char ** err_ptr )
{
  int ret_val, flags;
  const char * ptr;

  flags = 0x07;
  ptr   = str;

  do  {

    ret_val = ( iscntrl ( *ptr ) ) ? !(flags & 0x04) : (
              ( *ptr == '.' ) ? flags & 0x01 : (
              ( *ptr == '-' ) ? flags & 0x02 : (
              ( (*ptr & 0x5f) == 'E' ) ? ((flags & 0x1c) == 0x08) : (
              ( isdigit ( *ptr ) ) ? 1 : 0 ) ) ) );

    flags =   ( isdigit ( *ptr ) ) ?
                (flags + (flags & 0x04)) & 0x1d : (
              ( (*ptr & 0x5f) == 'E' ) ? 0x0e : (
              ( *ptr == '-' ) ? flags & 0x1d : (
              ( *ptr == '.' ) ? flags & 0x1c : 0 ) ) );

    ptr++;

  } while (ret_val && flags);

  if (err_ptr != NULL)
    *err_ptr = ptr - 1;

  return ret_val;
}
```



### Java

#### Install

1. Download [Oracle JDK](https://www.oracle.com/java/technologies/javase-downloads.html) (tar.gz) version of your choice for your platform.

2. Make a directory and extract the tar ball there.

    ```shell
    sudo mkdir /usr/lib/jvm
    cd /usr/lib/jvm
    sudo tar -xvzf ~/Downloads/jdk-14.0.2_linux-x64_bin.tar.gz
    ```

3. Configure the Java installation by adding the installation path to `/etc/profile` (global) or `~/.profile` (local).

    ```bash
    JAVA_HOME="/usr/lib/jvm/jdk-14.0.2"
    ```

4. Inform the System About the Location of the Java Installation:

    ```shell
    sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk-14.0.2/bin/java" 0
    sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk-14.0.2/bin/javac" 0
    sudo update-alternatives --set java /usr/lib/jvm/jdk-14.0.2/bin/java
    sudo update-alternatives --set javac /usr/lib/jvm/jdk-14.0.2/bin/javac
    ```

5. Verify if Everything is Working Properly:

    ```shell
    update-alternatives --list java
    update-alternatives --list javac
    java -version
    ```

    


### Python

#### Package management

To generate a list of all outdated packages:

```sh
pip list --outdated
```

##### pip ModuleNotFoundError

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



##### Update all Python Packages

###### Windows

Open a command shell by typing ‘powershell’ in the Search Box of the Task bar and enter: 

```powershell
pip freeze | %{$_.split('==')[0]} | %{pip install --upgrade $_}
```

This will upgrade all packages system-wide to the latest version available in the Python Package Index (PyPI).

###### Linux

Linux provides a number of ways to use pip in order to upgrade Python packages, including grep and awk.

To upgrade all packages using pip with grep on Ubuntu Linux:

```sh
pip3 list --outdated --format=freeze | grep -v '^\-e' | cut -d = -f 1 | xargs -n1 pip3 install -U 
```

To upgrade all packages using pip with awk on Ubuntu Linux:

```sh
pip3 list -o | cut -f1 -d' ' | tr " " "\n" | awk '{if(NR>=3)print)' | cut -d' ' -f1 | xargs -n1 pip3 install -U 
```

###### System Agnostic

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



###### Virtual Environment

The easiest way to update unpinned  packages (i.e., packages that do not require a specific version) in a  virtual environment is to run the following Python script that makes use of pip:

```sh
import pkg_resources
from subprocess import call

for dist in pkg_resources.working_set:
    call("python -m pip install --upgrade " + dist.<projectname>, shell=True)
```

###### Pipenv

The simplest way to update all the  unpinned packages in a specific virtual environment created with pipenv  is to do the following steps:

1.  Activate the Pipenv shell that contains the packages to be upgraded:

```sh
pipenv shell
```

1.  Upgrade all packages:

```sh
pipenv update
```

#### Command Line



#### Virtual Environements

##### Virtualenv

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

##### Pipenv

##### Conda

##### Pyenv



#### Packaging

##### Setuptools

##### Poet

## Verification 

Includes Debug, Profile and Test.

### Debugging

#### strace

`strace` runs the specified command until it exits.  It intercepts and records the system calls which are called by a process and the signals which  are  received by a process.  The name of each system call, its arguments and its return value are printed on standard error or to the file specified with the `-o` option.

```sh
strace -o cp_strace cp .bashrc bashrc
```

#### Examples



### Profiling



## Back-end

### Apache Web Server

#### Access Control

#### URL Rewriting

### Nginx

#### Access Control

Deny access to a directory under your web root:

```nginx
# Deny access to wp-content folders
location ~* ^/(wp-content)/(.*?)\.(zip|gz|tar|csv|bzip2|7z)\$ { deny all; }
location ~ ^/wp-content/webtoffee_import { deny all; }
```

#### URL Rewriting



### MySQL

Usually `PHP` installation has an upload file size that may be too small when importing a database backup. We have a few options to overcome this limitation.

#### Option 1 - Compress SQL file

Compress you `.sql` file using `zip`, `gzip` or `bzip2` method. SQL files basically are plain text files so they compress quite well. If you are still exceeding the limit use one of the other options.

#### Option 2 - Increase PHP file size limit

Open your `php.ini` file, which may be located at either of the locations:

- /etc/php.ini
- /etc/php/php.ini
- /etc/php5/php.ini
- /etc/php5/apache2/php.ini
- /usr/bin/php5/bin/php.ini

In case of `php-fpm` package this file has to be placed in `/usr/local/etc/php`. You can place this file in a volume with a `docker-compose.yml` file service. For example, `php.ini` should be place in the `./php` directory like in the following snippet:

```yaml
	volumes:
		- ./php:/usr/local/etc/php
```

Find lines starting with `post_max_size` and `upload_max_filesize` and increase values to required size in MB. e.g.

```ini
post_max_size = 10M
upload_max_filesize = 10M
```

Save the file and restart apache (or nginx):

```shell
/etc/init.d/apache2 restart
```

After import is completed you may want to restore original values.

#### Option 3 - use `mysql` shell in terminal

You can bypass `phpMyAdmin` altogether and do the import via terminal. Upload your `.sql` file to the web server or mount as shared volume with MySQL docker container and run flowing command in interactive terminal:

```mysql
mysql -u <username> -p <database> < /path/file.sql
```

where:

- `<username>` : your MySQL user name. e.g. root
- `<database>` : database you are importing to
- `/path/file.sql` : full path to your `.sql` file

It will prompt you for your MySQL password and complete the import.

For example: 

```shell
mysql -u root -p db_name < /backup/db_backup.sql
```

You should create that database `db_name` before issuing this command.

### Containers

#### Execute a command in a Detached Containers

Execute a CLI program in a detached container (interactively/detached) by issuing an exec command in the terminal:

```shell
# for container built from debian, ubuntu, fedora etc.
docker container exec -it dev_mysql_1 bash
# for container built from alpine
docker container exec -it dev_php_1 sh
```

You can also issue a command to run particular CLI tool directly:

```shell
docker container exec dev_mysql_1 mysql -u root -p db_name < /backup/db_backup.sql
```

Notice the difference of `-it` option which runs a command interactively.

### Virtualization

#### Install



```sh
sudo apt install qemu qemu-kvm libvirt-clients libvirt-daemon-system virtinst bridge-utils
```

#### Dependencies

##### System tools

- [kvm](https://www.linux-kvm.org/page/Main_Page)

    - Kernel-based Virtual Machine
    - Kernel module that handles CPU and memory communication

- [qemu](https://www.qemu.org)

    - Quick EMUlator
    - Emulates many hardware resources such as disk, network, and USB. While it can emulate CPU, you'll be exposed to qemu/kvm, which delegates concerns like that to the KVM (which is [HVM](https://en.wikipedia.org/wiki/Hardware-assisted_virtualization)).
    - Memory relationship between qemu/kvm is a little more complicated but can be [read about here](https://www.linux-kvm.org/page/Memory).

- [libvirt](https://libvirt.org)

    - Exposes a consistent API atop many vPermissions

        There are a few key pieces to configure so your using can interact with `qemu:///system`. This enables VMs to run as root, which is *generally* what you'll want. This is also the default used by `virt-manager`. [[ref](https://blog.wikichoon.com/2016/01/qemusystem-vs-qemusession.html)]

        `virsh`, will use `qemu:///session` by default, which means CLI calls not run as `sudo` will be looking at a different user. To ensure **all** client utilities default to `qemu:///system`, add the following configuration to your `.config` directory.

        ```sh
        sudo cp -rv /etc/libvirt/libvirt.conf ~/.config/libvirt/ &&\
        # replace your user and group
        sudo chown ${YOURUSER}:${YOURGROUP} ~/.config/libvirt/libvirt.conf
        ```

        Now uncomment the the `qemu:///system` line in the copied file.irtualization technologies. APIs are consumed by client tools for provisioning and managing VMs.

##### Client tools

- [virsh](https://libvirt.org/manpages/virsh.html)
    - Command-line tools for communicating with libvirt
- [virt-manager](https://virt-manager.org)
    - GUI to manage KVM, qemu/kvm, xen, and lxc.
    - Contains a [VNC](https://en.wikipedia.org/wiki/Virtual_Network_Computing) and [SPICE](https://en.wikipedia.org/wiki/Simple_Protocol_for_Independent_Computing_Environments) client for direct graphical access to VMs.
    - GUI alternative to `virsh`, albeit less capable.
- [virt-install](https://linux.die.net/man/1/virt-install)
    - Helper tools for creating new VM guests.
    - Part of the `virt-manager` project.
- [virt-viewer](https://linux.die.net/man/1/virt-viewer)
    - UI for interacting with VMs via VNC/SPICE.
    - Part of the `virt-manager` project.

##### Ancillary system tools:

These tools are used to support the system tools listed above.

- `dnsmasq`: light-weight DNS/DHCP server. Primarily used for allocating IPs to VMs.
- `dhclient`: used for DHCP resolution; probably on your distro already
- `dmidecode`: prints computers SMBIOS table in readable format. Optional dependency, depending on your package manager.
- `ebtables`: used for setting up NAT networking the host
- `bridge-utils`: used to create bridge interfaces easily. (tool has been [deprecated since 2016](https://lwn.net/Articles/703776), but still used)
- `openbsd-netcat`: enables remote management over SSH

#### Permissions

There are a few key pieces to configure so your using can interact with `qemu:///system`. This enables VMs to run as root, which is *generally* what you'll want. This is also the default used by `virt-manager`. [[ref](https://blog.wikichoon.com/2016/01/qemusystem-vs-qemusession.html)]

`virsh`, will use `qemu:///session` by default, which means CLI calls not run as `sudo` will be looking at a different user. To ensure **all** client utilities default to `qemu:///system`, add the following configuration to your `.config` directory.

```sh
sudo cp -rv /etc/libvirt/libvirt.conf ~/.config/libvirt/ &&\
# replace your user and group
sudo chown ${YOURUSER}:${YOURGROUP} ~/.config/libvirt/libvirt.conf
```

Now uncomment the the `qemu:///system` line in the copied file.

#### Lifecycle

##### Install Guest

You can create a VM from CLI or `virt-manager`.

```sh
virt-install \
  --name ubuntu2004 \
  --ram 2048 \
  --disk path=/var/lib/libvirt/images/u20.qcow2,size=8 \
  --vcpus 1 \
  --os-type linux \
  --os-variant generic \
  --console pty,target_type=serial \
  --cdrom /var/lib/libvirt/isos/ubuntu-20.04.2-live-server-amd64.iso
```

As it can be seen in the previous command, the default image store is in `/var/lib/libvirt/images`.

##### Clone Guest

A user can create a “base image” and use `virt-clone` to stamp out many instances of it. You can run a clone as follows.

```sh
virt-clone \
  --original ubuntu20.04 \
  --name cloned-ubuntu \
  --file /var/lib/libvirt/images/cu.qcow2
```

##### `virsh` Headless Management

`virsh` management user interface can be used to interact with virtual guest domains. It currently supports Xen,  QEMU,  KVM, LXC, OpenVZ, VirtualBox and VMware ESX.

###### Start VMs

To start a VM, run:

```sh
virsh start Ubuntu-20.04
```

You can also use the VM's ID to start it:

```sh
virsh start 2
```

###### Restart VMs

To restart a running VM, do:

```sh
virsh reboot Ubuntu-20.04
```

Or,

```sh
virsh reboot 2
```

###### Pause VMs

To pause a running VM, do:

```sh
virsh suspend Ubuntu-20.04
```

Or,

```sh
virsh suspend 2
```

###### Resume VMs

To resume a suspended VM, do:

```sh
virsh resume Ubuntu-20.04
```

Or,

```sh
virsh resume 2
```

###### Shutdown VMs

To power off a running VM, do:

```sh
virsh shutdown Ubuntu-20.04
```

Or,

```sh
virsh shutdown 2
```



#### Virtual Networking



#### Cluster Orchestration



#### References

1. [OctetzYouTube Channel](https://www.youtube.com/channel/UCkFMkIX1Tb2s8CIgfzmUfLA)
2. [Octetz blog](https://octetz.com/docs)
3. [Octetz: Linux Hypervisor Setup (libvirt/qemu/kvm)](https://octetz.com/docs/2020/2020-05-06-linux-hypervisor-setup/)
4. [YouTube: Dicussion and tutorial for Linux Hypervisor Setup](https://www.youtube.com/watch?v=HfNKpT2jo7U)
5. [OStechnix: Install & configure headless server in KVM](https://ostechnix.com/install-and-configure-kvm-in-ubuntu-20-04-headless-server/)
6. [VM Networking (libvirt / bridge)](https://octetz.com/docs/2020/2020-11-13-vm-networks/)
7. [YouTube: VM Networking](https://www.youtube.com/watch?v=6435eNKpyYw)
8. [Preparing VM images for qemu/KVM](https://octetz.com/docs/2020/2020-10-19-machine-images/)
9. [Shaping Linux traffic with `tc`](https://octetz.com/docs/2020/2020-09-16-tc/)



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

### Plug-ins

#### Emmet

Plug-in to generate HTML and CSS from a shorthand sysntax.



## InfoSec

### Cryptographic Hash Functions



#### References

1. [Guess function by looking at hash](https://unix.stackexchange.com/a/430143)
2. 
