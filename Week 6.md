# Overview of Shell Script
A shell script is essentially a text file containing a series of commands that the shell can interpret and execute.

Shell scripting in Linux is not limited to any specific language. The shell acts as an interpreter for these scripts, executing the commands defined within them. This flexibility allows for the creation of powerful and efficient automation tools that can handle complex tasks and workflows.

<div style="text-align: left;">
<img src="./images/script languages.png" alt="Editor" width="600" >
</div>

- We stick to only Bash scripts for which the interpreter mentioned as `#!/bin/bash`, And use `.sh` extension to indicate that a file is a shell script.
- **No Required Extension**: Linux and other UNIX-like systems do not require a file extension for shell scripts. The system determines how to execute a script based on the shebang line at the top of the file (e.g., `#!/bin/bash`) and the file's execute permissions, not the file name or extension
- Common Shell Types: `Bash` (Bourne-Again Shell): Default on many Linux systems, 
`sh` (Bourne Shell): Original Unix shell, `csh` (C Shell): C-like syntax, `ksh` (Korn Shell): Enhanced scripting features, `zsh`: Extended Bourne shell with advanced features

## Shell Script arguments
| Symbol | Purpose |
| --- | --- |
| `$0` | Name of the shell program (how script was invoked, meaning the name or path used to run it.) |
| `$#` | Number of arguments passed |
| `$1` OR `${1}` | first arguments (recommended using `${}`) |
| `${n}` | nth argument |
| `$*` OR `$@` | All arguments at once |
| `"$*"` | All argument as a single string |
| `"$@"` | All argument as a separate strings |
| `$?` | Last command's exit status |
| `$$` | Current shell PID |
| `$!` | Last background process PID |
| `$-` | Current shell flags |
| `$_` | Last argument of previous command |

### Command substitution
Initialize a variable with result of some command.
```bash
var=`command`
# OR
var=$(command)
```
## Input & Output in Schell Scripts
### Output
- `echo`: Terminates with a newline if `-n` option not given
```bash
echo My home is $HOME
```
- `printf`: supports format specifiers like in C
```bash
printf “My home is %s\n” $HOME
```
### Input
Different ways to read `stdin` inside script
1. `read` : Bash built‑in used to read a line of input (from keyboard or file descriptor) and assign it to one or more variables.
- Syntax :
```bash
read [options] [variables]
```
| Options | Description |
| --- | --- |
| `-p "prompt"` |  Display a prompt before reading input. |
| `-r` | 	Disable backslash escaping (recommended). |
| `-t seconds` |  Timeout if no input is received. |
| `-n N` |  Read only N characters. |
| `-s` |  Silent mode (useful for passwords). |
| `-d delim` |  Use a custom delimiter instead of newline. |
| `-a array` |  Store words into an array. |

2. `mapfile`: Reads lines from standard input or a file into an indexed array (mostly used with `-t`).
- Syntax :
```bash
mapfile [options] array_name # No need to declare array explictly
```
- Useful mapfile options:

| Option | Meaning |
| --- | --- |
| `-t` | Removes the trailing newline from each line |
| `-n N` | Reads at most N lines |
| `-s N` | Skips the first N lines |
| `-d DELIM` | Uses a specified delimiter instead of newline |

- Example: `mapfile -t names`, This stores lines in the array without the trailing newline character.

## Conditional Statments
### Case statments
```bash
case EXPRESSION in
  PATTERN_1) COMMANDS ;;
  PATTERN_2) COMMANDS ;;
  *) DEFAULT_COMMANDS ;; # where PATTERN is "*" (i.e wildcard, run always)
esac # opposite of case -> esac
```
- **Key Rules:**
    - `case` starts the block; `esac` ends it.
    - Patterns end with `)` and commands end with `;;`.
    - `*` acts as the default case.
    - Bash stops after the first matching pattern (no fall‑through).
    - Use `|` to match several values in one clause/PATTERN :
    - Patterns support globbing (`*`, `?`, `[abc]`) for flexible matching.

### if-elif-else condition
- Syntax:
```bash
if condition; then
    commands
elif condition; then
    commands
else
    commands
fi # opposite of if
```
### **How TO Write Conditions**
In Bash scripting, we use conditional expressions inside `if`, `elif`, `while`, and other constructs to make decisions based on whether a condition is true or false.

1. **`test` Expression** : Uses the built-in test command to evaluate file checks, string comparisons, or numeric comparisons.
```bash
# Example
if test -e file; then
    command
fi
```

