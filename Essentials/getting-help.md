
# Help 

The **Help** section provides an overview of various methods and tools for accessing Linux command documentation, finding files, and navigating manual pages. These methods range from the `man` and `info` commands to the use of options like `--help`. It also includes techniques for locating commands and additional system documentation.

## Table of Contents

- [Man Pages](#man-pages)
  - [Man Page Sections](#categorization-by-sections)
  - [Searching in Man Pages](#searching)
  - [Man Page Navigation](#navigation-controls)
- [Finding Command and Documentation](#finding-command-and-documentations)
  - [What is Command](#whatis-command)
  - [Whereis Command](#where-are-these-command-located)
  - [Locate Command](#find-any-file-or-directory)
- [Info Documentation](#info-documentation)
- [Using the Help Option](#using-the-help-option)
- [Additional System Documentation](#additional-system-documentation)

---

## Man Pages 

Man pages are used to describe the features of commands. They provide a basic description of the purpose of the command, as well as details regarding available options.

To view a man page for a command, use the man command:

```
sysadmin@localhost:~$ man ls
```

The following describes some of the more common sections found in man pages:

1. Name. Provides the name of the command and a very brief description.
2. Synopsys. It provides a concise example of how to use the command. 
```
SYNOPSIS 
     cal [-31jy] [-A number] [-B number] [-d yyyy-mm] [[month] year]
```
The square brackets [ ] are used to indicate that this options is optional but not mandatory to run command. For example, [-31jy] means options -3, -1, -j, or -y are available, but not required for the cal command to function properly.

The last set of square brackets [[month] year] mean that a year can be specified by itself, but to specify a month the year must also be specified.

Another component of the SYNOPSIS that might cause some confusion can be seen in the man page of the date command

```
SYNOPSIS                                                              
       date [OPTION]... [+FORMAT]                                      
       date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]
```
In this SYNOPSIS there are two syntaxes for the date command. The first one is used to display the date on the system while the second is used to set the date.

The ellipsis ... following [OPTION] indicates that one or more of the items before it may be used.

Additionally, the [-u|--utc|--universal] notation means that either the -u option or the --utc option or the --universal option may be used. Typically this means that all three options really do the same thing, but sometimes the use of the | character is used to indicate that the options can't be used in combination, like a logical “or".
3. Description. Provides a more detailed description of the command.
4. OPTIONS. Lists the options for the command as well as a description of how they are used. 
5. FILES. Lists the files that are associated with the command as well as a description of how they are used.
6. AUTHOR.
7. REPORTING BUGS. 
8. COPYRIGHT.

#### Searching 
To search a man page for a term, type the / character followed by a search term, then hit the Enter key. The program searches from the current location down towards the bottom of the page to try to locate and highlight the term.

If a match is found, it will be highlighted. To move to the next match of the term, press n. To return to a previous match of the term, press Shift+N. If the term is not found, or upon reaching the end of the matches, the program will report Pattern not found (press Return).

#### Navigation controls 
If the man command can find the manual page for the argument provided, then that manual page will be displayed using a command called less. The following table describes useful keys that can be used with the less command to control the output of the display:
[!Navigate output](./images/man-navigation-controls.png)

#### Categorization by sections 

By default, there are nine sections of man pages:

    1. General Commands
    2. System Calls
    3. Library Calls
    4. Special Files
    5. File Formats and Conventions
    6. Games
    7. Miscellaneous
    8. System Administration Commands
    9. Kernel Routines

To determine which section a specific man page belongs to, look at the numeric value on the first line of the output of the man page. For example, the cal command belongs to the first section of man pages:

```
CAL(1)                    BSD General Commands Manual             CAL(1)
```

To search for man pages by name, use the -f option to the man command. It displays man pages that match, or partially match, a specific name and provide the section number and a brief description of each man page:
```
sysadmin@localhost:~$ man -f passwd                                    
passwd (5)           - the password file                              
passwd (1)           - change user password                           
passwd (1ssl)        - compute password hashes     
```

Unfortunately, you won't always remember the exact name of the man page that you want to view. Fortunately, each man page has a short description associated with it. The -k option to the man command searches both the names and descriptions of the man pages for a keyword.

For example, to find a man page that displays how to copy directories, search for the keyword copy:
```
sysadmin@localhost:~$ man -k copy                                               
cp (1)               - copy files and directories                               
cpgr (8)             - copy with locking the given file to the password or gr...
cpio (1)             - copy files to and from archives                          
cppw (8)             - copy with locking the given file to the password or gr...
dd (1)               - convert and copy a file                                  
debconf-copydb (1)   - copy a debconf database                                  
install (1)          - copy files and set attributes                            
scp (1)              - secure copy (remote file copy program)                   
ssh-copy-id (1)      - use locally available keys to authorize logins on a re...
```

## Finding Command and Documentations

The whatis command (or man -f) returns what section a man page is stored in. 

#### Where are these command located 
To search for the location of a command or the man pages for a command, use the whereis command. This command searches for commands, source files and man pages in specific locations where these files are typically stored
```
sysadmin@localhost:~$ whereis ls 
ls: /bin/ls /usr/share/man/man1p/ls.1.gz /usr/share/man/man1/ls.1.gz
```

#### Find Any File or Directory

To find any file or directory, use the locate command. This command searches a database of all files and directories that were on the system when the database was created. Typically, the command to generate this database is run nightly.
```
sysadmin@localhost:~$ locate gshadow                                   
/etc/gshadow                                                           
/etc/gshadow-                                                          
/usr/include/gshadow.h                                                
/usr/share/man/cs/man5/gshadow.5.gz
...       
```
Note: The locate command makes use of a database that is traditionally updated once per day (normally in the middle of the night). This database contains a list of all files that were on the system when the database was last updated.

As a result, any files that you created today will not normally be searchable with the 'locate' command. If you have access to the system as the root user (the system administrator account), you can manually update this file by running the 'updatedb' command. Regular users cannot update the database file.

The output of the locate command can be quite large. When searching for a filename, such as passwd, the locate command produces every file that contains the string passwd, not just files named passwd.

In many cases, it is helpful to start by finding out how many files match. Do this by using the -c option to the locate command:

```
sysadmin@localhost:~$ locate -c passwd                                 
98
```
To limit the output produced by the locate command use the -b option. This option only includes listings that have the search term in the basename of the filename. The basename is the portion of the filename not including the directory names.
```
sysadmin@localhost:~$ locate -c -b passwd                              
83
```

As you can see from the previous output, there will still be many results when the -b option is used. To limit the output even further, place a \ character in front of the search term. This character limits the output to filenames that exactly match the term:
```
sysadmin@localhost:~$ locate -b "\passwd"                              
/etc/passwd                                                                     
/etc/pam.d/passwd                                                               
/usr/bin/passwd                                                                 
/usr/share/doc/passwd                                                           
/usr/share/lintian/overrides/passwd
```

## Info Documentation 

The 'info' command also provides documentation on operating system commands and features. The goal is slightly different from man pages: to provide a documentation resource that gives a logical organizational structure, making reading documentation easier.

```
info command
```
Note that going into the node about sorting leads into a sub-node of the original. To go back to the previous node, use the 'U' key. While U leads to the start of the node one level up, the 'L' key returns to the same location as before entering the sorting node.

## Using the Help Option

Many commands will provide basic information, very similar to the SYNOPSIS found in man pages, by simply using the --help option to the command. This option is useful to learn the basic usage of a command quickly without leaving the command line

```
sysadmin@localhost:~$  cat --help                                                
```

##  Additional System Documentation

On most systems, there is a directory where additional documentation (such as documentation files stored by third-party software vendors) is found.
‌⁠​​
These documentation files are often called readme files since the files typically have names such as README or readme.txt. The location of these files can vary depending on the distribution that you are using. Typical locations include 
```
/usr/share/doc and /usr/doc
```
Typically, this directory is where system administrators go to learn how to set up more complex software services. However, sometimes regular users also find this documentation to be useful.
