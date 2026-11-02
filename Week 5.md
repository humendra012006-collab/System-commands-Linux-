# Shell Variables
## Overview
Shell variables in Linux are named data storage units used by the shell to hold values such as strings, numbers, paths, or command outputs, and they include local variables, environment variables, and special built‑in variables like `PATH`, `HOME`, and `$`.
### Types of Shell Variables
1. **Local Variables** : Local variables are variables that are defined inside a shell or script and exist only for the duration of that shell session or script execution. Once the script finishes or the shell exits, these variables are automatically destroyed.
2. **Environment Variables** : Environment variables are special variables that are stored in the shell environment and are automatically passed to child processes created from that shell. They are used to configure system behavior and application settings (like : `$PATH`, `$HOME`, `$USER`, `$PWD`).
3. **Built-in Shell Variables** : Built-in shell variables are predefined variables that are automatically created and managed by the shell to provide information about the system, shell environment and script execution state (like : `$SHELL`, `$HOSTNAME`, `$?`).

## Frequently used shell variables
- To view all the variables use commands like : `printenv`, `env`, `set`.

| Variable | Description |
| --- | --- |
| `$0` | Name of the shell or script (`echo $0`) |
| `$1` - `$9` | Positional arguments passed to script (`echo $1`) |
| `$?` | Exit status of last command (0 - 255) |
| `$$` | Process ID (PID) of current shell (`echo $$`) |
| `$!` | Process ID of last background job (`echo $!`) |
| `$-` | Flags set in the bash shell (like : `hBimHsc`) |
| `HOME` | Home directory path (`cd $HOME`) |
| `PATH` | Executable search directories (`export PATH=$PATH:/bin`) |
| `USER` | Current logged-in username (`echo $USER`) |
| `PWD` | Current working directory (`echo $PWD`) |
| `OLDPWD` | Previous working directory (`cd $OLDPWD`) |
| `SHELL` | Default login shell path (`echo $SHELL`) |
| `HOSTNAME` | Machine host name (`echo $HOSTNAME`) |
| `RANDOM` | Random integer from 0 to 32767 (`echo $RANDOM`) |

### List of Existing variable
```bash
gphani@icme:~$ echo ${!<match_string>} # List of names of shell variables that start with match_string
gphani@icme:~$ echo ${!HO*}
HOME HOSTNAME HOSTTYPE
```
## Defining Variables
When creating shell variables, you must follow certain rules to ensure your script runs correctly.

```bash
# Syntax : Defining a shell variable
myvar="value"
myvar= # assinged empty string i.e. ""
# OR
declare [options] myvar="value"
declare myvar # declared but, unset variable 
# OR
export myvar="value" # Marks a shell variable so it becomes an environment variable.
# Makes the variable available to child processes (scripts, programs, subshells).
```
- **`myvar`** (Variable Name):
    - Names can contain letters (a-z, A-Z), numbers (0-9) and underscores `_`.
    - Names must start with a letter or underscore
    - **Can't** start with a number
- **`=`** (Assignment Operator):
    - **No space** around the `=` sign
- **`"value string"`** (Value):
    - Can be a number, string, or \`command\` (inside Backtick)

### Using `declare` :
Using Bash’s declare command for typed or attribute‑controlled variables such as integers, arrays, or read‑only values.

| Option | Purpose | Example |
| --- | --- | --- |
| `-i` | Integer variable | `declare -i num=10` |
| `-a` | Indexed array | `declare -a arr #(1 2 3)` |
| `-A` | Associative array  | `declare -A map #([key]=value)` |
| `-r` | Read-only variable | `declare -r PI=3.14` |
| `-l` | Convert value to lowercase | `declare -l lower="HELLO"` |
| `-u` | Convert value to uppercase | `declare -u upper="hello"` |
| `-x` | Export variable to environment | `declare -x PATH="$PATH:/new/path"` |
| `-g` | Declare variable globally inside functions | `declare -g globalVar="value"` |

- `-i` : when we declare a shell variable using `-i`, it only allows integer type values to store if we assigned value other than integer type (like string) it will store 0 (zero).
```bash
gphani@icme:~$ declare -i num=10
gphani@icme:~$ echo ${num}
10
gphani@icme:~$ num=hello # or num="hello"
gphani@icme:~$ echo ${num}
0
gphani@icme:~$ declare +i num=Hello
gphani@icme:~$ echo ${num}
Hello
```
- `-l` or `-u` : Both declare variable to either lowercase or uppercase respectively, even if we assigned uppercase value to `-l` will store it as lowercase only and vice versa.
```bash
gphani@icme:~$ declare -u str="HELLo"
gphani@icme:~$ echo ${str}
HELLO
gphani@icme:~$ str="lowercase"
gphani@icme:~$ echo ${str}
LOWERCASE
gphani@icme:~$ declare -l strlow=`echo ${str}` # same as srtlow=LOWERCASE
gphani@icme:~$ echo ${strlow}
lowercase
gphani@icme:~$ declare +u str="hello"
gphani@icme:~$ echo ${str}
hello
```
- `-r` : If we declare a variable using `-r` and assign it, we can't re-assign it and it is read only, neither using `+r`.
- We can also remove the restrictions that we apply using `+` :
| Option | Purpose | Example |
| --- | --- | --- |
| `+i` | Remove Integer variable restriction | `declare +i num=HELLO` |
| `+l` | Remove Lowercase restriction | `declare -l strlow="HELLO"` |
| `+u` | Remove Uppercase restriction | `declare -u str="hello"` |

