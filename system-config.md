# System Configuration Cheat-sheet

## Contents

[TOC]



## Kernel Parameters

### Setup Swap Space
#### Setup swap space in a file
Following commands require super-user permissions:
```
swapoff -a
fallocate -l 8G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
echo '/swapfile none swap sw 0 0' | tee -a /etc/fstab
```
#### Set Swappiness
Swappiness is a measure of swap affinity for kernel. It's value is set between `0` and `100`, where `0` stands for `disable` and `100` for `aggressive` swap affinity. For a large RAM sizes (say >= 8G) favorable value would be `10` and for small RAM sizes value should approach `60`. In order to change swappiness of the system use one of the following methods interminal:

1. Add following to `/etc/sysctl.conf`
```
vm.swappiness = 10
```
2. Setting for the first time to the file, you can use this one-liner:
```
echo 'vm.swappiness = 10' | sudo tee -a /etc/sysctl.conf
```
3. Change swappiness (10 to 20) as:
```
sudo sed -i 's/swappiness = 10/swappiness = 20/g' /etc/sysctl.conf
```
Finally, run following command to apply changes at the system level:
```
sudo sysctl -p
```
### Configure ACPI events (like Lid Close Action)
#### Open logind.conf to edit:
```sh
sudo gedit /etc/systemd/logind.conf
```
Set **`HandleLidSwitch`** to one of `poweroff`|`hibernate`|`suspend`|`ignore` for a lid close event:
```ini
HandleLidSwitch=suspend
```
Other options are:
```ini
HandleSuspendKey=suspend
HandleLidSwitch=suspend
HandleLidSwitchDocked=ignore
```
Restart logind service to apply changes:
```sh
sudo systemctl restart systemd-logind.service
```
For some laptops, the hibernate function might not work. Run test command:
```sh
sudo apt install pm-utils && sudo pm-hibernate
```

### Suspend/Sleep params

In certain cases, sleep fails to suspend the computer. One such example faced (by *yours truly*) is failure to freeze a task:

```
Freezing of tasks failed after 20.009 seconds (1 tasks refusing to freeze, wq_busy=0):
```

 A possible work around is to change kernel sleep parameter:

```sh
sudo kernelstub -a "mem_sleep_default=deep"
```



## Firmware update

The latest models of Lenovo ThinkPads can update the firmware using the Linux Vendor Firmware Service (LVFS). When updates are published on LVFS, GNOME can notify you about upgrades. 

### Install from CLI

List all devices that support firmware updates as:

```shell
sudo fwupdmgr get-devices
```

One can check bios version on Linux using the `dmidecode` command:

```shell
sudo dmidecode -s bios-version
```

Try the following bash for loop:

```sh
for d in system-manufacturer system-product-name bios-release-date bios-version
do
	echo "${d^} : " $(sudo dmidecode -s $d)
done
```

Download and refresh metadata from LVFS server:

```shell
sudo fwupdmgr refresh

fwupdmgr get-updates
```

Install BIOS and other firmware updates:

```shell
sudo fwupdmgr update
```



### Update Lenovo BIOS (from Linux without using Windows)

Procedure to update Lenovo BIOS from Linux is as follows:

1. Download Lenovo BIOS Bootable CD for your Laptop model

2. Use the `geteltorito` command to extract bootable image 

    ```shell
    sudo apt install genisoimage
    
    # usage: geteltorito -o {output-image-name.img {Bootable-CD.iso}
    geteltorito -o x230.img g2uj28us.iso
    ```

3. Run the `dd` command to write extracted image to USB stick or pen 

    ```shell
    sudo dd if=x230.img of=/dev/sdb bs=64K status=progress
    ```

4. Reboot the laptop

5. Interrupt boot process by pressing the `ENTER` key

6. Press `F12` key and select USB mass storage device as boot source

7. The BIOS update utility should run now



## ACPI Configuration

### Catch ACPI events

For script (`lid.sh`) to be called in `/etc/acpi/`, you have to create the correct file in `/etc/acpi/events/`. 

[1]: https://www.thinkwiki.org/wiki/How_to_configure_acpid	"ThinkWiki: How to configure acpid"

Create a file called `/etc/acpi/events/laptop-lid-close` with the following content:

```
event=button/lid LID close
action=/etc/acpi/laptop-lid-close.sh
```

And create a file `/etc/acpi/laptop-lid-close.sh` with the following content and gave it execute rights (`chmod +x /etc/acpi/laptop-lid-close.sh`):

```sh
#!/bin/sh

systemctl suspend
```

### Reset a serial IO Device

Check the bus/device description:

```sh
cat  /sys/bus/serio/devices/serio1/description
```

Reset the connection on the bus:

```sh
# disconect the device
echo -n "none" | sudo tee /sys/bus/serio/devices/serio1/drvctl 
# reconnect the device
echo -n "reconnect" | sudo tee /sys/bus/serio/devices/serio1/drvctl 
```