2. **Single Brackets [ exprn ]**: Functionally equivalent to the test command (`[` is an alias/command for test in POSIX shells).
- Important syntax detail: Spaces must appear inside the brackets **[ exprn ]**, not [exprn], and variables are typically quoted (`"$var"`) to prevent word-splitting or globbing issues.
- Syntax :
```bash
if [ condition ]; then
    commands
fi
# Example
if [ -e file ]; then
    command
fi
```
---
#### File and directory conditions
These conditions check whether a file or directory exists and what its properties are.
| Condition | Meaning |
| --- | --- |
| `-e file` | Checks whether a file or directory exists |
| `-f file` | Checks whether it exists and is a regular file |
| `-d file` | Checks whether it exists and is a directory |
| `-r file` | Checks whether it is readable |
| `-w file` | Checks whether it is writable |
| `-x file` | Checks whether it is executable |
| `-s file` | Checks whether it exists and has a non-zero size |
| `-L file` | Checks whether it is a symbolic link |
| `-O file` | Checks whether the current user owns it |
| `-G file` | Checks whether the current user's group owns it |
| `file1 -nt file2` | Checks whether file1 is newer than file2 |
| `file1 -ot file2` | Checks whether file1 is older than file2 |
| `file1 -ef file2` | Checks whether both refer to the same file |

- There are more conditions to check.
---
#### String conditions
These conditions are used to compare strings or check whether a string is empty.
| Condition | Meaning |
| --- | --- |
| `-z "$str"` |	True if the string is empty (zero length) |
| `-n "$str"` |	True if the string is not empty |
| `"$str1" = "$str2"` |	True if both strings are equal |
| `"$str1" != "$str2"` |	True if strings are different |
| `"$str1" < "$str2"` |	Lexicographically less than (inside `[[ ]]`) |
| `"$str1" > "$str2"` |	Lexicographically greater than (inside `[[ ]]`) |

---
#### Numeric conditions
These conditions compare integer values.
| Condition | Meaning |
| --- | --- |
| `$n1 -eq $n2` | Check if n1 is equal to n2 |
| `$n1 -ge $n2` | Check if n1 is greater than or equal to n2 |
| `$n1 -gt $n2` | Check if n1 is greater than n2 |
| `$n1 -le $n2` | Check if n1 is less than or equal to n2 |
| `$n1 -lt $n2` | Check if n1 is less than n2 |
| `$n1 -ne $n2` | Check if n1 is not equal to n2 |
---
3. **Double Brackets [[ exprn ]]**: Supports pattern matching (like `[[ $pasword =~ ^[0-9]+[A-Z][a-z] ]]`) and regular expressions without unexpected word splitting on unquoted variables.
- Bash also supports regular expressions using the `=~` operator inside `[[ ]]`
- Syntax :
```bash
if [[ exprn ]]; then
    command
fi
# Example
if [[ $email =~ ^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$ ]]; then
    command
fi
```
- Operators which can be used inside `[[  ]]`

    | Operator | Meaning |
    | --- | --- |
    | `==` | Equal |
    | `!=` | Not equal |
    | `>` | Greater than |
    | `<` | Less than |
    | `>=` | Greater than or equal |
    | `<=` | Less than or equal |
- Bash also provides some useful conditions for checking variables and file descriptors.

    | Condition | Meaning |
    | --- | --- |
    | `-v variable` | Checks whether a variable is set |
    | `-R variable` | Checks whether a variable is a nameref |
    | `-t 0` | Checks whether stdin (file descriptor 0) is connected to a terminal |
    | `-t 1` | Checks whether stdout (file descriptor 1) is connected to a terminal |
    | `-t 2` | Checks whether stderr (file descriptor 2) is connected to a terminal |

---
4. **Double Parentheses ((( exprn )))**: Used for arithmetic conditions involving integers.
- Variables inside `(( ... ))` can be referenced directly (like `v` or `$v`), and standard math operators such as `**` (exponentiation), `>`, `<`, `==`, `&&`, and `||` work naturally.
- Syntax :
```bash
if (( exprn )); then
    command
fi
# Example
if (( $salary > 80000 )); then
    echo "Pay more tax"
fi
```

---
5. **A Command (wc -l file)**:

- Syntax :
```bash
if command; then # ture or false based on the status of command
    command
fi
# Example
if wc -l input.txt; then
    echo "File is non empty"
fi
```
---
6. **A Pipeline** (`command1 | command2`) : By default, a pipeline's exit status is the exit status of the last command in the chain.
- Syntax :
```bash
if command1 | command2; then # ture or false based on the status of command
    command
fi
# Example
if ls | grep '*.log'; then
    echo "Log file is present"
fi
```
7. **Negation (`!` condition)**: Placing ! (with a space after it) before a condition inverts its true/false result.
- Can be places in front of any conditions above, with space in between.
- Syntax :
```bash
if ! condition; then # ture or false based on the status of command
    command
fi
# Example
if ! ls ./bangbang 2>/dev/null; then 
    echo "Directory is not there"
fi
```
#### **Logical conditions (AND, OR, NOT)**
Logical operators allow us to combine multiple conditions.
| Operator | Meaning |
|---|---|
| `&&` | AND – both conditions must be true |
| `\|\|` | OR – at least one condition must be true |
| `!`| NOT – reverses the condition |
---

