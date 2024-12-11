# Working with Text

### Viewing Files in the Terminal 

The `cat` command, short for concatenate, is a simple but useful command whose functions include creating and displaying text files, as well as combining copies of text files. One of the most popular uses of `cat` is to display the content of text files. To display a file in the standard output using the `cat` command, type the command followed by the filename:
```bash
sysadmin@localhost:~$ cd Documents
sysadmin@localhost:~/Documents$ cat food.txt 
Food is good.
```

Although the terminal is the default output of this command, the `cat` command can also be used for redirecting file content to other files or input for another command by using redirection characters.

### Viewing Files Using a Pager

There are two commonly used pager commands:

- The `less` command provides a very advanced paging capability.It is usually the default pager used by commands like the `man` command.
- The `more` command has been around since the early days of UNIX. While it has fewer features than the `less` command, however, the `less` command isn't included with all Linux distributions. The `more` command is always available.

#### Pager Movement Commands

To view a file with the `less` command, pass the file name as an argument:
```bash
sysadmin@localhost:~/Documents$ less words
```

![Pager movements commands](./images/pager-movements-commands.jpg)

#### Pager Searching Commands

There are two ways to search in the `less command`: searching forward or backward from your current position.

To start a search to look forward from your current position, use the slash **/** key. 

To search backward from your current position, press the question mark **?** key, then type the text or pattern to match and press the Enter key. The cursor moves backward to the first match it can find or reports that the pattern cannot be found.

If more than one match can be found by a search, then use the **n** key to move the next match and use the **Shift+N** key combination to go to a previous match.

### Head and Tail 

The `head` and `tail` commands are used to display only the first few or last few lines of a file, respectively (or, when used with a pipe, the output of a previous command). By default, the `head` and `tail` commands display ten lines of the file that is provided as an argument.

Passing a number as an option will cause both the `head` and `tail` commands to output the specified number of lines, instead of the standard ten. For example to display the last five lines of the /etc/sysctl.conf file use the **-5** option.

#### Negative Value Option 

The GNU version of the head command recognizes -n -3 as show all but the last three lines, and yet the head command still recognizes the option -3 as show the first three lines.

#### Positive Value Option

If the **-n** option is used with a number prefixed by the plus sign, then the tail command recognizes this to mean to display the contents starting at the specified line and continuing all the way to the end.

For example, the following displays the contents of the /etc/passwd from line 25 to the end of the file. 

```bash
sysadmin@localhost:~$ nl /etc/passwd | tail -n +25
    25  sshd:x:103:65534::/var/run/sshd:/usr/sbin/nologin
    26  operator:x:1000:37::/root:/bin/sh
    27  sysadmin:x:1001:1001:System Administrator,,,,:/home/sysadmin:/bin/bash
```

#### Live chages

Live file changes can be viewed by using the **-f** option to the tail command—useful when you want to see changes to a file as they are happening.

## Comand Line Pipes 

The pipe **|** character can be used to send the output of one command to another.

The pipe character allows you to utilize `head` and `tail` commands not only on files, but on the output of other commands. This can be useful when listing a large directory, for example the /etc directory there are a large number of files. To more easily view the beginning of the output, pipe it to the head command. The following example displays only the first ten lines:
```bash
sysadmin@localhost:~$ ls /etc | head
X11
adduser.conf
alternatives
apparmor
apparmor.d
apt
bash.bashrc
bind
bindresvport.blacklist
binfmt.d
```

It is important to carefully choose the order in which commands are piped, as each command only sees input from the previous command. The examples below illustrate this using the **nl** command, which adds line numbers to the output. In the first example, the **nl** command is used to number the lines of the output of the preceding `ls` command:

```bash
sysadmin@localhost:~$ ls /etc/ssh | nl
     1  moduli
     2  ssh_config
     3  ssh_host_ecdsa_key
     4  ssh_host_ecdsa_key.pub
     5  ssh_host_ed25519_key
     6  ssh_host_ed25519_key.pub
     7  ssh_host_rsa_key
     8  ssh_host_rsa_key.pub
     9  ssh_import_id
    10  sshd_config
```

In the next example, note that the `ls` command is executed first and its output is sent to the `nl` command, numbering all of the lines from the output of the `ls` command. Then the `tail` command is executed, displaying the last five lines from the output of the `nl` command: 

```bash
sysadmin@localhost:~$ ls /etc/ssh | nl | tail -5
     6  ssh_host_ed25519_key.pub
     7  ssh_host_rsa_key
     8  ssh_host_rsa_key.pub
     9  ssh_import_id
    10  sshd_config
```

Compare the output above with the next example:

```bash
sysadmin@localhost:~$ ls /etc/ssh | tail -5 | nl
     1  ssh_host_ed25519_key.pub
     2  ssh_host_rsa_key
     3  ssh_host_rsa_key.pub
     4  ssh_import_id
     5  sshd_config
```

## Input/Output Redirection

Input/Output (I/O) redirection allows for command line information to be passed to different streams. 

**STDIN** Standard input, or STDIN, is information entered normally by the user via the keyboard. When a command prompts the shell for data, the shell provides the user with the ability to type commands that, in turn, are sent to the command as STDIN.

**STDOUT** Standard output, or STDOUT, is the normal output of commands. When a command functions correctly (without errors) the output it produces is called STDOUT. By default, STDOUT is displayed in the terminal window where the command is executing. STDOUT is also known as stream or channel #1.

**STDERR** Standard error, or STDERR, is error messages generated by commands. By default, STDERR is displayed in the terminal window where the command is executing. STDERR is also known as stream or channel #2.

I/O redirection allows the user to redirect **STDIN** so that data comes from a file and **STDOUT/STDERR** so that output goes to a file. Redirection is achieved by using the arrow **<** **>** characters.

### STDOUT 

STDOUT can be directed to files. 
Using the **>** character, the output of echo command can be redirected to a file:

```bash
sysadmin@localhost:~$ echo "Line 1" > example.txt
```

It is important to realize that the single arrow overwrites any contents of an existing file. 

It is also possible to preserve the contents of an existing file by appending to it. Use two arrow **>>** characters to append to a file instead of overwriting it. 

### STDERR 

STDERR can be redirected similarly to STDOUT. When using the arrow character to redirect, stream #1 (STDOUT) is assumed unless another stream is specified. Thus, stream #2 must be specified when redirecting STDERR by placing the number **2** preceding the arrow **>** character.

The STDERR output of a command can be sent to a file:

```bash
sysadmin@localhost:~$ ls /fake 2> error.txt
```
In the example, the **2>** indicates that all error messages should be sent to the file error.txt, which can be confirmed using the cat command:
```bash
sysadmin@localhost:~$ cat error.txt
ls: cannot access /fake: No such file or directory
```

### Redirecting Multiple Streams

It is possible to direct both the STDOUT and STDERR of a command at the same time. 

Both STDOUT and STDERR can be sent to a file by using the ampersand **&** character in front of the arrow **>** character. The **&>** character set means both **1>** and **2>**:

```bash
sysadmin@localhost:~$ ls /fake /etc/ppp &> all.txt
sysadmin@localhost:~$ cat all.txt
ls: cannot access /fake: No such file or directory
/etc/ppp:
ip-down.d
ip-up.d
```

Note that when you use **&>**, the output appears in the file with all of the STDERR messages at the top and all of the STDOUT messages below all STDERR messages.

If you don't want STDERR and STDOUT to both go to the same file, they can be redirected to different files by using both **>** and **2>**. For example, to direct STDOUT to example.txt and STDERR to error.txt execute the following:
```bash
sysadmin@localhost:~$ ls /fake /etc/ppp > example.txt 2> error.txt
sysadmin@localhost:~$ cat error.txt
ls: cannot access /fake: No such file or directory
sysadmin@localhost:~$ cat example.txt
/etc/ppp:
ip-down.d
ip-up.d
```

### STDIN 

The concept of redirecting STDIN is a difficult one because it is more difficult to understand why you would want to redirect STDIN. 

Most Linux users end up redirecting STDOUT routinely, STDERR on occasion, and STDIN very rarely.

The first command in the example below redirects the output of the cat command to a newly created file called new.txt. This action is followed up by providing the cat command with the new.txt file as an argument to display the redirected text in STDOUT.

```bash
sysadmin@localhost:~$ cat > new.txt
Hello
How are you?
Goodbye
sysadmin@localhost:~$ cat new.txt                               
Hello
How are you?
Goodbye
```

It is possible, however, to tell the shell to get STDIN from a file instead of from the keyboard by using the **<** character:
```bash
sysadmin@localhost:~$ tr 'a-z' 'A-Z' < example.txt
/ETC/PPP:
IP-DOWN.D
IP-UP.D
```
Most commands do accept file names as arguments, so this use case is relatively rare. However, for those that do not, this method could be used to have the shell read from the file instead of relying on the command to have this ability.

One last note to save the resulting output, redirect it into another file:
```bash
sysadmin@localhost:~$ tr 'a-z' 'A-Z' < example.txt > newexample.txt
sysadmin@localhost:~$ cat newexample.txt
/ETC/PPP:
IP-DOWN.D
IP-UP.D
```