## Hardware Drivers

### Dummy Output Problem

#### Fixing the no sound issue in Ubuntu (Dummy Output)

If you get only Dummy Output in Ubuntu (and variants), first check your audio device & kernel driver module:

```sh
lspci -nnk | grep -A2 Audio
...
	Kernel driver in use: snd_hda_intel
	Kernel modules: snd_hda_intel
```

If you do get `snd_hda_intel` in the output in the above command, and you get no sound (and only a  Dummy Output) in Ubuntu, you need to  add `options snd-hda-intel model=generic` at the end of the `/etc/modprobe.d/alsa-base.conf` file.

```sh
echo "options snd-hda-intel model=generic" | sudo tee -a /etc/modprobe.d/alsa-base.conf
```

If you continue to get no sound output and still only see the Dummy Output in System Settings, you can try to set the `model` to `auto` instead of `generic`, so edit the `/etc/modprobe.d/alsa-base.conf` file.

#### Fix PCI/internal sound card not detected (dummy output) with Ubuntu kernel 5.3.0-41 and -42 in Ubuntu 19.10 / 18.04

There's a regression (thanks JustNiz for notifying me) in the 5.3.0-41  and -42 kernel that causes a new "dummy output" issue on Ubuntu 19.10  and 18.04. The explanations for this bug are available in [this bug report](https://bugs.launchpad.net/ubuntu/+source/linux-oem-osp1/+bug/1864061)

The solution for this "dummy output" regression is to:

1. Edit `/etc/modprobe.d/alsa-base.conf` as root and add `options snd-hda-intel dmic_detect=0` at the end of this file. You can do this with a single command, by using (run this command only once):

    ```sh
    echo "options snd-hda-intel dmic_detect=0" | sudo tee -a /etc/modprobe.d/alsa-base.conf
    ```

2. Edit `/etc/modprobe.d/blacklist.conf` as root and add `blacklist snd_soc_skl` at the end of the file. You can do this with a single command, by using (run this command only once):

    ```sh
    echo "blacklist snd_soc_skl" | sudo tee -a /etc/modprobe.d/blacklist.conf
    ```

3. After making these changes, reboot your system.

## System Configuration



### `pkg-config`

`pkg-config` return meta-information about installed libraries. It's used to retrieve information about installed  libraries  in the  system.  It is typically used to compile and link against one or more libraries.

`pkg-config` defines and supports a unified interface for querying installed libraries for the purpose of compiling software that depends on them. It allows programmers and installation scripts to work without explicit knowledge of detailed library path information. 

`pkg-config` retrieves information about packages from special  metadata  files.  These files  are  named  after the package, and has a `.pc` extension.  On most systems, `pkg-config` looks in `/usr/lib/pkgconfig`, `/usr/share/pkgconfig`, `/usr/local/lib/pkgconfig` and  `/usr/local/share/pkgconfig`  for  these  files.  It will additionally look in the colon-separated (on Windows, semicolon-separated) list of  directories  specified  by the `PKG_CONFIG_PATH` environment variable.

