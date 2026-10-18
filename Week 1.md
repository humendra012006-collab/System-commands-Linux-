# Basic Command for Linux navigation
Detailed discription will be mentioned later.
- Print on screen `echo`
- Comments in bash `#`
- Current working directory `pwd`
- Change the directory `cd`
- Listing Files and Directories `ls`
- Create file `touch`
- Open a file `cat`
- Move or Rename file `mv`
- Remove file or directory `rm`
- Creat Directory `mkdir`
- Remove Directory (empty directory) `rmdir`
- Copy file or directory `cp`
- Help and documentation `man`, `info`, `help`
- Clera and exit terminal `clera` / Ctrl+l & `exit` / Ctrl+d

# Linux File System Hierarchy Standard
File system: traversing the tree, `/` is the root of the file system & `/` is also the delimiter for sub-directories.
- `.` is current directory
- `..` is parent directory
- Path for traversal can be absolute (`/home/Desktop`) or relative (`../parentFile`).
```
/            → Root folder
├── bin      → Essential user commands (ls, cp, cat)
├── boot     → Bootloader files, kernel, initramfs
├── dev      → Device files (disks, terminals, /dev/null)
├── etc      → System-wide configuration files
├── home/     → Users' personal directories
│   |── user1
|   └── user2
├── lib      → Essential shared libraries and kernel modules
├── media    → Mount point for removable media (USB, CD)
├── mnt      → Temporary mount point for filesystems
├── opt      → Optional third-party software
├── proc     → Virtual FS: process and kernel information
├── root     → Home directory of the root user
├── run      → Runtime data since last boot (PIDs, sockets)
├── sbin     → System administration binaries (fdisk, reboot)
├── srv      → Data served by the system (web, FTP)
├── sys      → Virtual FS: devices and kernel/driver info
├── tmp      → Temporary files (may be cleared on reboot)
├── usr/      → Secondary hierarchy: user programs and data
│   ├── bin  → Most user commands
│   ├── lib  → Libraries for /usr/bin and /usr/sbin
│   ├── local → Locally installed software
│   ├── sbin → Non-essential admin binaries
│   └── share → Architecture-independent data (docs, man pages)
└── var/      → Variable data
    ├── log  → System and application logs
    ├── cache → Application cache data
    ├── spool → Queued tasks (print, mail)
    └── tmp  → Temp files preserved between reboots
```
## More on /sys & /proc
- `/sys` is a virtual filesystem ( pseudo-filesystem / not stored on disk) that exposes clean, hierarchical view of kernel data structures. Linux kernel objects—such as devices, drivers, and modules—through a structured, in‑memory directory tree, allowing users to read and sometimes modify kernel attributes in real time. It is automatically mounted at `/sys` and provides organized access to hardware and kernel subsystem information (introduced in Linux 2.6 to replace and organize hardware-related information previously mixed into `/proc`). Major Directories Under `/sys`
    - `/sys/block` — block devices (HDDs, SSDs, NVMe) 
    - `/sys/bus` — hardware buses (PCI, USB, I²C) with device/driver subdirs 
    - `/sys/class` — device classes (net, input, graphics, power_supply) 
    - `/sys/devices` — full hardware device tree as seen by the kernel 
    - `/sys/dev` — device nodes grouped by major/minor numbers 
    - `/sys/module` — loaded kernel modules and parameters 
    - `/sys/power` — system power management controls

- `/proc` is a virtual filesystem (pseudo-filesystem / does not store data on disk) in Linux that provides a mechanism to access kernel data structures and runtime system information through a hierarchical structure of files and directories. Dynamically created during system boot and dissolved at shutdown, it acts as a process information and control center, exposing active process details, hardware status, and kernel parameters in real time for utilities (such as `ps`, `top`, and `free`) and system administrators.Major Directories and Files Under `/proc`

    - `/proc/[PID]` — process-specific directories (e.g., `/proc/1234`) containing runtime stats, memory mappings, open file descriptors, and environment variables for process ID `PID`
    - `/proc/cpuinfo` — detailed CPU architecture, core counts, clock speeds, and hardware flags
    - `/proc/meminfo` — system memory usage summary, including RAM, swap space, buffers, and cache
    - `/proc/sys` — configurable kernel parameters (e.g., networking, virtual memory management, filesystem behavior) accessible via `sysctl`
    - `/proc/version` — kernel version, compiler info, and build timestamp
    - `/proc/filesystems` — filesystems currently supported by the running kernel
    - `/proc/mounts` — symlink or listing of all currently mounted filesystems across the host

