# Linux Commands

### alias

The ```alias``` command lets you give your own name to a command or sequence of commands. You can then type your short name, and the shell will execute the command or sequence of commands for you.

```bash
$ alias cls=clear
$ cls
```

### cat

The ```cat``` command (short for “concatenate”) lists the contents of files to the terminal window. This is faster than opening the file in an editor, and there’s no chance you can accidentally alter the file.

```bash
$ cat .bash_logout
```

### chmod

The ```chmod``` command sets the file permissions flags on a file or folder. The flags define who can read, write to or execute the file.

- 0: No permission
- 1: Execute permission
- 2: Write permission
- 3: Write and execute permissions
- 4: Read permission
- 5: Read and execute permissions
- 6: Read and write permissions
- 7: Read, write and execute permissions

To set the permission to be read, write, and execute (7 from our list) for the owner; read and write (6 from our list) for the group; and read and execute (5 from our list) for the others we’d need to use the digits 765 with the ```chmod``` command:

```bash
$ chmod -R 765 example.txt
```

### chown

The ```chown``` command allows you to change the owner and group owner of a file. Listing our example.txt file with ```ls -l``` we can see *dave dave* in the file description. The first of these indicates the name of the file owner, which in this case is the user *dave*. The second entry shows that the name of the group owner is also *dave*.  Each user has a default group created when the user is created. That user is the only member of that group. This shows that the file is not shared with any other groups of users.

```bash
$ sudo chown dave:mary example.txt
```

### df

The ```df``` command shows the size, used space, and available space on the mounted filesystems of your computer.

Two of the most useful options are the ```-h``` (human readable) and ```-x``` (exclude) options. The human-readable option displays the sizes in Mb or Gb instead of in bytes. The exclude option allows you to tell ```df``` to discount filesystems you are not interested in. For example, the ```squashfs``` pseudo-filesystems that are created when you install an application with the ```snap``` command.

```bash
$ df -h -x squashfs
```

### find

Use the ```find``` command to track down files that you know exist if you can’t remember where you put them. You must tell ```find``` where to start searching from and what it is looking for. In this example, the ```.``` matches the current folder and the ```-name``` option tells ```find``` to look for files with a name that matches the search pattern.

You can use wildcards, where ```*``` represents any sequence of characters and ```?``` represents any single character. We’re using ```*ones*``` to match any file name containing the sequence “ones.” This would match words like bones, stones, and lonesome.

```bash
$ find . -name *ones*
```

We can tell ```find``` to restrict the search to files only. We do this using the ```-type``` option with the ```f``` parameter. The ```f``` parameter stands for files.

```bash
$ find . -type f -name *ones*
```

If you want the search to be case insensitive use the ```-iname``` (insensitive name) option.

```bash
$ find . -iname *wild*
```

### free

The ```free``` command gives you a summary of the memory usage with your computer. It does this for both the main Random Access Memory (RAM) and swap memory. The ```-h``` (human) option is used to provide human-friendly numbers and units. Without this option, the figures are presented in bytes.

```bash
$ free -h
```

### grep

The ```grep``` utility searches for lines which contain a search pattern. When we looked at the alias command, we used ```grep``` to search through the output of another program, ```ps``` . The ```grep``` command can also search the contents of files. Here we’re searching for the word “train” in all text files in the current directory.

```bash
$ grep train *.txt
```

### gzip

The ```gzip``` command compresses files. By default, it removes the original file and leaves you with the compressed version. To retain both the original and the compressed version, use the ```-k``` (keep) option.

```bash
$ gzip -k core.c
```

### head

The ```head``` command gives you a listing of the first 10 lines of a file. If you want to see fewer or more lines, use the ```-n``` (number) option. In this example, we use ```head``` with its default of 10 lines. We then repeat the command asking for only five lines.

```bash
$ head -core.c
```

```bash
$ head -n 5 core.c
```

### kill

The ```kill``` command allows you to terminate a process from the command line. You do this by providing the process ID (PID) of the process to ```kill```. Don’t kill processes willy-nilly. You need to have a good reason to do so. In this example, we’ll pretend the ```shutter``` program has locked up.

```bash
$ kill *id*
```

### ls

This might be the first command the majority of Linux users meet. It lists the files and folders in the directory you specify. By default, ```ls``` looks in the current directory. There are a great many options you can use with ```ls```, and we strongly advise reviewing its the man page. Some common examples are presented here.

