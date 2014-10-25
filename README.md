HDFS for Go
===========

[![build](https://travis-ci.org/colinmarc/hdfs.svg?branch=master)](https://travis-ci.org/colinmarc/hdfs)

`hdfs` is a native go client for hdfs, using the protocol buffers interface the
namenode provides. It implements protocol version 9, which means it supports
Hadoop 2.0.0 and up (including CDH5).

It tries to be idiomatic by aping the stdlib `os` package where possible. This
includes implementing `os.FileInfo` for file status, and returning
`os.ErrNotExist` for missing files, for example.

It is heavily indebted to [snakebite](https://github.com/spotify/snakebite).

The best place to get started is the
[Godoc](https://godoc.org/github.com/colinmarc/hdfs).

```go

client, _ := hdfs.New("namenode:8020")

file, _ := client.Open("/mobydick.txt")

buf := make([]byte, 59)
file.ReadAt(buf, 48847)

fmt.Println(string(buf))
// => Abominable are the tumblers into which he pours his poison.
```

The `hdfs` Binary
-----------------

The library also ships with a command line client for hdfs. Like the library,
its primary aim is to be idiomatic, by enabling your favorite unix verbs:

```
$ hdfs --help
Usage: ./hdfs COMMAND
The flags available are a subset of the POSIX ones, but should behave similarly.

Valid commands:
  ls [-la] [FILE]...
  rm [-r] FILE...
  mv [-fT] SOURCE... DEST
  mkdir [-p] FILE...
  touch [-amc] FILE...
  chmod [-R] OCTAL-MODE FILE...
  chown [-R] OWNER[:GROUP] FILE...
  cat SOURCE...
  head [-n LINES | -c BYTES] SOURCE...
  tail [-n LINES | -c BYTES] SOURCE...
  get SOURCE [DEST]
  getmerge SOURCE DEST

```

It's also pretty fast compared to `hadoop -fs`, and, best of all, comes with
bash tab completion!

You can install it with `go get`, or by cloning and running `make install`
(alternatively, running `make` will create a binary in the source directory, so
you can install it where you want). To enable tab completion, source
`cmd/hdfs/bash_completion`, or drop it into the bash completion directory for
platform (on linux, this is usually `/etc/bash_completion.d`).