Logical operators behave differently depending on whether you're using `[ ]`, `[[ ]]`, or `(( ))`.
- **With single square brackets [ ]**:  `&&` and `||` are generally used between separate test commands, not as operators inside a single `[ ]` expression.
```bash
if [ "$a" -gt 10 ] && [ "$b" -lt 20 ]; then
    echo "Both conditions are true"
fi
```
- **With double square brackets [[ ]]**: All three operators `&&`, `||`, and `!` can be used inside `[[ ]]`.
```bash
if [[ ! condition1 || condition2 ]]; then # only first condition gets negation
    echo "At least one condition is true"
fi
```
- **With double parentheses (( ))**: All three can be used, but here they are interpreted as arithmetic/logical operators.
```bash
if (( a > 10 && b < 20 )); then
    echo "Both conditions are true"
fi
```
---
## Loops
## Functions

## Login shell and Non-login shell
In Bash, Login shells and Non-login shells are two types of shell sessions. The difference is mainly how the shell starts and which configuration files it reads during startup, This concept is important because it explains why sometimes your environment variables, PATH, aliases, or other settings are available in one terminal but not another.

### 1. Login Shell :
A login shell is started when we authenticate into a session (for example, logging in over SSH, using a text console/TTY, or starting a shell with `bash -l` / `--login`).
- A login shell reads its login configuration files to initialize the environment.
- For Bash, it reads the first available file among:
1. `/etc/profile` — system-wide login configuration.
2. `~/.bash_profile` — user's login configuration.
3. `~/.bash_login` — if `.bash_profile` doesn't exist.
4. `~/.profile` — if neither `.bash_profile` nor `.bash_login` exists.
- It reads /etc/profile and then only the first available user file among `.bash_profile`, `.bash_login`, and `.profie`.


### 2. Non-Login Shell
A non-login shell is started after we are already logged in—such as opening a new terminal window/tab inside a desktop environment, running bash inside an existing shell, or running a shell script.
- When Bash starts as a non-login, interactive shell, it reads `~/.bashrc`, this file is commonly used to configure:
    - Aliases
    - Shell functions
    - Prompt appearance
    - Environment variables
    - Other interactive shell settings

## Run a Script

There are two primary ways to run a shell script in Unix/Linux: 
### 1. **Sourced Scripts** :
When you source a script, the commands are read and run directly inside **current shell session** rather than a separate child process. 
- Primary Use Case: Preparing or configuring your shell environment (e.g., loading environment variables, functions, or aliases into your current terminal, or reloading config files).
- Process ID (PID): Same as the current shell.
- Syntax:
```bash
source script.sh
# Or using the shorthand:
. script.sh
```
#### Example:
Suppose script.sh contains:
```bash
#!/bin/bash
name="Phani"
cd /home
```
- Now run: `source script.sh`
- What happens?
    - The variable `name` is created in your current shell.
    - Your current working directory changes to `/home`.
    - These changes remain even after the script finishes.
### 2. **Executed Scripts** :
When you execute a script directly, the system spawns a new child process (subshell) to run the commands.
- Syntax:
```bash
bash script.sh
# Or, if the script has execute permission:
./script.sh
```
- Primary Use Case: Running standalone tasks, automation, or new functionality without polluting your current shell session.
- Permissions Required: The file must have execute permission.
- Process ID (PID): Different as the current shell.
#### Example:
Suppose script.sh contains:
```bash
#!/bin/bash
name="Phani"
cd /home
```
- Now run: `./script.sh` or `bash script.sh`
- What happens?
    - The variable `name` is created inside the script's shell (subshell).
    - The working directory changes to `/home` inside that shell (subshell).
    - Once the script finishes, those changes disappear from the perspective of your original shell.

### Script location
Use absolute path or relative path while executing the script.
- We know we can run the script using :
```bash
./script.sh # if present in PWD
# OR
/home/username/scripts/script.sh # from different directory
```
- If we want to run a script simply by typing its name, without specifying its path, then its location must be in one of the directories listed in `$PATH` variable.
```bash
echo $PATH # /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
# suppose script "script.sh" is present in "/usr/bin" 
backup.sh # simply typing the name of script works 
# The shell searches PATH and finds your script.
```
- OR, temporarily add it for the current terminal session:
```bash
export PATH="$PATH:/home/username/scripts"
# Every script in side scripts can be executed just by it's name
```

