# RHCSA Study Guide

### Table of Contents
- [Stdout and Stderr](#stdour-and-stderr)
- [Bash](#bash)
- [Compression and Archiving](#compression-and-archiving)
- [Links](#links)

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