# Linux Commands
Linux commands are powerful tools that allow users to interact with the operating system through a command-line interface. These commands can perform a wide range of tasks, from basic file operations to advanced system management.
- **Flags** and **arguments** in Linux commands
    - `Flags` (or options) modify the behavior of a command. They usually start with a hyphen (-) and can be either single (-l) or double (--long) (Example : `ls -l`, here `-l` is a flag or option).
        - `Flags` can be grouped like (`ls -laih`)
    - `Arguments` are inputs that a command requires to execute (usually file or directory names or path). They usually follow the command and flags (Example : `man ls`, here `ls` is argument).
- A command line generally looks like:
```bash
user@host:~$ command  [options]  [arguments]
user@host:~$ ls -a
user@host:~$ man ls # Here man is Command $ ls is Argument
```
- A topical view of terminat terminoligy (`user@host:~$ ls -a`)
    - `user@host:~$` : Command Prompt
    - `user` : User Name
    - `host` : Host Name
    - `:~$` : Current Path (like :~/Desktop)
    - `ls`  : Command
    - `-a` : Flag/Option

## Help on a CLI (Command Line Interface)
1. Using `--help` Flag: This will display a summary of how to use the command.
```bash
ls --help
```
2. Using `man` Pages: This opens the manual page for the command, which provides detailed documentation (using `-k` similar as `apropos` command).
    - We can also use man with man page section (optional).
    ```bash
    man <section number> <argument>
    ```
    | Section | Type of pages |
    | --- | --- |
    | 1 | Executable programs or shell commands
    | 2 | System calls provided by kernel
    | 3 | Library calls
    | 4 | Special files usually found in /dev
    | 5 | File formats and conventions
    | 6 | Games
    | 7 | Miscellaneous: macro packages, conventions
    | 8 | System administration commands
    | 9 | Kernel routines
    ```bash
    # Example
    man ls
    man 1 ls # man with man page section
    man -k [keyword] # equivalent to `apropos [keyword]`, list all commands whose manual page contain given keyword
    ```
3. Using `info` Pages : Info pages offer a more structured and detailed form of documentation compared to man pages. The man command was initially the manual pages, as the complexity increased over time. There was a need of better command for help. Therefore, info command was introduced.
    ```bash
    info ls
    ```
4. Using `whatis` : Display one-line manual page discription (show first line of `man` command only, Example : `whatis ls` output : "list directory content")
    - `man -f [keyword]` equivalent to `whatis [keyword]` 

## List Directory Content `ls`
The ls command in Linux lists files and directories, and with the right options, you can get a summary showing permissions, ownership, size, and modification time in a human-readable format.
- `-l` Long format listing.
- `-a` List all files, including hidden files (those starting with a dot .).
- `-h` Human-readable file sizes (e.g., KB, MB).
- `-R` Recursively list subdirectories.
- `-t` Sort by modification time.
- `-i` Prints inode number.
- `-d` List directory themselves not there content.
- `-s` Lists files with their sizes in **blocks** before the file names.
```bash
user@host:~$ ls -la # all files in long list format.
user@host:~$ ls -lhi # each file in long list formate with inode no. and size in human-readable formate
user@host:~$ ls -lR # recursively list all files and subdirectories in long list formate
user@host:~$ ls /Desktop # print files and directorys inside Desktop directory
user@host:~$ ls -ld /Desktop/temp # print the details of temp directory(it self) only

```
**Typical output of `ls -l`**
```bash
user@host:~$ ls -l
# output
drwxr-xr-x 5 human human1 12288 Nov 25 10:00 Documents
```
- `drwxr-xr-x` File Permissions
- `5` Number of Hard links
- `human` Owner
- `human1` Group
- `12288` Size
- `Nov 25 10:00` Last modified timestamp
- `Documents` File Name
## File Permissions
In Linux, file permissions determine who can read, write, or execute a file. Permissions are represented by a string of 10 characters (use `file *` to know about files and directorys in pwd).
- Permission Symbols:
    - `r` - Read
    - `w` - Write
    - `x` - Execute