# Some Command Line Utilities
- `find` : Covered in week 4 

- `stat` : displays detailed information about a file or file system, including size, permissions, ownership, timestamps, inode, and device data.

## `make` Command
The`make` command in Linux is used to automatically build and manage programs by reading instructions from a file called **Makefile**. It helps compile source code, rebuild only the files that have changed, and execute defined tasks.
- But we can do more with it rather than just compilation.
- Syntax
```bash
make [options] [target]
```
- target – Specifies what task or file to build.
- options – Modify how make operates.
- If no target is specified, make runs the first target defined in the Makefile.

1. Common options: 

    | Option |	Explanation |
    | --- | --- |
    | `-f filename` | Specifies a custom Makefile name. |
    | `-j` |	Runs multiple jobs in parallel. |
    | `-n` |	Displays commands without executing them (dry run). |
    | `-B` |	Forces rebuilding of all targets regardless of timestamps. |
    | `-s` |	Runs commands silently, without displaying them. |
    | `-C directory` | Changes to the specified directory before running make. |
    | `-k` |	Continues building other targets even if one fails. |
    | `-v` |	Displays the version of GNU Make. |
    | `-h` |	Displays help and available options. |

### Understanding `Makefile` in Linux
A **Makefile** is a file containing rules that tell the `make` command how to build, compile, or perform tasks automatically. To write a Makefile properly, you need to understand its structure, variables, symbols, rules and a few important conventions.

#### 1. Basic Structure of a Makefile

A Makefile rule generally follows this structure:

```makefile
target: prerequisites
<TAB>	command
```

| Part | Meaning |
|---|---|
| `target` | The file or task you want to create or execute. |
| `prerequisites` | Files or dependencies required to create the target. |
| `command` | The command that executes to build the target. |

**Important:** The command (recipe) line must begin with a **TAB**, not spaces.

##### Example

```makefile
program: main.c
	gcc main.c -o program
```

Here, `program` is the target, `main.c` is the prerequisite, and `gcc main.c -o program` is the recipe that creates the executable.

When you run `make`, it checks whether the target needs to be built or updated based on its prerequisites.

---

#### 2. Variables in Makefile

Variables allow you to store values such as filenames, compiler names, flags and commands so you don't have to repeat them everywhere.

##### Example

```makefile
CC = gcc
CFLAGS = -Wall
TARGET = program
SRC = main.c

$(TARGET): $(SRC)
	$(CC) $(CFLAGS) $(SRC) -o $(TARGET)
```

- `$(VARIABLE)` is the syntax used to access or expand a variable's value.
- For example, `$(CC)` becomes `gcc`, and `$(TARGET)` becomes `program`.

---

#### 3. Difference Between `=`, `:=`, `?=`, `+=` and `!=`

These symbols are assignment operators used to define Makefile variables.

- `=` — Recursive Assignment

```makefile
A = hello
B = $(A) world
A = Linux
```

Now `B` becomes `Linux world`, because the value of `A` is evaluated when `B` is used, not when `B` is initially defined.

- `:=` — Immediate Assignment

```makefile
A := hello
B := $(A) world
A := Linux
```

Now `B` remains `hello world`, because its value was evaluated and stored at the time of assignment.

- Some more :
    | Symbol | Meaning |
    |---|---|
    | `=` | Assigns a value with deferred evaluation. |
    | `:=` | Assigns a value evaluated immediately. |
    | `?=` | Assigns only if the variable is not already defined. |
    | `+=` | Appends a value to an existing variable. |
    | `!=` | Executes a shell command and stores its output. |

- We can also use `Variable_name<symbol>$(VARIABLE)` like `DATESTAMP:=$(shell date +"%Y-%m-%d_%H.%M")`

---

#### 4. Important Symbols Used in Makefiles

| Symbol | Meaning |
|---|---|
| `:` | Separates target from prerequisites. |
| `$` | Used to access variables and automatic variables. |
| `#` | Starts a comment. |
| `\` | Continues a line onto the next line. |
| `;` | Allows a recipe command to be written on the same line as the target rule. |
| `*` | Wildcard used for matching filenames. |
| `%` | Pattern-matching symbol used in pattern rules. |
| `-` | In a recipe, ignores a command's error; before a target in a command-line option context, its meaning differs. |
| `@` | Hides the command from being printed before execution. |
| `+` | Marks a recipe line to be executed even in certain special modes, such as `make -n`. |

---

#### 5. Automatic Variables

Automatic variables are special variables that `make` automatically sets for each rule.

| Variable | Meaning |
|---|---|
| `$@` | Name of the target. |
| `$<` | First prerequisite. |
| `$^` | All prerequisites, without duplicates. |
| `$?` | Prerequisites that are newer than the target. |
| `$*` | Stem matched by a pattern rule. |

##### Example

```makefile
program: main.c utils.c
	gcc $^ -o $@
