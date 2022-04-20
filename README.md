# RHCSA Study Guide

Quick study guide to help me study before the RHCSA exam!

### Table of Contents
* [Stdout and Stderr](#stdour-and-stderr)
* [Bash](#bash)
    * [Bash Arguments](#bash-arguments)
* [Compression and Archiving](#compression-and-archiving)
    * [Gzip](#gzip)
    * [Bzip2](#bzip2)
    * [Tar](#tar)
* [Links](#links)
    * [Soft Links](#soft-links)
    * [Hard Links](#hard-links)
* [Processes](#processes)
    * [Find Processes](#find-processes)
    * [Kill Processes](#kill-processes)
* [Logs](#logs)
* [Reset Root Password](#reset-root-password)
* [Disk Storage](#disk-storage)
* [Logical Volume Manager](#logical-volume-manager)
    * [Physical Volumes](#physical-volumes)
    * [Volume Group](#volume-group)
    * [Logical Volume](#logical-volume)
* [Network Files System (NFS)](#network-file-system-nfs)
    * [AutoFS](#autofs)
* [Linux Permissions](#linux-permissions)
    * [Special Permissions](#special-permissions)
    * [Access Control Lists](#access-control-lists)
* [Mounting Stratis File System](#mounting-stratis-file-system)
*[Crontab](#crontab)

## Stdout and Stderr

Values always used
- 0: stdin
- 1: stdout
- 2: stderr

Redict stdout

```
$ 1>
$ ./error.sh 1> capture.txt
```

Redict stderr

```
$ 2>
$ ./error.sh 2> capture.txt
```

Redict stdout to one file and stderr to another

```
$ ./error.sh 1> capture.txt 2> error.txt
```

Redirect stdout and stderr to the same file. This uses the &> to redirect instruction, which allows the shell to make one stream go to the same destination as another stream.
"redirect stream 2, stderr, to the same destination that stream 1, stdout, is being redirected to"
```
$ ./error.sh > capture.txt 2>&1
```

## Bash
- [Bash Arguments](#bash-arguments)

### Bash Arguments
Positional Paraments are arguments passed to a script which are processed in the same order in which they're sent. Index starts at 1, the positional parameter refers to this representation of the arguments using their position
```bash
$ echo "Username: $1";
$ echo "Age: $2";
```

The variable **$@** is the array of all the input parameters
```bash
$ for user in "$@"
$ do
$   echo "Username: $user"'
$ done
```

The variable **$#** returns the input size
```bash
$ i=1;
$ j=$#;
$ while [ $i -le $j ] 
$ do
$     echo "Username - $i: $1";
$     i=$((i + 1));
$     shift 1;
$ done
```

## Compression and Archiving
- [Gzip](#gzip)
- [Bzip2](#bzip2)
- [Tar](#tar)

### Gzip


Most popular, uses the command `gzip` to create a zip file in `.gz` format.
```
$ gzip filename
```
Or decompress with either `gzip -d` or using `gunzip`
```
$ gzip -d filename.gz
$ gunzip filename.gz
```

### Bzip2
Well-known and available on most major Linux distros. Can compress using the command `bzip2` to create a zip file in `.bz2` format.
`-z` enables file compression
```
$ bzip2 filename
$ bzip2 -z filename
```
Use `bzip2 -d` in order to decompress files
```
$ bzip2 -d filename.bz2
```

### Tar
The `tar` utility stands for *tape archive* which allows to create backups using `tar gzip bzip`. It compresses files and directories into an archive file known as a *tarball*.

Creating a backup of of the directory
- `-c` Create the archive
- `-v` Show the process verbosely
- `-f` Name the archive
```
$ tar -cvf backup.tar /home/user
```

Creating a `gzip` archive backup of of the directory
- `-c` Create the archive
- `-v` Show the process verbosely
- `-f` Name the archive
- `-z` Compressed gzip archive file
```
$ tar -cvfz backup.tar.gz /home/user
```
We can extract the archive file the `-x` switch

Extracting a `gzip` archive backup of of the directory
```
$ tar -xvfz backup.tar.gz
```
Extracting archive backup of of the directory
```
$ tar -xvf backup.tar
```

## Links
- [Soft Links](#soft-links)
- [Hard Links](#hard-links)

### Soft Links
Symbolic link, soft link/symlink, is a special file that serves as a reference to another file or directory.
Created with the `ln` command and `-s` switch
```bash
$ ln -s {source-filename} {symbolic-filename/symbolic-dir-name}
$ ln -s file1 link1
```

### Hard Links
The link is between the filename and the actual data stored on the filesystem. Creating a hard link creates a new filename pointing to the extact same data as the old filename. Changes made to one filename, the other reflects those changes. The permissions, link count, owenership, time stamps, and file content are the exact same, data is only removed from your drive when all links to the data have been removed
```bash
$ ln {source-filename} {symbolic-filename/dir-name}
$ ln link_test /tmp/link_new
```

## Processes
- [Find Processes](#find-processes)
- [Kill Processes](#kill-processes)

### Find Processes
A PID is auto assigned to each process when it is created.

You can use `pidof` or `ps` command to get the process PID. The command `pidof` finds a runnig process by name while `ps` displays information about processes while `ps aux` displays all running processes, shows user/owner column in output, and prints the processes that have not been executed from the terminal.
```
$ pidof httpd
$ ps aux | grep httpd 
```

### Kill Processes
Can kill a process with either `kill` or `killall`. The `kill` ends a process using it's PID while `killall` ends a process by the process's name.
```
$ kill {PID}
$ killall {Process-Name}
```

## Logs

The `/etc/rsyslog.conf` stores all the log locations for the different logs.

The `journalctl` command is used to view the systemd, kernel, and journal logs, which displays the oldest entries first.

You can set `journalctl` to store logs persisently by either creating the following directory
```
$ mkdir -p /var/log/journal
```
Or by by editing the `systemd-journald` configuratin file and seting Storage=auto to Storage=persistent
```bash
$ vim /etc/systemd/journald.conf
$ #Storage=auto > Storage=persistent
```

## Reset Root Password

Reboot the system and on GRUB2 boot screen, press `e` key to interrupt the boot process.

At the end of the line that begins with *linux* add `rd.break` to the end of the line
```bash
linux ($root)/vmlinuz-4.18.0-80.e18.x86_64 root=/dev/mapper/rhel-root ro crash kernel=auto resume=/dev/mapper/rhel-swap rd.lvm.lv/swap rhgb quiet rd.break
```
Press `Ctrl+X` to start the system where `switch_root` prompt will appear. Remount the file system as writable
```
# mount -o remount,rw /sysroot
```
The file system is mounted as read-only in the /sysroot directory. Remounting the file system as writable allows you to change the password. Enter `chroot` environment
```
chroot /sysroot
```

The `sh-4.4#` prompt will appear, reset the root password
```
# passwd
```
Then enable SELinux relabeling process on the next system boot
```
# touch /.autorelabel
# exit
$ exit
```
## Disk Storage
The `lsblk` commands allows you to display a list of available block devices, while the `blkid` command allows you to display information about available block devices.

By default it lists all available block devices, to display information about a particular device only then specify the device name
```
$ blkid /dev/sdb1
```

The `fdisk` command suite is a partitioning utility that can list, create, and remove. To format, use the `mkfs` command instead.

List out the partitions of a disk
```
$ fdisk -l
```

Create a new partition by selectng a primary disk that has unused space
```
$ fdisk /dev/sdb
 Welcome to fdisk (util-linux 2.32.1).
 Changes will remain in memory only until you decide to write them.
 Be careful before using the write command.
 
 Does not contain a recognized partition table.
     Created a new DOS disklabel with disk identifier 0x569c5370.
    
 Command (m for help): n
 Partition type
    p   primary (0 primary, 0 extended, 4 free)
    e   extended (container for logical partitions)
 Select (default p): p
 Partition number (1-4, default 1): 1
 First sector (2048-2097151, default 2048):
 Last sector, +sectors or +size{K,M,G,T,P} (2048-2097151, default 2097151): +500
    
 Created a new partition 1 of type 'Linux' and of size 250.5 KiB.
 **Be sure to write your changes to disk using the `w` flag**
```

Deleting a partition would be same command.
```
$ fdisk /dev/sdb
 Welcome to fdisk (util-linux 2.32.1).
 Changes will remain in memory only until you decide to write them.
 Be careful before using the write command.
    
 Command (m for help): d
 Selected partition 1
 Partition 1 has been deleted.
```
## Logical Volume Manager
- [Physical Volumes](#physical-volumes)
- [Volume Group](#volume-group)
- [Logical Volume](#logical-volume)


```mermaid
    graph TD;
        PV1 --> VG1;
        PV2 --> VG1;
        PV3 --> VG2;
        VG1 --> LV;
        VG2 --> LV;
```


### Physical Volumes
A physical volume is any physical storage device that has been initialized as a physical volume with LVM.
LVM places labels on the physical volumes' UUID and metadata storage when initialized.
Uses the `pvs` command to to see what physical volumes are configured. To add additional PV, use the `pvcreate` command.
```
$ pvs
$ pvcreate /dev/sdb
```

### Volume Group
Physical volumes are combined into volume groups (VGs), which creates a pool of disk space out of which logical volumes can be allocated.
Within a volume group, the disk space available for allocation is divided into units of a fixed-size called extents. An extent is the smallest unit of space that can be allocated. Within a physical volume, extents are referred to as physical extents.

To create a volume group, use `vgcreate` command which wil create a new volume group by name and adds at least one physical volume to it.

```
$ vgcreate vg1 /dev/sdb1
```

Use the `vgs` command to view existing volume groups. To add additional physical volumes to an existing volume gorup, use `vgextend` command. And to remove the physical volume, uses the `vgreduce` command.

```
$ vgextend vg1 /dev/sdc1
$ vgreduce vg1 /dev/sdb1
```

### Logical Volume
Logical volumes are made up of volume groups.

## Network File System (NFS)
A *Network File System (NFS) allows remotes hosts to mount file systems over a network and interact with those file systems as though they are mounted locally.

### AutoFS
The `automount` utlility can mount and umount NFS file systems automatically and can be used to mount other file systems.

`autofs` uses `/etc/auto.master` (master map) as its default primary configuration file.

Direct maps in `autofs` provide a mechanism to automatically mount file systems at arbitrary points in the file system hierarchy. A direct map is denoted by a mount point of `/-` in the master map

The primary configuration file for the automounter is `/etc/auto.master`, the format of the master map is as follows
```
{mount-point map-name options}
/home /etc/auto.misc
```
**Mount-point** refers to the `autofs` mount point, this can be a single directory name for an indirect mount of the full path of the mount point for direct mounts.

**Location** refers to the file system location such as a local file system path, an NFS file system, or other valid file system location.

There are two ways to configure exports on an NFS Server
- Manually editing the NFS configuration file `/etc/exports`
- Through the command line by using `exportfs`

The `/etc/exports` file controls which file systems are exported to remote hosts and specifies options. Each entry for an exported file system has the following structure
```
{exort host(options)}
/home   server2(rw,no_root_squash)
```
**export** The directory being exported

**host** The host or network to which the export is being shared

**options** the options to be used for host

On default `root_squash` is used to prevent root users connected remotely from having root privileges, and instead the NFS server assigns them the user ID `nfsnobody`. The exported file system is default to read-only `ro` as well.

To able asynchronous writes, specify with the option `async`.

NFS requires `rpcbind`, which dynamically assigns ports for RPC services and can cause issues for configuring firewall rules, to allow clients to access NFS shares behind a firewall, allow access through firewall.
```
$ firewall-cmd --permanent --add-service=rpc-bind
```
When the nfs service starts, the /usr/sbin/exportfs command launches and reads this file, passes control to `rpc.mountd`

Allow `mountd` through the firewall,
```
$ firewall-cmd --permanent --add-service=mountd
```

Configuring `autofs` service to mount user home directories automatically.
- Client needs to use the following `/etc/auto.master` map
```
/home   /etc/auto.home
+auto.master
```
- The `/etc/auto.home` map contains the entry
```
*   server2:/export/home/&
```


## Linux Permissions

- *Read* Numeric 4
- *Write* Numeric 2
- *Execute* Numeric 1

### Special Permissions
Special Permissions allow for additional privileges over the standard permission sets

**SUID**

Special permissions for the user access level. A file with **SUID** always executes as the user who owns the file, regardless of the user passing the command. `u+s`

**SGID**

- if set on a file, allows the file to be executed as the group that owns the file
- set on a directory, any files created in the directory will have their group ownership set to that of the directory owner. `g+s`

**Sicky Bit**

This permission does not affect individual files but on the directory level it restricts file deletion. Only the owner (and root) can rmeove the file within that directory. `o+t`

**Numerical Value**

To use the numerical method, pass a fourth digit preceding in the `chmod` command
- Start at 0
- **SUID** = 4
- **SGID** = 2
- **Sticky** = 1
```
$ chmod X### file | directory
$ chmod 2770 /community_content/
```

### Access Control Lists
An access ACL is the access control list for a specific file or directory

A default ACL can only be associated with a directory; if a file within the directory does not have an access AC:L, is uses the rules of the default ACL for the directory

The `setfacl` utility sets ACLs for files and directories.
```
$ setfacl -m rules files
```

Rules (rules) must be specified in the following formats. Multiple rules can be specified in the same command if they are separated by commas.

- **u:uid:perms** Sets the access ACL for a user. The user name or UID may be specified. The user may be any valid user on the system.
- **g:gid:perms** Sets the access ACL for a group. The group name or GID may be specified. The group may be any valid group on the system.
- **m:perms** Sets the effective rights mask. The mask is the union of all permissions of the owning group and all of the user and group entries.
- **o:perms** Sets the access ACL for users other than the ones in the group for the file.

Permissions (perms) must be a combination of the characters r, w, and x for read, write, and execute.

To set a default ACL, add `d:` before the rule and specify a directory instead of file name
```
$ setfacl -m d:o:rx /share
```

To determine the existing ACLs for a file or directory, use the `getfacl` command
```
$ getfacl home/john/picture.png
```

## Mounting Stratis File System

When mounting a Stratis File system persistently using `/etc/fstab`, use `xfs` as the file system type and add the `x-systemd.requires=stratisd.service`
```
UUID=a1f0b64a-4ebb-4d4e-9543-b1d79f600283 /mnt/fs1 xfs defaults,x-systemd.requires=stratisd.service
```

## Crontab

`Crontab` file is a simple text file that instructs the `cron` daemon to perform a task at a certain time or interval. Any user can schedule `cron` tasks or jobs on a system, the task runs under the user account from which it was created
`crontab` file has a specific syntax to it.
```
* * * * * /path/to/script
Minute (0-59)
    |
    Hour (0-23)
        |
        Day of month (1-31)
            |
            Month of year (1-12)
                |
                Day of week (0-7), 0 & 7 are Sunday
```
An asterisk (`*`) means every or all, as in every minute or all hours, every day, and so on.

To allow or deny access to specific users, crontab uses the files `/etc/cron.allow` and `/etc/cron.deny`. Based on the existence of `/etc/cron.allow` and `/etc/cron.deny` files, crontab decides whom to give access to cron in following order.

- If `cron.allow` exists – only the users listed in the file `cron.allow` will get an access to crontab.
- If `cron.allow` does not exist – all users except the users listed into cron.deny can use crontab
- If neither of the file exists – only the root can use crontab
- If a user is listed in both `cron.allow` and `cron.deny` – that user can use crontab.

If the `cron.allow` file exists, then you must be listed therein in order to be allowed to use this command. If the cron.allow file does not exist but the `cron.deny` file does exist, then you must not be listed in the `cron.deny` file in order to use this command.