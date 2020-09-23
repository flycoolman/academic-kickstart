---
title: 'Whatis Locate Which Whereis'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary: A shor summary of the commands 'Whatis', 'Locate', 'Which', 'Whereis'.
authors:
- admin
tags:
- Linux
- System
categories:
- Linux
- System
date: "2020-09-10T00:00:00Z"
lastmod: "2020-09-23T00:00:00Z"
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Placement options: 1 = Full column width, 2 = Out-set, 3 = Screen-width
# Focal point options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
image:
  placement: 2
  caption: ''
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---


## Whatis Locate Which Whereis

### Whatis

    hongwei@840-g5:~$ whatis whatis
    whatis (1)           - display one-line manual page descriptions

### Locate

    hongwei@840-g5:~$ whatis locate
    locate (1)           - find files by name

**locate** indeed finds all the files that have the pattern specified anywhere in their paths. You can tell it to only find files and directories whose names (rather than full paths) include the pattern with the **-b** option, which is usually what you want, and gives a less unwieldy list.    

**locate** is fast because it uses a binary database that gets periodically updated (once daily, by cron). You can update it yourself to ensure recently added files are found by running **sudo updatedb**.  

One more thing about locate - **it doesn't care whether files still exist or not**, so to avoid finding recently deleted files, use -e. Often I also pipe to less as the list can be long. Typically I do:  

    sudo updatedb && locate -b -e psql | less

### Which

    hongwei@840-g5:~$ whatis which
    which (1)            - locate a command

**which** finds the binary executable of the program (if it is in your PATH)  

**which** returns the  pathnames of the files (or links) which would be executed in the current environment, had its arguments been given as commands in a strictly POSIX-conformant shell.  It does this by searching the **PATH** for executable files matching the names of the arguments. It does not canonicalize path names.

    hongwei@840-g5:~$ which psql
    /usr/bin/psql

### Whereis

    hongwei@840-g5:~$ whatis whereis
    whereis (1)          - locate the binary, source, and manual page files for a command

**whereis** finds the binary, the source, and the man page files for a program.  
**whereis** locates the binary, source and manual files for the specified command names.  
**whereis** attempts to locate the desired program in the standard Linux places, and in the places specified by **$PATH** and **$MANPATH**.  

    hongwei@840-g5:~$ whereis psql
    psql: /usr/bin/psql /usr/share/man/man1/psql.1.gz
    hongwei@840-g5:~$ whereis postgresql
    postgresql: /usr/lib/postgresql /etc/postgresql /usr/share/postgresql

## Links

[What is the difference between locate/whereis/which](https://askubuntu.com/questions/799776/what-is-the-difference-between-locate-whereis-which)  

<br>

#### Did you find this page helpful? Consider sharing it 🙌
