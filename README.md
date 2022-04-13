# RHCSA Study Guide

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