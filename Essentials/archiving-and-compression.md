# Archiving and Compression

This section covers the essential tools and techniques for archiving and compressing files in Linux. It explains how to use `gzip` for compression and `tar` for creating, listing, and extracting archives. These tools help manage file storage efficiently and simplify the process of transferring multiple files.

## Table of Contents

- [Compression with `gzip`](#compression-with-gzip)
  - [Compressing Files](#compressing-files)
  - [Decompressing Files](#decompressing-files)
- [Archiving Files](#archiving-files)
  - [Create Mode](#create-mode)
  - [List Mode](#list-mode)
  - [Extract Mode](#extract-mode)

---

## Compression with `gzip`

Linux provides several tools to compress files; the most common is `gzip`. Here we show a file before and after compression:

```bash
sysadmin@localhost:~/Documents$ ls -l longfile*
-rw-r--r-- 1 sysadmin sysadmin 66540 Dec 20  2017 longfile.txt
sysadmin@localhost:~/Documents$ gzip longfile.txt
sysadmin@localhost:~/Documents$ ls -l longfile*
-rw-r--r-- 1 sysadmin sysadmin 341 Dec 20  2017 longfile.txt.gz
```

Compressed files can be restored to their original form using either the gunzip command or the `gzip –d` command. This process is called decompression. After gunzip does its work, the longfile.txt file is restored to its original size and file name.

```bash
sysadmin@localhost:~/Documents$ gunzip longfile.txt.gz
sysadmin@localhost:~/Documents$ ls -l longfile*
-rw-r--r-- 1 sysadmin sysadmin 66540 Dec 20  2017 longfile.txt
```

## Archiving Files

The traditional UNIX utility to archive files is called `tar`. 
The `tar` command takes in several files and creates a single output file that can be split up again into the original files on the other end of the transmission.

The tar command has three modes that are helpful to become familiar with:
 - Create: Make a new archive out of a series of files.
 - Extract: Pull one or more files out of an archive.
 - List: Show the contents of the archive without extracting.

### Create Mode 

```
tar -c [-f ARCHIVE] [OPTIONS] [FILE...]
```

### List Mode 

```
tar -t [-f ARCHIVE] [OPTIONS]
```

Given a tar archive, compressed or not, you can see what’s in it by using the **-t** option. 

### Extract Mode 

```
tar -x [-f ARCHIVE] [OPTIONS]
```

Creating archives is often used to make multiple files easier to move. Before extracting the files, relocate them to the Downloads directory:

```bash
sysadmin@localhost:~/Documents$ cd ~
sysadmin@localhost:~$ cp Documents/folders.tbz Downloads/folders.tbz
sysadmin@localhost:~$ cd Downloads
```
Finally, you can extract the archive with the –x option once it’s copied into a different directory. The following example specifying the operation, the compression, and a file name to operate on.

Add the **–v** flag and you will get a verbose output of the files processed, making it easier to keep track of what's happening:

```bash
sysadmin@localhost:~/Downloads$ tar -xjvf folders.tbz
School/
School/Engineering/
School/Engineering/hello.sh
School/Art/
School/Art/linux.txt
School/Math/
School/Math/numbers.txt
```

It is important to keep the **–f** flag at the end, as `tar` assumes whatever follows this option is a file name.