To list the files and folders in the current directory:

```bash
$ ls
```

To list the files and folders in the current directory with a detailed listing use the ```-l``` (long) option:

```bash
$ ls -l
```

To use human-friendly file sizes include the ```-h``` (human) option:

```bash
$ ls -lh
```

To include hidden files use the ```-a``` (all files) option:

```bash
$ ls -lha
```

### man

The man command displays the “man pages” for a command in ```less```. The man pages are the user manual for that command. Because man uses less to display the man pages, you can use the search capabilities of less.

For example, to see the man pages for chown, use the following command:

```bash
$ man chown
```

### mkdir

The ```mkdir``` command allows you to create new directories in the filesystem. You must provide the name of the new directory to ```mkdir```. If the new directory is not going to be within the current directory, you must provide the path to the new directory.

To create two new directories in the current directory called “invoices” and “quotes,” use these two commands:
```bash
$ mkdir invoices
```

### cp

The ```cp``` command is just a short way of telling your machine to copy a file or directory from one folder to another. It is among other de-facto Linux commands you can’t live without. You can copy multiple files to a directory right from your terminal with this neat command.

```bash
$ cp arquivo1 copia
```

### mv

The ```mv``` command allows you to move files and directories from directory to directory. It also allows you to rename files.

To move a file you must tell ```mv``` where the file is and where you want it to be moved to. In this example, we’re moving a file called ```apache.pdf``` from the “~/Document/Ukulele” directory and placing it in the current directory, represented by the single ```.``` character.

```bash
$ mv ~/Documents/Ukulele/Apache.pdf .
```

To rename the file, you “move” it into a new file with the new name.

```bash
$ mv Apache.pdf The_Shadows_Apache.pdf
```

The file move and rename action could have been achieved in one step:

```bash
$ mv ~/Documents/Ukulele/Apache.pdf ./The_Shadows_Apache.pdf
```

### passwd

The ```passwd``` command lets you change the password for a user. Just type ```passwd``` to change your own password.

```bash
$ sudo passwd mary
```

### ping

The ```ping``` command lets you verify that you have network connectivity with another network device. It is commonly used to help troubleshoot networking issues. To use ```ping```, provide the IP address or machine name of the other device.

```bash
$ ping 192.168.4.18
```

### ps

The ```ps``` command lists running processes. Using ```ps``` without any options causes it to list the processes running in the current shell.

```bash
$ ps
```

### pwd

Nice and simple, the ```pwd``` command prints the working directory (the current directory) from the root / directory.

```bash
$ pwd
```

### tar

With the ```tar``` command, you can create an archive file (also called a tarball) that can contain many other files. This makes it much more convenient to distribute a collection of files. You can also use ```tar``` to extract the files from an archive file. It is common to ask ```tar``` to compress the archive. If you do not ask for compression, the archive file is created uncompressed.

They have used the ```-c``` (create) option and the ```-v``` (verbose) option. The verbose option gives some visual feedback by listing the files to the terminal window as they are added to the archive. The ```-f``` (filename) option is followed by the desired name of the archive. In this case, it is ```songs.tar```.

```bash
$ tar -cvf songs.tar Ukulele/
```

There are two ways to tell ```tar``` that you want the archive file to be compressed. The first is with the ```-z``` (gzip) option. This tells tar to use the ```gzip``` utility to compress the archive once it has been created.

It is usual to add “.gz” as suffix to this type of archive. That allows anyone who is extracting files from it to know which commands to pass to ```tar``` to correctly retrieve the files.

```bash
$ tar -cvzf songs.tar.gz Ukulele/
```

To create an archive file that is compressed using a superior compression algorithm giving a smaller archive file use the ```-j``` (bzip2) option.

```bash
$ tar -cvjf songs.tar.bz2 Ukulele/
```

If you are archiving a great many files, you must choose between the ```-z``` option for decent compression and reasonable speed, or the ```-j``` option for better compression and slower speed.

To extract files from an archive file use the ```-x``` (extract) option. The ```-v``` (verbose) and ```-f``` (filename) options behave as they do when creating archives. Use ```ls``` to confirm which type of archive you are going to extract the files from, then issue the following command.

```bash
$ ls
```

```bash
$ tar -xvf songs.tar
```