```
drwxr-xr-x
```
- The first character represents the file type (`-` for regular files, `d` for directories, and more ).
    - **`-`** Regular file (text, md, pdf, ect)
    - **`d`** Directory
    - `l` Symbolic link
    - `c` Character file
    - `b` Block file
    - `s` Socket file
    - `p` Named pipe
- The next three characters are permissions for the **owner** of the file (`rwx`).
- The following three characters are permissions for the **group** (`r-x`).
- The last three characters are permissions for **others** (`r-x`).\

### Changing File Permissions `chmod`
The chmod command in Linux is used to change the file permissions of a file or directory. Permissions determine who can read, write, or execute a file. Permissions are typically represented in two ways: symbolic notation (like rwx) and octal notation.\
**Syntax:**
```
chmod [permissions] [file]
```
#### Octal Numbers for Permissions 
Permissions in Linux are defined using three categories: ( User/Owner, Group, Others). Each category can have the following permissions:
    - Read(**r**) : 4
    - Write(**w**) : 2
    - Execute(**x**) : 1
- To set permissions using octal numbers, you add the values for the desired permissions. The octal representation for the permissions is a three-digit number like :
    - No permission: 0
    - Write only: 2 (w)
    - Write and execute: 3 (wx)
    - Read and execute: 5 (rx)
    - Read and write: 6 (rw)
    - Read, write, and execute: 7 (rwx)
- The first digit is for the user, the second digit is for the group, the third digit is for others.
    - Examples : (644: `rw-r--r--`, 700: `rwx------`, 755: `rwxr-xr-x`)
    ```bash
    touch text1.txt # creating file
    chmod 755 text1.txt
    ```
#### Symbolic Notation for Permissions
We can also use (`+`, `-`, `=`) for setting permissions for three categories:
- `u` :  The file **owner**.
- `g` :  The users who are members of the **group**.
- `o` :  All **other** users.
- `a` :  All users, identical to **ugo** (either use `a` or `ugo`).
- With following permissions:
    - `r` Read
    - `w` Write
    - `x` Execute
- Example :
    ```bash
    touch text1.txt # creating file
    chmod u+x text1.txt # add execution permission for user/owner
    chmod o-w text1.txt # remove write permission from other
    chmod g=rx text1.txt # set permission to read and execute only for group (r-x)
    chmod go=r text1.txt # set premission to read only for group and other (both r--)
    chmod a+w text1.txt # add write permission to all same as (ugo+r)
    ```
## File Ownership `chown`
The `chown` command in Linux allows modifying the ownership of files or directories. It can assign a new user, group, or both, either individually or simultaneously. only root or privileged user (via sudo) can change ownership.
**Syntax:**
```
chown new_owner[:new_group] file(s)
```
Understanding User Ownership and Permissions in Linux : Different users in the operating system have ownership and permission to ensure that the files are secure and put restrictions on who can modify the contents of the files. In Linux, different users use the system
- **Root User**: It is a superuser who has access to all the directories and files in our system, and it can perform any operation. An important thing to note is that only the root user can perform changing of permissions or ownership of the files that are not owned by them.
- **Regular User**: These users have limited access to files and directories and can only modify the files that they own.
    - To perform administrative tasks as a regular user, you typically use the `sudo` command, which stands for "superuser do." This command allows you to execute commands with root privileges without needing to switch to the root user. Using sudo helps maintain security by granting temporary elevated privileges, thus minimizing the risk of accidental system changes.
### Using `sudo` for Administrative Privileges
To perform administrative tasks as a regular user, you typically use the `sudo` command, which stands for "superuser do." This command allows you to execute commands with root privileges without needing to switch to the root user. Using sudo helps maintain security by granting temporary elevated privileges, thus minimizing the risk of accidental system changes.\

