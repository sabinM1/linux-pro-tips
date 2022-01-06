# linux-pro-tips
### When you had enough of troubleshooting

I. Specific DE/distro/hardware etc. problems:
  1. [GNOME](https://github.com/sabinM1/linux-pro-tips/blob/master/GNOME.md)
  2. [Arduino](https://github.com/sabinM1/linux-pro-tips/blob/master/Arduino.md)

II. General:
  1. [Wake-on-LAN with NetworkManager](#wake-on-lan-with-networkmanager)
  2. [Tar cheat sheet & basic functionality](#tar-cheat-sheet--basic-functionality)
  3. [Automount a drive](#automount-a-drive)
  4. [Bash - Output song names from multiple folders to a file](#bash---output-song-names-from-multiple-folders-to-a-file)
  5. [Iptables](#iptables)

III. Notes:
  1. **Bold** sections should be considered important.
  2. This project was made to remind myself to never fix a working machine.
  3. Pull Requests are welcome.
  4. The commands/information found in this project are provided without any kind of warranty.
  5. In case of redistribution, be sure to keep and respect the [GPL-3.0 License](https://github.com/sabinM1/linux-pro-tips/blob/master/LICENSE).

---

## Wake-on-LAN with NetworkManager
##### From [wiki.archlinux.org](https://wiki.archlinux.org/title/Wake-on-LAN#NetworkManager)

NetworkManager provides [Wake-on-LAN ethernet support](https://www.phoronix.com/scan.php?page=news_item&px=NetworkManager-WoL-Control). One way to enable Wake-on-LAN by magic packet is through `nmcli`.

First, search for the name of the wired connection:

`nmcli con show`
```
NAME    UUID                                  TYPE            DEVICE
wired1  612e300a-c047-4adb-91e2-12ea7bfe214e  802-3-ethernet  enp0s25
```

By following, one can view current status of Wake-on-LAN settings:

`nmcli c show "wired1" | grep 802-3-ethernet.wake-on-lan`
```
802-3-ethernet.wake-on-lan:             default
802-3-ethernet.wake-on-lan-password:    --
```

**Enable Wake-on-LAN by magic packet on that connection:**

 `nmcli c modify "wired1" 802-3-ethernet.wake-on-lan magic`

**Then reboot, possibly two times. To disable Wake-on-Lan, substitute `magic` with `ignore`.**

The Wake-on-LAN settings can also be changed from the GUI using [nm-connection-editor](https://archlinux.org/packages/?name=nm-connection-editor).

You can disable Wake-on-Lan for all connections permanently by adding a dedicated configuration file:

`/etc/NetworkManager/conf.d/wake-on-lan.conf`
```
[connection]
ethernet.wake-on-lan = ignore
wifi.wake-on-wlan = ignore
```

### Enable WoL in TLP

**When using [TLP](https://wiki.archlinux.org/title/TLP) for suspend/hibernate, the `WOL_DISABLE` setting should be set to `N` in `/etc/tlp.conf` to allow resuming the computer with WoL.**

---

## Tar cheat sheet & basic functionality
##### From [haskaalo/tarcheatsheet](https://gist.github.com/haskaalo/68c0ea38e0abc3d4081db6e9446e8253) and [cheat.sh/tar](http://cheat.sh/tar)

### Compress a file or directory

`tar -czvf name-of-archive.tar.gz /path/to/directory-or-file`

* -c: Create an archive.
* -z: Compress the archive with gzip.
* -v: makes tar talk a lot. Verbose output shows you all the files being archived and much.
* -f: Allows you to specify the filename of the archive.

### Extract an Archive

`tar -xvzf name-of-archive.tar.gz`

* f: this must be the last flag of the command, and the tar file must be immediately after. It tells tar the name and path of the compressed file.
* z: tells tar to decompress the archive using gzip
* x: tar can collect files or extract them. x does the latter.
* v: makes tar talk a lot. Verbose output shows you all the files being extracted.

### Create a compressed archive, using archive suffix to determine the compression program

`tar caf target.tar.xz file1 file2 file3`

### List the contents of a tar file
`tar tvf source.tar`

### Create a .gz archive and exclude all jpg,gif,... from the tgz
`tar czvf /path/to/foo.tgz --exclude=\*.{jpg,gif,png,wmv,flv,tar.gz,zip} /path/to/foo/`

### Extract a specific file without preserving the folder structure
`tar xf source.tar source.tar/path/to/extract --strip-components=depth_to_strip`

---

## Automount a drive
##### From [techrepublic.com](https://www.techrepublic.com/article/how-to-properly-automount-a-drive-in-ubuntu-linux/)

### Locate the partition to mount

The first thing to be done is to locate the partition you want to mount. In this case, we'll be working with an entire drive. To do so type:

`sudo fdisk -l`

The output will be similar to the following:

```bash
Disk /dev/nvme0n1: 465,76 GiB, 500107862016 bytes, 976773168 sectors
Disk model: KINGSTON SA2000M8500G                   
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 35121E42-842O-F912-8451-0BO08894A1D8

Device           Start       End   Sectors   Size Type
/dev/nvme0n1p1    4096   1052671   1048576   512M EFI System
/dev/nvme0n1p2 1052672 976768064 975715393 465,3G Linux filesystem


Disk /dev/sda: 1,82 TiB, 2000398934016 bytes, 3907029168 sectors
Disk model: WDC WD20EZRZ-00Z
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: 31616826-A6D4-4688-8847-D800B1356D7A 

Device     Start        End    Sectors  Size Type
/dev/sda1   2048 3907028991 3907026944  1,8T Linux filesystem
```

In this case the disk I want to automount is *WDC WD20EZRZ-00Z*, located at `/dev/sda`.

### Locate the UUID

Next we need to find the UUID (Universal Unique Identifier) of the drive. To do that, issue the command:

`sudo blkid`

```bash
/dev/nvme0n1p1: LABEL_FATBOOT="NO_LABEL" LABEL="NO_LABEL" UUID="0FBE-6148" BLOCK_SIZE="512" TYPE="vfat" PARTUUID="e1aba8f4-65d5-4b45-8206-4d5a92e3c105"
/dev/nvme0n1p2: UUID="3BO08d83-0511-b00b-8bbb-fef71ed2xddd0" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="614e5ef3-8dec-144a-8543-742d5c824aad"
/dev/sda1: LABEL="2big" UUID="bb7LM404-a173-4358-996a-2ae010be123c" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="2big" PARTUUID="2f52b4a6-66b4-4582-98b0-b1ff8f0b44f8"
```

In this case the UUID will be `bb7LM404-a173-4358-996a-2ae010be123c`.

### Permissions and mount point

Before we add the entry to fstab, we must first create a mount point for the drive. The mount point is the directory where users will access the data on the drive (as they can't access /dev/sda itself). So let's create a directory called data:

`sudo mkdir /data`

You'll want to also change the group ownership of that directory, so that users can access it. For this, you might create a group called data and then add users to the new group. After you've done that, you could then change the ownership of the mountpoint. All this could be done with the commands:

`sudo groupadd data`<br>
`sudo usermod -aG data $USER`<br>
`sudo chown -R :data /data`

### Editing fstab

Start by opening `/etc/fstab` in your favorite editor (e.g. `sudo nano /etc/fstab` or `sudo vi /etc/fstab` etc.)

At the bottom of that file, we'll add an entry that contains the information we've discovered. The entry will look like this:

```bash
UUID=bb7LM404-a173-4358-996a-2ae010be123c /data    auto nosuid,nodev,nofail,x-gvfs-show 0 0
```

Breaking that line down, we have:

- `UUID=bb7LM404-a173-4358-996a-2ae010be123c` - is the UUID of the drive. You don't have to use the UUID here. You could just use /dev/sdj, but it's always safer to use the UUID as that will never change (whereas the device name could).
- `/data` - is the mount point for the device.
- `auto` - automatically determine the file system
- `nosuid` - specifies that the filesystem cannot contain set userid files. This prevents root escalation and other security issues.
- `nodev` - specifies that the filesystem cannot contain special devices (to prevent access to random device hardware).
- `nofail` - removes the errorcheck.
- `x-gvfs-show` - show the mount option in the file manager. If this is on a GUI-less server, this option won't be necessary.
- `0` - determines which filesystems need to be dumped (0 is the default).
- `0` - determine the order in which filesystem checks are done at boot time (0 is the default).

Save and close the file.

### Testing the entry

**Before you reboot the machine, you need to test your new fstab entry.** To do this, issue the command:

`sudo mount -a`

If you see no errors, the fstab entry is correct and you're safe to reboot. Congratulations, you've just created a proper fstab entry for your connected drive. Your drive will automatically mount every time the machine boots.

## Bash - Output song names from multiple folders to a file
##### OC

Put the script below in your root music directory and it will output the name of the songs to `albums.txt`.

### Notes:

If your music does not correspond to the following pattern you can edit the regex in the script (now it's `grep -E '[0-9][0-9]'`):<br>
`NR Name` eg. `06 Lose Yourself to Dance.flac` or `08. Waltz for Zizi.flac` etc.

Also if you have mp3, wav etc. files you can edit `sed 's/.flac//g'` to the respective file type (or make an OR operation for multiple types).

##### There are also two versions for Losslessclub, one for normal albums, web albums, vinyl rips etc. and one for discgraphy, which also includes the duration of the album (you have to edit this manually). If you don't know what I just talked about don't worry. Anyways, you can find them [in this Gist](https://gist.github.com/sabinM1/86a06a0785b01842aa316f68ed22f779).

---

`touch albums.sh && chmod +x albumes.sh` and edit the file with your preffered editor.
```bash
#!/bin/bash

# Forbid root rights
[ "$EUID" == "0" ] && echo -e "\e[91mDon't use sudo or root user to execute these scripts!\e[0m" && exit

export ALBUME="$PWD/albums.txt"

: > "$ALBUME" # clear (and create) file

for dir in ./*/
do cd -P "$dir" || continue
   printf %s\\n "$PWD" >&2
   echo -e "${PWD##*/}\n" >> "$ALBUME" && \ls -lha | cut -d " " -f 12-99 | grep -E '[0-9][0-9]' | sed 's/.flac//g' >> "$ALBUME" && echo -e "\n" >> "$ALBUME" && cd "$OLDPWD" || 
! break; done || ! cd - >&2

echo
```

(**NOT RECOMMENDED**):<br>
You can also `curl https://gist.githubusercontent.com/sabinM1/86a06a0785b01842aa316f68ed22f779/raw/albums.sh | bash` in your root music directory (assuming you have curl installed) if you don't want to create a script file **and you trust GitHub and me enough to run unverified scripts on your machine**. [The Gist](https://gist.github.com/sabinM1/86a06a0785b01842aa316f68ed22f779) is public and hopefully the same as the script above, but because there is a "hopefully",  you never know what it's being transmitted to bash if you don't verify it.

## Iptables
##### From [thomas-krenn.com](https://www.thomas-krenn.com/en/wiki/Saving_Iptables_Firewall_Rules_Permanently)

### Allow a port

Change *tcp* to *udp* if you need UDP.

```bash
sudo iptables -I INPUT -p tcp --dport PORT_NUMBER_HERE -j ACCEPT
```

### Permanently save Iptables

You need to have the *iptables-persistent* package installed (`apt install iptables-persistent`).

On debian-based distro, run as root:
 - IPv4:
```bash
iptables-save > /etc/iptables/rules.v4
```
 - IPv6:
```bash
ip6tables-save > /etc/iptables/rules.v6
```

### Restore Iptables from file

On debian-based distro, as root:
```bash
iptables-restore < /etc/iptables/rules.v4
```