Here is an example `.pc` file for [libpng](https://en.wikipedia.org/wiki/Libpng):

```
prefix=/usr/local
exec_prefix=${prefix}
libdir=${exec_prefix}/lib
includedir=${exec_prefix}/include
 
Name: libpng
Description: Loads and saves PNG files
Version: 1.2.8
Libs: -L${libdir} -lpng12 -lz
Cflags: -I${includedir}/libpng12
```

For example, when compiling a program with `libpng`:

```sh
gcc -o test test.c $(pkg-config --libs --cflags libpng)
```

[Ref#2,3,4](#References)

### `update-alternatives`

#### --config `<name>`

Show alternatives for the <name> group and ask the user to select which one to use:

```sh
update-alternatives --config x-session-manager
```
#### --install `<link> <name> <path> <priority>`

To add a group of alternatives to the system:

```sh
# add an alternative jdk installation
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk-14.0.2/bin/javac 1
# add an alternative session manager
update-alternatives --install /usr/bin/x-session-manager x-session-manager /usr/bin/gnome-session-classic 60
```
#### --remove `<name> <path>`
```sh
update-alternatives --remove x-session-manager /usr/bin/startkde
```

### gsettings

#### get/set:

Get list of Gnome favorite applications:

```sh
gsettings get org.gnome.shell favorite-apps
```

Set list of Gnome favorite applications (only possible way to order apps in _dash-to-dock_ right now):

```sh
gsettings set org.gnome.shell favorite-apps "['org.gnome.Nautilus.desktop', 'org.gnome.Terminal.desktop', 'firefox.desktop', 'vivaldi-stable.desktop', 'io.elementary.appcenter.desktop']"
```

### date time
#### get/set Timezone

```sh
timedatectl list-timezones | grep -i europe
timedatectl set-timezone Europe/Paris
```

### Systemd services

#### units

```
System Unit Search Path
    /etc/systemd/system.control/*
    /run/systemd/system.control/*
    /run/systemd/transient/*
    /run/systemd/generator.early/*
    /etc/systemd/system/*
    /etc/systemd/systemd.attached/*
    /run/systemd/system/*
    /run/systemd/systemd.attached/*
    /run/systemd/generator/*
    ...
    /lib/systemd/system/*
    /run/systemd/generator.late/*

User Unit Search Path
    ~/.config/systemd/user.control/*
    $XDG_RUNTIME_DIR/systemd/user.control/*
    $XDG_RUNTIME_DIR/systemd/transient/*
    $XDG_RUNTIME_DIR/systemd/generator.early/*
    ~/.config/systemd/user/*
    /etc/systemd/user/*
    $XDG_RUNTIME_DIR/systemd/user/*
    /run/systemd/user/*
    $XDG_RUNTIME_DIR/systemd/generator/*
    ~/.local/share/systemd/user/*
    ...
    /usr/lib/systemd/user/*
    $XDG_RUNTIME_DIR/systemd/generator.late/*
```



### Network Administration

#### Network-manager

```
Dec  8 01:06:21 pop-os systemd-resolved[794]: Failed to send hostname reply: Invalid argument
```

#### DHCP lease renewal

Using `dhclient`

```sh
sudo dhclient -r eth0
sudo dhclient eth0
```

Using services

```sh
sudo ifdown eth0
sudo ifup eth0
### RHEL/CentOS/Fedora specific command ###
sudo /etc/init.d/network restart
### Debian / Ubuntu Linux specific command ###
sudo /etc/init.d/networking restart
### Systemd specific command ###
sudo systemctl restart networking.service
```

Using `nmcli`

```sh
nmcli con
nmcli con down id 'home_wifi'
nmcli con up id 'home_wifi'
```



#### SAMBA Share



Samba variables

| Variable                | Definition                                                   |
| ----------------------- | ------------------------------------------------------------ |
| **Client variables**    |                                                              |
| `%a`                    | Client’s architecture (see [Table 4-1](https://www.oreilly.com/library/view/using-samba-3rd/0596007698/ch04.html#samba3-CHP-4-TABLE-1)) |
| `%i`                    | IP address of the interface on the server to which the client connected |
| `%I`                    | Client’s IP address (e.g., 172.16.1.2)                       |
| `%m`                    | Client’s NetBIOS name                                        |
| `%M`                    | Client’s DNS name (defaults to the value of `%I` if `hostname lookups = no`) |
| User variables          |                                                              |
| `%u`                    | Current Unix username (requires a connection to a share)     |
| `%U`                    | Username transmitted by the client in the initial authentication request |
| `%D`                    | User’s domain (e.g., the string `DOM-A` in DOM-A\user)       |
| `%H`                    | Home directory of `%u`                                       |
| `%g`                    | Primary group of `%u`                                        |
| `%G`                    | Primary group of `%U`                                        |
| Share variables         |                                                              |
| `%S`                    | Current share’s name                                         |
| `%P`                    | Current share’s root directory                               |
| `%p`                    | Automounter’s path to the share’s root directory, if different from `%P` |
| Server variables        |                                                              |
| `%d`                    | Current server process ID                                    |
| `%h`                    | Samba server’s DNS hostname                                  |
| `%L`                    | Samba server’s NetBIOS name sent by the client in the NetBIOS session request |
| `%N`                    | Home directory server, from the automount map                |
| `%v`                    | Samba version                                                |
| Miscellaneous variables |                                                              |
| `%R`                    | The SMB protocol level that was negotiated                   |
| `%T`                    | The current date and time                                    |
| `%$(`**`var`**`)`       | The value of environment variable *`var`*                    |

Create a system user to manage Samba service:

```sh
sudo groupadd --system smbgroup
sudo useradd --system --no-create-home --group smbgroup -s /bin/false smbuser
sudo chown -R smbuser:smbgroup /media/NAS/share
sudo chmod -R g+w /media/NAS/share
```

A sample standalone network share configuration:

```toml
[global]
        log file = /var/log/samba/%m
        log level = 1
        server role = standalone server

[demo]
        # This share requires authentication to access
        path = /srv/samba/demo/
        read only = no
        # Accessible by only smbgroup members 
        valid users = @smbgroup
        inherit permissions = yes

```

Now check an `smb.conf` configuration file for internal correctness:

```sh
testparm
```

After changing the configuration file, a service restart is in order:

```sh
sudo systemctl restart smbd
```



## References

1. [thinkwiki: How to configure acpid](https://www.thinkwiki.org/wiki/How_to_configure_acpid)
2. `pkg-config` manual page
3. [pkg-config](https://en.wikipedia.org/wiki/Pkg-config) on Wikipedia
4. [Tutorialspoint article](https://www.tutorialspoint.com/unix_commands/pkg-config.htm)