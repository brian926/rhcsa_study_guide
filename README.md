# RHCSA Study Guide

[Stdout and Stderr](#stdour-and-stderr)

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