**Examples of Using sudo**
1. Changing File Ownership: If you want to change the ownership of a file called `important.txt` to a user named `alice`, you would run:
- This command changes the owner of `important.txt` to `alice`, allowing her to modify it as needed.
```bash
sudo chown alice important.txt
```
2. Changing File Permissions: To make a script executable for everyone, you could use:
- This grants the user read, write, and execute permissions, while the group and others receive read and execute permissions.
```bash
sudo chmod 755 myscript.sh
```
3. Installing Software: When installing software using a package manager like `apt`, you need administrative privileges:
- This command installs the Nginx web server on the system.
```bash
sudo apt install nginx
```
4. Editing System Configuration Files: To edit a configuration file in /etc, such as hosts, you can use:
- This opens the `hosts` file in the nano text editor with elevated permissions, allowing you to make necessary changes.
```bash
sudo nano /etc/hosts
```
5. Creating Directories in System Locations: To create a directory in `/usr/local`, which usually requires administrative privileges:
- This command creates a new directory for your application in a system-wide location.
```bash
sudo mkdir /usr/local/myapp
```
Using `sudo` effectively is crucial for managing a Linux system. It allows you to perform administrative tasks while maintaining system security. Always use `sudo` command with caution, as executing commands with elevated privileges can lead to significant changes that may affect system stability.

## Creating File, Deletion of File & Read Files
### Creating File
We can create file of any type in Linux with different commands like `touch`, `cat`, `echo` or `nano` also `>`.
1. Using `touch` command : `touch` command in Linux is used for changing file timestamps however one of the most common usages of touch command includes creating a new empty file. With the touch command, you can change access, modify and change time of files and folders in Linux. You can update the timestamps or modify them to a date in the past.
```bash
touch file1.txt # This creates a empty file
touch file2.txt file3.txt # This creates multiple files
touch Desktop # This will change the time stamp with current time (makes it looks newly modified)
```
- Few flags :
    - `-a` Updates only the access time of a file with current time (when the file was last opened or read.).
    - `-m` Updates only the modification time with current time (file content was last modified).
    - `-c` Prevents the touch command from creating a new file if it doesn’t already exist (if used then file will not created).
2. Using `cat` : Like a CLI text editor, also can read using `cat file.txt` this will read file and display it.
```bash
cat > file2.md # Press enter and write content of file, to exit press Ctrl+d
```
3. Using `echo` : The echo command in Bash is used to display text, variables, or messages to the terminal,can be used for creating file with small content in it (It can be also use to print the current session user name `echo $USER`).
```bash
echo "Hello world" > file1.txt # Overwrite content if exist, created otherwise
echo "Hello world2" >> file1.txt # Add content if exist, created otherwise
echo -e "Hello world\nThis is second line." > file2.txt
```
- `-e` : Enables Escape Sequences
4. Using `nano` (text) editor : This will open CLI editor
```bash
nano file.txt # create file and open editor
nano # open editor
```
5. Using `vi` OR `vim` editor, editor for programers, more about it in week 2.
```bash
vi test.cpp
```
6. Using `>` : This create empty file
```bash
user@host:~$ > file1.md
```
### Deleting File
1. Using `rm` : Remove file or Directory
```bash
rm file.txt # This will remove file.txt if exist
rm file1.md file2.md # can remove multiple files
```
2. Using `unlink` : Call the unlink function to remove the specified file.
```bash
unlink file.md
```
### Reading a Files
We can read file (of readable type) using different coommands
1. `cat <filename>` : read and display the entire content of a file on terminal, best for short files.
2. `nano <filename>` : read and display file in nano editor (open in terminal).
3. `more <filename>` : read and display file content on terminal page by page.
4. `less <filename>` : read and display file content on terminal page by page, similar as `more` with extra features (Jok in Linux 'less is more' as `less` occupy more space then `more` command).
5. `head` : Display the first part of a file or piped data directly in the terminal. By default, it shows the first 10 lines (use `-n` to specify no. of lines to display)
    ```bash
    head file1.txt # display first 10 lines of file1
    head -n 5 file1.txt # display first 5 lines of file1
    ```
6. `tail` : Display the last part of a file. By default, it shows the last 10 lines, making it especially useful for monitoring log files and tracking recent updates without opening the entire file (`-n` for specify no. of lines)
    ```bash
    tail file1.txt # display last 10 lines of file1
    tail -n 5 file1.txt # display last 5 lines of file1
    ```
