
## Essentials

### Table of Contents

- [Variables](#variables)
  - [Local Variables](#local)
  - [Environment Variables](#environment-variables)
- [Command Types](#command-types)
  - [Internal Commands](#internal-commands)
  - [External Commands](#external-commands)
  - [Aliases](#aliases)
  - [Functions](#functions)
- [Quoting](#quoting)
  - [Double Quotes](#double-quotes)
  - [Single Quotes](#single-quotes)
  - [Backslash Character](#backslash-character)
  - [Backquotes](#backquotes)
- [Control Statements](#control-statements)
  - [Semicolons](#semicolons)
  - [Double Ampersand](#double-ampersand)
  - [Double Pipe](#double-pipe)

### Variables

Variables are given names and stored temporarily in memory. There are two types of variables used in the Bash shell: "local" and "environment".

#### Local

```
sysadmin@localhost:~$ variable1='Something'
```

```
sysadmin@localhost:~$ echo $variable1
```

#### Environment variables

Environment variables, also called global variables, are available system-wide, in all shells used by Bash when interpreting commands and performing tasks. The system automatically recreates environment variables when a new shell is opened. Examples include the PATH, HOME, and HISTSIZE variables.

```
sysadmin@localhost:~$ HISTSIZE=500                                            
sysadmin@localhost:~$ echo $HISTSIZE                              
500  
```

The export command is used to turn a local variable into an environment variable.

```
export variable
```

After exporting variable1, it is now an environment variable. It is now found in the search through the environment variables:

```
sysadmin@localhost:~$ export variable1                                  
sysadmin@localhost:~$ env | grep variable1
variable1=Something
```

Exported variables can be removed using the unset command:

```
sysadmin@localhost:~$ unset variable2
```

### Command Types 

There are several different sources of commands within the shell of your CLI including internal commands, external commands, aliases, and functions.

#### Internal Commands

Also called built-in commands, internal commands are built into the shell itself. A good example is the cd (change directory) command as it is part of the Bash shell. 

#### External Commands

External commands are binary executables stored in directories that are searched by the shell. If a user types the ls command, then the shell searches through the directories that are listed in the PATH variable to try to find a file named ls that it can execute.

It would be tedious to have to manually look in each directory that is listed in the PATH variable. Instead, use the which command to display the full path to the command in question:

```bash
which command 
```

The which command searches for the location of a command by searching the PATH variable.

```bash
sysadmin@localhost:~$ which ls                                       
/bin/ls                                                               
sysadmin@localhost:~$ which cal                                        
/usr/bin/cal
```

#### Aliases

An alias can be used to map longer commands to shorter key sequences. When the shell sees an alias being executed, it substitutes the longer sequence before proceeding to interpret commands.

For example, the command ls -l is commonly aliased to l or ll. Because these smaller commands are easier to type, it becomes faster to run the ls -l command line.

New aliases can be created using the following format, where name is the name to be given the alias and command is the command to be executed when the alias is run.

```
sysadmin@localhost:~$ alias mycal="cal 2019"                                    
sysadmin@localhost:~$ mycal                                                     
                            2019                                                
      January               February               March                        
Su Mo Tu We Th Fr Sa  Su Mo Tu We Th Fr Sa  Su Mo Tu We Th Fr Sa                
       1  2  3  4  5                  1  2                  1  2                
 6  7  8  9 10 11 12   3  4  5  6  7  8  9   3  4  5  6  7  8  9                
13 14 15 16 17 18 19  10 11 12 13 14 15 16  10 11 12 13 14 15 16                
20 21 22 23 24 25 26  17 18 19 20 21 22 23  17 18 19 20 21 22 23                
27 28 29 30 31        24 25 26 27 28        24 25 26 27 28 29 30                
                                            31
```
Aliases created this way only persist while the shell is open. Once the shell is closed, the new aliases are lost. Additionally, each shell has its own aliases, so aliases created in one shell won’t be available in a new shell that’s opened.

#### Functions

Typically, functions are used to execute multiple commands.

Functions are useful as they allow for a set of commands to be executed one at a time instead of typing each command repeatedly. In the example below, a function called my_report is created to execute the ls, date, and echo commands.

```bash
sysadmin@localhost:~$ my_report () {                                            
> ls Documents                                                                  
> date                                                                          
> echo "Document directory report"                                              
> }     
```

## Quoting

There are three types of quotes that have special significance to the Bash shell: double quotes ", single quotes ', and back quotes `. Each set of quotes alerts the shell not to treat the text within the quotes in the normal way.

#### Double Quotes

Double quotes stop the shell from interpreting some metacharacters (special characters), including glob characters (*, [], ?). 

Within double quotes an asterisk is just an asterisk, a question mark is just a question mark, and so on, which is useful when you want to display something on the screen that is normally a special character to the shell. 

Double quotes still allow for command substitution, variable substitution, and permit some other shell metacharacters.
  
```bash
sysadmin@localhost:~$ echo "The path is $PATH"                          
The path is /usr/bin/custom:/home/sysadmin/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games
```

#### Single Quotes

Single quotes prevent the shell from doing any interpreting of special characters, including globs, variables, command substitution and other metacharacters.

```bash
sysadmin@localhost:~$ echo The car costs $100                           
The car costs 00                                                        
sysadmin@localhost:~$ echo 'The car costs $100'                        
The car costs $100
```

#### Backslash Character

Use a backslash \ character in front of the dollar sign $ character to prevent the shell from interpreting it. The command below demonstrates using the \ character:

```bash
sysadmin@localhost:~$ echo The service costs \$1 and the path is $PATH
The service costs $1 and the path is /usr/bin/custom:/home/sysadmin/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games
```

#### Backquotes

Backquotes, or backticks, are used to specify a command within a command, a process called command substitution.

To execute the date command and have the output of that command sent to the echo command, put the date command in between two backquote characters:

```
sysadmin@localhost:~$ echo Today is `date`                         
Today is Mon Nov 4 03:40:04 UTC 2018
```

## Control Statements

#### Semicolons

```
command1; command2; command3
```
Each command runs independently and consecutively; regardless of the result of the first command, the second command runs once the first has completed, then the third and so on.

#### Double Ampersand

```
command1 && command2
```
The double ampersand && acts as a logical "and"; if the first command is successful, then the second command will also run. If the first command fails, then the second command will not run.

#### Double Pipe

```
command1 || command2
```
With the double pipe, if the first command runs successfully, the second command is skipped; if the first command fails, then the second command is run.

```
sysadmin@localhost:~$ ls /etc/ppp || echo failed                 
ip-down.d  ip-up.d              
sysadmin@localhost:~$ ls /etc/junk || echo failed                  
ls: cannot access /etc/junk: No such file or directory             
failed
```