# Combining commands and files
## Executing multiple commands
Command could be a script or normal command
- Run multiple commands 
```bash
command1; command2; command3;
# Each command will be executed one after other
date; ls /usr/bin;
```
- Run another command if first runs sucessfully (returns 0 - success)
```bash
command1 && command2
# command2 will be executed only if command1 succeeds
sudo apt install vim && man vim
```
- Run either a command to do the task
```bash
command1 || command2
# command2 will not be executed if command1 succeeds
apt install vim || sudo apt install vim
```
## File descriptors
A file descriptor (FD) is a non-negative integer that the operating system assigns to a process when it opens a file, socket, pipe, or any I/O resource. It acts as a handle for the process to perform operations like read, write, or close without knowing the underlying device details.
- As shown in the blow picture each `stdin`, `stdout` and `stderr` are usually correspond to keyboard or moniter but we can deliver the content of `stdout` and `stderr` to some file or to other command (pipe) or take input `stdin` from file or output of some other cammands.
- Rember : Use `>` to write/overrite file and use `>>` for appending to file.
<div style="text-align: left;">
<img src="./images/file_des0.png" alt="Editor" width="700" >
</div>

1. **`command > file1`** : Using `>` we can transfer the output of command to file1, override the fiel1 if exist already.

    ```bash
    # Example
    ls /usr/bin > list_file.txt
    ```
    <div style="text-align: left;">
    <img src="./images/file_des1.png" alt="Editor" width="600" >
    </div>

2. **`command >> file1`** : Using `>>` we can append the output of to fiel1, it will not override if file already exist.
    ```bash
    # Example
    ls -l /etc/bin >> list_file.txt
    ```
    <div style="text-align: left;">
    <img src="./images/file_des2.png" alt="Editor" width="600" >
    </div>

3. **`command 2> file1`** : Using `2>` redirecting error to file1, again this will override the file1 if exists already, we can use `2>>` to appent to the file1.
    ```bash
    # Example
    ls -l /ETC/BIN 2> list_error.txt
    ```
    <div style="text-align: left;">
    <img src="./images/file_des3.png" alt="Editor" width="600" >
    </div>

4. **`command < file`** : Use `<` to take standard input of a command from a file
    ```bash
    # Example
    wc < list_error.txt
    ```
    <div style="text-align: left;">
    <img src="./images/file_des4.png" alt="Editor" width="600" >
    </div>


5. **`command > file1 2> fiel2 `** : Using `>` to redirect output to file1, and `2>` to redirect error to file2.
    ```bash
    # Example
    ls -R $HOME /baba > list_file_rec.txt 2> error_file.txt
    ```
    <div style="text-align: left;">
    <img src="./images/file_des5.png" alt="Editor" width="600" >
    </div>

6. **`command > file1 2>&1`** : Using `2>&1` we redirect error to standard out put which is using `>` is redirected to file1, In short use this whenever we need store both error and output.
    ```bash
    # Example
    ls -R $HOME /baba > output_file.txt 2>&1
    ```
    <div style="text-align: left;">
    <img src="./images/file_des6.png" alt="Editor" width="600" >
    </div>

7. **`command1 | command2`** : Using `|` **(Pipe)**, The pipe takes the standard output of the command on the left and sends it as the standard input (stdin) to the command on the right.
    ```bash
    # Example
    cat list_error.txt | grep "FATAL" 
    ps aux | grep node # lists all running processes on the system, and filters the list to show only the rows related to "node".
    ```
    <div style="text-align: left;">
    <img src="./images/file_des7.png" alt="Editor" width="650" >
    </div>

8. **`command1 | command2 > file1`** : Similarly we can use all above like `2>`, `>>`, `command1 < file0 | command2 > file1 2>&1`, etc. 
    ```bash
    # Example
    ps aux | grep node > process_info.txt
    ```
    <div style="text-align: left;">
    <img src="./images/file_des8.png" alt="Editor" width="650" >
    </div>

## `tee` Command
The tee command in Linux is used to read from standard input and write the output simultaneously to both standard output (the terminal) and one or more files. It is particularly useful when you want to view the output of a command while also saving it to a file.
```bash
# Syntax
command | tee [OPTIONS] [FILE...]
# Example
ls -l /usr/bin > list_file.txt
ls -l /usr/bin > file1 file2 # both files have same content in it
```
<div style="text-align: left;">
<img src="./images/tee.png" alt="Editor" width="700" >
</div>

## Special directory `/dev/null`
The `/dev/null` is a special file in Linux, often referred to as a **black hole**. Any data written to it is discarded, making it useful for suppressing unwanted output or errors.
<div style="text-align: left;">
<img src="./images/dev_null.png" alt="Editor" width="700" >
</div>

### How It Works:
- Writing to it (Discarding Data): Anything you send or redirect into `/dev/null` is instantly discarded and disappears forever. It consumes the data without saving it or using up disk space.
```bash
# Syntax
command 2> /dev/null 
command > /dev/null
# Example
ls $HOME /baba 2> /dev/null # only list directorys inside HOME do not show Error
ls $HOME /baba > /dev/null # only show the Error '/baba does not exist'
```
- Reading from it (Providing Nothing): If you try to read from `/dev/null`, it immediately returns an End-Of-File (EOF) marker, meaning it always looks completely empty.
    - Use Cases :
    1. Silent Execution: Run commands without displaying output `(> /dev/null)` or errors `(2> /dev/null)`.
    2. Log Cleaning: Clear logs without deleting files.
    3. Secure Deletion: Overwrite sensitive files with null bytes.
```bash
# Syntax
cat /dev/null > file
# Example
cat /dev/null > /var/log/syslog # clear logs
cat /dev/null > sensitive_file # delete its content
```

# Software Management