```

Here:

- `$^` → `main.c utils.c`
- `$@` → `program`

So the recipe effectively runs:

```bash
gcc main.c utils.c -o program
```

These automatic variables help you avoid writing filenames repeatedly.

---

#### 6. The `.PHONY` Rule

`.PHONY` is used to declare targets that are not actual files, but rather tasks or actions.

```makefile
.PHONY: clean run

clean:
	rm -f program

run:
	./program
```

Without `.PHONY`, if a file named `clean` already exists, `make clean` might consider the target up to date and skip the recipe.

**Use `.PHONY` for task names such as `clean`, `run`, `install`, `test` and `all`.**

---

#### 7. A Complete Example of a Makefile

Consider a Make file named `make.file` :
```bash
BACKUPDIR=/home/gphani/backup
DATESTAMP:=$(shell date +"%Y-%m-%d_%H.%M")
TARBALL=backup_$(DATESTAMP).tar

backup: file1 file2
	@echo "Backing up file1 file2 to $(DATESTAMP)"
	tar -cvf $(TARBALL) file1 file2
	@ls -l $(TARBALL)
	mv $(TARBALL) $(BACKUPDIR)
```
**Purpose**: This Makefile creates a timestamped TAR archive of file1 and file2, displays information about the archive and then moves it into the backup directory.
- `$(shell ...)`: executes a shell command and captures its output.
- `@`: hides the command itself while displaying its output. (i.e `@echo "Backing up file1 file2 to $(DATESTAMP)"`, Displays a message on the terminal informing the user that the backup is being created.).

```bash
gphani@icme:~$ mkdir backup
gphani@icme:~$ echo "Hello world" > file1
gphani@icme:~$ echo "Hello world 1" > file2
gphani@icme:~$ cp logfiles
logfiles/     logfiles.tar
gphani@icme:~$ cp logfiles
gphani@icme:~$ make -f make.file backup # -f to specify read from file (default look for  Makefile)
Backing up file1 file2 to 2022-03-06_22.26
tar -cvf backup_2022-03-06_22.26.tar file1 file2
file1
file2
-rw-rw-r-- 1 gphani gphani 174080 Mar  6 22:26 backup_2022-03-06_22.26.tar
mv backup_2022-03-06_22.26.tar /home/gphani/backup
gphani@icme:~$ ls -l backup
total 172
-rw-rw-r-- 1 gphani gphani 174080 Mar  6 22:26 backup_2022-03-06_22.26.tar
gphani@icme:~$ date
Sunday 06 March 2022 10:27:11 PM IST
gphani@icme:~$ make -f make.file backup
make: 'backup' is up to date. # as there no changes made so no need for re-run
gphani@icme:~$ ls >> file1
gphani@icme:~$ make -f make.file backup
Backing up file1 file2 to 2022-03-06_22.27
tar -cvf backup_2022-03-06_22.27.tar file1 file2
file1
file2
-rw-rw-r-- 1 gphani gphani 174080 Mar  6 22:27 backup_2022-03-06_22.27.tar
mv backup_2022-03-06_22.27.tar /home/gphani/backup
```

**The main idea:** A Makefile is built around **targets, prerequisites, recipes and variables**. Once you understand these four, writing Makefiles becomes much easier.

## Group and Compress Files

- `tar` : The command bundles multiple files or directories into an archive without compression. (week 1), Uncompress using `tar -xvf archive.tar`.
- `zip`: Combines files and directories into an archive and compresses them, Uncompress using `unzip archive.zip`.
- `compress`: Compresses files using the older LZW algorithm, usually producing .Z files, Uncompress using `uncompress file.Z`.
- `gzip`: Compresses individual files into .gz format to reduce their size, Uncompress using `gunzip file.gz`.
- `bzip2`: Compresses files into .bz2 format, often offering better compression than gzip, Uncompress using `bunzip2 file.bz2`.
- `xz`: Compresses files into .xz format, generally providing a high compression ratio, Uncompress using `unxz file.xz`.
- `7z`: Archives and compresses files into the .7z format, often achieving high compression efficiency, Uncompress using `7z x archive.7z`.