### Variables Default value
1. **Substitute default value** :
If the variable is unset, or if it is set to an empty string, use this default value instead.
```bash
gphani@icme:~$ unset MY_VAR
gphani@icme:~$ echo "${MY_VAR:-guest}"   
guest
gphani@icme:~$ MY_VAR=""
gphani@icme:~$ echo "${MY_VAR:-guest}"   
guest
gphani@icme:~$ MY_VAR="alice"
gphani@icme:~$ echo "${MY_VAR:-guest}"   
alice
```
- **if** variable is set (not as empty string): 
    - display its value
- **else**: 
    - display “guest”
- If we want empty string ("") as a valid, intentional value and only apply the default if the variable has never been declared, omit the colon.
    ```bash
    gphani@icme:~$ unset MY_VAR
    gphani@icme:~$ echo "${MY_VAR-guest}"   
    guest
    gphani@icme:~$ MY_VAR=""
    gphani@icme:~$ echo "${MY_VAR-guest}"   
    
    gphani@icme:~$ MY_VAR="alice"
    gphani@icme:~$ echo "${MY_VAR-guest}"   
    alice
    ```

2. **Set default value** : 
If variable is never set and we want to actually assign the default value back into the variable as it's value, we can use the equals sign `=`.

```bash
gphani@icme:~$ decare MY_VAR
gphani@icme:~$ echo "${MY_VAR:=guest}"   
guest
gphani@icme:~$ echo ${MY_VAR}
guest
gphani@icme:~$ unset MY_VAR
gphani@icme:~$ MY_VAR=""
gphani@icme:~$ echo "${MY_VAR:=guest}"   

gphani@icme:~$ echo ${MY_VAR}

gphani@icme:~$ unset MY_VAR
gphani@icme:~$ : ${MY_VAR:=guest} # works same but won't print anything
```
- **if** myvar is set (empty string is considered as set):
    - display its value
- **else**:
    - set “guest” as its value display its new value
3. **Reset value if variable is set** : 
```bash
${variable:+alternate_value}
```
- If the variable is set and non-empty: It discards the variable's current value and substitutes `alternate_value`.
- If the variable is unset OR empty (""): It evaluates to nothing (an empty string).
```bash
gphani@icme:~$ decare MY_VAR
gphani@icme:~$ echo "${MY_VAR:+guest}"   

gphani@icme:~$ MY_VAR=""
gphani@icme:~$ echo "${MY_VAR:+guest}"   

gphani@icme:~$ MY_VAR="buyy"
gphani@icme:~$ echo "${MY_VAR:+HELLO}" 
HELLO
gphani@icme:~$ echo ${MY_VAR}
buyy
```
- **if** variable is set:
    - display alternate_value
- **else**:
    - display null
## Accessing a variable
The `$` dereferences the variable.
```bash
echo $myvar # correct
echo ${myvar} # correct ,good only for variable 
echo “The value of variable is ${myvar}” # correct and recommended
echo '$myvar' # treat as litral, output : $myvar
echo \$myvar # escape the $, output : $myvar
```
### Backslash: `\`
The backslash escapes the next character, preventing its usual special interpretation in this context.

```bash
gphani@icme:~$ echo \$HOME # escape the special meaning of $
$HOME
gphani@icme:~$ date
Mon Nov  2 14:17:41 UTC 2026
gphani@icme:~$ date -R
Mon, 02 Nov 2026 14:17:43 +0000
gphani@icme:~$ alias date="date -R"
gphani@icme:~$ date
Mon, 02 Nov 2026 14:18:19 +0000
gphani@icme:~$ \date # escape alias
Mon Nov  2 14:18:40 UTC 2026
```
### Test if a variable is **set**:
`[[ -v myvar ]]`: This is a conditional test using the -v operator. It checks if the shell variable named myvar has been set (i.e., defined).
- It returns true even if the variable holds an empty string (e.g., myvar=""). It only returns false if the variable has never been declared or assigned.
- echo `$?`: This prints the exit status of the immediately preceding command (`[[ -v myvar ]]`).
    - `0` means success (true: the variable is set).
    - `1` means failure (false: the variable is not set).
```bash
[[ -v myvar ]];
echo $?
```
### Test if a variable is **not set**:
```bash
[[ -z ${myvar+x} ]];
echo $?
```
This is a traditional, highly compatible method used to check if a variable is unset (especially useful in older versions of Bash or POSIX-compliant shells where -v isn't available).

- `${myvar+x}` (Parameter Expansion):
    - If `myvar` is set (**even if empty ""**), this expression evaluates to the letter x.
        - When using `${variable+word}`, if the `variable` is set, Bash completely ignores whatever value is inside the variable and instead substitutes whatever `word` follows the plus sign
    - If `myvar` is not set (unset), this expression evaluates to an empty string ("").
        - When using `${variable+word}`, If the `variable` is unset, the expansion yields nothing, results in empty string ("").
- `-z`: This conditional operator checks if a string has a length of zero (is empty), Combining them ([[ -z ... ]]):
    - If `myvar` is not set, the parameter expansion yields an empty string, making `-z` true.
    - If `myvar` is set, the parameter expansion yields x, making `-z` false.
- `echo $?`: Prints the exit status of the test.
    - `0` means success (true: the variable is not set).
    - `1` means failure (false: the variable is set).

## Removing a variable or its value
```bash
unset myvar # This removes the variable entirely from the shell's environment
myvar= # This does not remove the variable — The variable still exists in the shell's environment, but its value is "".
myvar="" # same as myvar=
```