## Directory Create, Delete and Change Directory 
- `pwd` : Prints the absolute path of your current working directory, starting from the root (/).
### Create Directory `mkdir`
The `mkdir` command is used to create new directories.
- `-p` Ensures all missing parent directories are created without errors(for nested creation of directorys).
- `-v` Prints a confirmation message for each directory created.
- `-m` Sets directory permissions immediately upon creation.
```bash
mkdir dir1 # This creates a directory named dir1
mkdir dir1 dir2 dir3 # Creates dir1, dir2, and dir3 at once
mkdir -p parent/child/grandchild # Missing directorys created without error
mkdir -v my_folder # Confirmation message for directory creation
mkdir -m 755 secure_dir # gives full access to the owner, read/execute to others
```
### Delete Directory `rmdir`, `rm`
- The `rmdir` command is used to remove **empty directories** only,  use `rm -r` if you need to remove non-empty directories.
    ```bash
    rmdir dir1 # Remove a single empty directory
    rmdir dir1 dir2 dir3 # Remove multiple empty directories
    ```
- The `rm` command in Linux is used to permanently delete files and directories from the filesystem. Unlike moving files to a "Recycle Bin," `rm` removes them immediately, making recovery difficult without special tools.
    - `-i` Interactive mode; prompts before each deletion.
    - `-f` Force delete without prompts, even for write-protected files.
    - `-R` Recursive delete; removes directories and their contents.
    - `-v` Verbose mode; shows each file being deleted.
    ```bash
    rm -r dir1 # Remove directory and containt of it
    ````
### Change Current Directory `cd`
`cd` command is use to change directory, allows navigation in file system.
```bash
user@host:~$ cd [directory]
```
- `~` : home directory (/home/username).
```bash
user@host:~/Desktop/temp$ cd ~ # Return to user@host:~$
user@host:~/Desktop/temp$ cd # same as cd ~ , back to home
```
- `-` : switch to privous directory
```bash
user@host:~/Desktop/temp/first.txt$ cd
user@host:~$ cd -
user@host:~/Desktop/temp/first.txt$
```
- `..` : refers to the parent directory.
```bash
user@host:~/Desktop/temp$ cd ..
user@host:~/Desktop$ pwd # output : /home/username/Desktop
```
- `/` : Root folder
```bash
user@host:~$ cd /
user@host:/$ pwd # output : /
```
## Inode Number of file
An inode is a data structure on a filesystem that stores information about a file or directory (except its name or data).vInode Information Includes:
- File Type
- Permissions
- Owner
- Group
- Size
- Timestamps
- Location of Data blocks
## Hard Link & Soft Link
A link in UNIX is a pointer to a file. Like pointers in any programming languages, links in UNIX are pointers pointing to a file or a directory. Creating links is a kind of shortcuts to access a file. Links allow more than one file name to refer to the same file, elsewhere. 
- There are two types of links :
    - Soft Link or Symbolic links
    - Hard Links
### Hard Link
A hard link is a direct directory entry pointing to the exact same inode (data block index) on the storage drive as the original file.
```bash
ln target_file hardlink_name
```
- Inode Sharing: Each hard-linked file is assigned the same inode value as the original, meaning they reference the exact same physical file location on disk.
- Link Count: Increments the file's hard link count (visible in ls -l). The underlying data blocks are only freed from storage when the link count reaches zero (all hard links are deleted).
- Location Flexibility: Hard links remain fully functional even if the original file or any of the linked files are moved to different directories within the file system, because the inode reference remains unchanged.
- Content Synchronization: Changes made to content via a hard link immediately reflect across all other hard links to that file.
- Limitations:
    - Cannot cross different file systems or disk partitions.
    - Cannot link to directories (to prevent recursive loops in file tree traversal).
### Soft Link
A soft link (or symbolic link) is a special file that acts similarly to a shortcut feature in graphical operating systems, pointing to another file or directory (use `-s` with `ln` to create soft link).
```bash
ln -s target_file softlink_name
```
- Separate Inode: Each soft-linked file contains its own separate inode value that stores the file path string pointing to the original target.
- Link Count: Does not increase the hard link count of the target file.
- Target Vulnerability (Dangling Link): If the original file is deleted, moved, or renamed, the soft link breaks and will not work correctly (often referred to as a broken, dangling, or hanging link).
- Cross-Filesystem Compatibility: Soft links can seamlessly link across different file systems and disk partitions.
- Directory Support: Unlike hard links, soft links can freely point to both files and directories.
- Content Synchronization: Like hard links, any changes made to the data through either the link or the original file are reflected in both, as long as the target path remains valid.
## Role of block size (lecture 1.4)
In Linux, block size is the smallest unit of disk space that a filesystem can allocate to store data. It plays a critical role in system performance, storage efficiency, and resource utilization.
1. Disk Space Allocation & Storage Efficiency
- Internal Fragmentation: Filesystems allocate disk space in whole blocks. If a block size is 4 KB and a file is 1 KB, the file still occupies a full 4 KB (4096 bytes) block, leaving 3 KB wasted (slack space). Conversely, if a file size exceeds 4096 bytes—even by just 1 byte (4097 bytes)—the filesystem must allocate 2 full blocks (8 KB / 8192 bytes), leaving nearly 4 KB wasted in that second block.
2. Inode Management, Allocation Memory Page Alignment
In Linux filesystems like ext4, block size dictates the minimum allocation boundary.

- The default block size (typically 4096 bytes / 4 KB on x86 architectures) directly limits max file size and max filesystem size. For example, in ext4, a 4 KB block size allows maximum single file sizes up to 16 TB.
- Linux manages RAM in pages (standard size is 4 KB on x86_64). Having a filesystem block size equal to the architecture's memory page size (4 KB) optimizes memory mapping (page cache) and prevents additional translation overhead during page faults and buffer cache management.

## Some More Commands
- `ps` : used to display information about currently running processes on a system. It provides details such as process IDs (PIDs), CPU and memory usage, and the commands that initiated the processes (Use `-A` or `-e` for all process details).

- `uname` : Print system information, stands for "Unix Name," and it is designed to give you key information about your Linux machine, such as the kernel version, operating system, hardware architecture, and more (Use `-a` for all system information). 

- `date` : Print or set the system date and time (with flag `-R` it prints date and time in RFC 5322 formate / standard for email communication).

- `cal` : Display a calander and the date of Easter.
    ```bash
    cal # Display calander of current month
    ncal # Display calander of current month (different formate)
    cal [month] [year] # Display calander of specified month & year (month: nov or 11)
    ncal [month] [year]
    ```

- `free` : Display the amount of free and used memory in the system (with `-h` show in human readable formate KB, MB, Gb).

- `df` : The `df` (disk free) used to display the amount of disk space available and used on mounted file systems. It’s a quick way to check storage usage, capacity, and mount points. By default, `df` reports sizes in 1K blocks and shows all currently mounted file systems. It cannot display information for unmounted file systems.

- `groups` : Display Group Membership of Current User
    ```bash
    groups # display the groups a current user in 
    groups [username] # display the groups in which specified user in 
    cat /etc/group # all the users
    ```

- `cp` : The cp command in Linux is used to copy files and directories from one location to another. By default, it overwrites existing files without prompting (with `-R` copy directorys recursively).
    ```bash
    cp [options] [source] [destination]
    cp file1.txt file2.txt # Creates file2.txt with the same content as file1.txt. If file2.txt exists, it is overwritten.
    cp file1.txt file2.txt /path/to/dir/ # copy both files to destination

    ```

- `mv` : Move or Rename files and Directorys.
    ```bash
    mv [options] [source] [destination]
    mv myfile.txt /path/to/destination/ # Moving a file to a different directory
    mv file1.txt file2.txt /path/to/destination/ # moving multiple files
    mv oldfile.txt newfile.txt # rename the file.
    mv sourcedir /path/to/destination/ # move a directory
    mv olddir newdir # rename directory
    ```

- `rm` : Used to permanently remove files or directories from the filesystem. Deleted items are not sent to a recycle bin, so recovery is generally not possible without specialized tools (flags like `-i` prompts before each deletion, `-f` Force delete without prompts, even for write-protected files, `-R` Recursive delete; removes directories and their contents, `-v` shows each file being deleted).
    ```bash
    rm [OPTIONS] FILE
    rm file.txt # remove file
    rm -i file.txt # prompts before deletion
    rm -R mydir/ # removes directories and their contents
    ```
- `alias` : An alias in Linux is a shortcut for a longer command or sequence of commands, making the terminal faster and easier to use. You can create them temporarily (upto terminal is not closed) for the current session or permanently so they persist across sessions.
    ```bash
    alias rm="rm -i" # now rm prompts before each deletion
    alias ll="ls -l" # list directorys in long formate by just writing ll
    ```
- `unalias` : Removes one or more defined shell aliases, either individually or all at once, from the current shell session (cannot remove built-in shell commands; it only affects user-defined aliases).

- `whoami` : Prints the effective username of the current user executing the command. It is a quick way to verify the identity of the user in the current shell session, especially after switching users with su or similar commands (also may use `echo $USER`).

- `who` : Displays information about all users currently logged into the system. It provides details such as the username, terminal, login time, and the source of the login (e.g., IP address or hostname). It is useful for understanding the global state of user sessions on the system.

- `file` : Display file type (like `file filename` tell about file) and with `file *` display 'fileName : file type' of pwd.

- `stat` : Display detailed information about a file or directory — including size, permissions, ownership, timestamps, and inode details.

- `du` : In Linux, `du` (disk usage) estimates how much disk space files and directories use, with common options like `-h` for human‑readable sizes, `-s` for totals and `-a`for Includeing files as well as directories. If no path is given, it analyzes the current directory.

- `wc` : Display number of lines, no. of characters and size of file (`-l` display only no. of lines in a file)
    ```bash
    wc file1.md # display no. of lines and characters, size
    wc -l file1.md # display no. of lines only
    ```

- `which` : Print the location of comand specified (Example : `which mkdir`, output : `/usr/bin/mkdir`, even all commands are here in `/usr/bin`)

- `type` : The `type` command in Linux is a useful utility for identifying how the shell will interpret a given command. It provides information on whether a command is a shell built-in, external binary, function, or alias, helping users understand the source of the command and its behavior. The command is particularly helpful when troubleshooting or exploring system utilities, as it reveals whether a command is being executed as an alias, keyword, or a binary file from the disk (Example : `type <command_name>`).

- `readlink` : Print resolved symbolic links or canonical file names, use `-f` for recursive depth (Example: readlink -f /usr/bin/pico output `/usr/bin/nano`).

- `greap` : Powerful command-line utility in Unix/Linux used to search for patterns (plain text or regular expressions) within files or streams. It outputs the lines that match the given pattern, making it invaluable for developers when searching through code, logs, or configuration files (More on Weeks 4 when study `RE`).

- `tar` : The command bundles multiple files or directories into an archive without
compression. To create a new TAR file, you must add the `-c` option. Then, use the `-f` flag to specify the archive’s name. Regardless of the extension, you can unpack a TAR file.
    ```bash
    tar [options] tar_file_name file1 dir1/ # Creat new tar file
    tar [options] tar_file_name # Unpack tar file
    tar -cf my_tar.tar first_project (make tar archive)
    tar -xf my_tar.tar (untar/unpack the tar file)
    ```

- `zip` : The command compresses one or multiple files into a ZIP archive,
reducing their size. use `-r` for recursive . To unzip the zipped file, use: `unzip` also `-d` can be used to specify the path where to unzip.
    ```bash
    zip [options] [zip_file_name.zip] file1 dir1 # Creat a zip file 
    unzip [options] [zip_file_name.zip]
    zip -r project.zip /path/to/project # Creat zip file of entire directory structure of project
    unzip project.zip -d ~/dir3/copy_project # unzip at ~/dir3/copy_project
    ```
- `diff` : A powerful utility used to compare the contents of two files line by line
 
## How to Find New Commands
One need not to search on web to learn commands of Linux, Linux it's self provide some commands to explore more commands by writing relevent word to find required commands.
- `apropos` : Searching for keywords from the manual pages (actually `man -k` similar as `apropos`)
```bash
man -k [keyword] # print all commands whose manual page contain given key word
apropos [keyword] # print all commands whose manual page contain given key word
```
- `info` : Display all type of commands (like a browser), use Arow key to go up-down press `ENTER` to open any command discription, can go back using `<` (i.e. Shift,)
- `help` : Help without any parameter lists all available built-in commands (reserved for the shell which we are runing)

### Search more for
- The `scp` (Secure Copy Protocol) command is a Linux utility used to securely transfer files and directories between local and remote systems over a network. It operates over SSH (Secure Shell), ensuring that both the data and authentication credentials are encrypted during transfer, protecting against interception.

- The `dmesg` command, short for display message, is a Linux utility used to examine or control the kernel ring buffer, which stores messages generated by the kernel during system boot and runtime. These messages provide critical insights into hardware detection, device initialization, and kernel interactions, making dmesg an essential tool for troubleshooting and monitoring.