To extract files from a “.tar.gz” archive, use the ```-z``` (gzip) option.

```bash
$ tar -xvzf songs.tar.gz
```

Finally, to extract files from a “.tar.bz2” archive use the ```-j``` option instead of the ```-z``` (gzip) option.

```bash
$ tar -xvjf songs.tar.bz2
```

### top

The ```top``` command shows you a real-time display of the data relating to your Linux machine. The top of the screen is a status summary.

The first line shows you the time and how long your computer has been running for, how many users are logged into it, and what the load average has been over the past one, five, and fifteen minutes.

The second line shows the number of tasks and their states: running, stopped, sleeping and zombie.

The third line shows CPU information. Here’s what the fields mean:

- us: value is the CPU time the CPU spends executing processes for users, in “user space”
- sy: value is the CPU time spent on running system “kernel space” processes
- ni: value is the CPU time spent on executing processes with a manually set nice value
- id: is the amount of CPU idle time
- wa: value is the time the CPU spends waiting for I/O to complete
- hi: The CPU time spent servicing hardware interrupts
- si: The CPU time spent servicing software interrupts
- st: The CPU time lost due to running virtual machines (“steal time”)

The fourth line shows the total amount of physical memory, and how much is free, used and buffered or cached.

The fifth line shows the total amount of swap memory, and how much is free, used and available  (taking into account memory that is expected to be recoverable from caches).

The user has pressed the E key to change the display into more humanly digestible figures instead of long integers representing bytes.

The columns in the main display are made up of:

- PID: Process ID
- USER: Name of the owner of the process
- PR: Process priority
- NI: The nice value of the process
- VIRT: Virtual memory used by the process
- RES: Resident memory used by the process
- SHR: Shared memory used by the process
- S: Status of the process. See the list below of the values this field can take
- %CPU: the share of CPU time used by the process since last update
- %MEM: share of physical memory used
- TIME+: total CPU time used by the task in hundredths of a second
- COMMAND: command name or command line (name + options)

(The command column didn’t fit into the screenshot.)

The status of the process can be one of:

- D: Uninterruptible sleep
- R: Running
- S: Sleeping
- T: Traced (stopped)
- Z: Zombie

Press the Q key to exit from ```top```.

### uname

You can obtain some system information regarding the Linux computer you’re working on with the ```uname``` command.

- Use the ```-a``` (all) option to see everything.
- Use the ```-s``` (kernel name) option to see the type of kernel.
- Use the ```-r``` (kernel release) option to see the kernel release.
- Use the ```-v``` (kernel version) option to see the kernel version.

```bash
$ uname -a
```

```bash
$ uname -s
```

```bash
$ uname -r
```

```bash
$ uname -v
```

### w

The ```w``` command lists the currently logged in users.

```bash
$ w
```

### whoami

Use ```whoami``` to find out who you are logged in as or who is logged into an unmanned Linux terminal.

```bash
$ whoami
```

### vmstat (Virtual Memory Statistics)

Linux ```vmstat``` command used to display statistics of virtual memory, kernerl threads, disks, system processes, I/O blocks, interrupts, CPU activity and much more. By default vmstat command is not available under Linux systems you need to install a package called ```sysstat``` that includes a ```vmstat``` program. The common usage of command format is.

```bash
$ vmstat
```

### netstat (Network Statistics)

```netstat``` is a command line tool for monitoring incoming and outgoing network packets statistics as well as interface statistics. It is very useful tool for every system administrator to monitor network performance and troubleshoot network related problems.

```bash
$ netstat -a | more
```

### lsblk

Often you will find the need to list the available block devices of your Linux system. The lsblk is one of the most used Linux commands for this purpose. This handy terminal command will present you with a tree structure of your block devices and is used heavily by professional users.

```bash
$ lsblk
```

# Standard Linux Directories

| Path | Contents |
|----|----|
| /bin | essential programs (binaries)|
| /lib | essential application libraries|
| /etc | configuration files|
| /usr | “UNIX system resources”. Nonessential resources.|
| /tmp | temporary files, typically cleared at shutdown|
| /home | user home directories|
| /root | the root user’s home directory|
| /boot | boot files (optional)|
| /sbin | system administration programs|
| /mnt | temporary mount points|
| /var | contains “variable” data|
| /proc | process information|
| /dev | devices|
| /sys | info about devices|