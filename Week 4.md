# Understand Shell Features And Regex 
When we type a command and hit Enter, the shell doesn't just pass our text directly to the OS. Instead, it processes the command line through a series of expansion and redirection steps before executing the command (like: Shell expansion, Parameter expansion, Brace expansion) .
- Note : Week 4 containt Only `regex` and `string commands` but to know the difference Shell Features topic is added as well.
- This is different from **regex**, where text-processing utilities like `grep`, `sed`, and `awk` supports **regex** which is not resolved by shell.
- Bash provides us with many features like :
    | Feature                             | What it does                                      | Example                             |
    | ----------------------------------- | ------------------------------------------------- | ----------------------------------- |
    | 1. Quoting and escaping             | Controls how Bash interprets characters           | `echo "*.txt"`                      |
    | 2. Brace expansion                  | Generates multiple strings                        | `echo file{1..4}.txt`               |
    | 3. Tilde expansion                  | Expands home-directory shortcuts                  | `cd ~`                              |
    | 4. Parameter expansion              | Reads and manipulates variables                   | `echo "${HOME}"`                    |
    | 5. Command substitution             | Replaces a command with its output                | `today=$(date)`                     |
    | 6. Arithmetic expansion             | Calculates arithmetic expressions                 | `echo $((5 + 3))`                   |
    | 7. Word splitting                   | Splits unquoted expansion results into words      | `files=$list; printf '%s\n' $files` |
    | 8. Filename expansion (globbing)    | Matches filenames using wildcard patterns         | `ls *.txt`                          |
    | 9. Redirection                      | Sends input/output to files or other destinations | `ls > files.txt`                    |
    | 10. Pipelines                       | Connects commands through standard streams        | `cat file.txt \\| grep error`       |
    | 11. Lists and command chaining      | Controls execution order and conditions           | `mkdir test && cd test`             |
    | 12. Control structures              | Implements decisions and loops                    | `if`, `for`, `while`, `case`        |
    | 13. Functions                       | Groups commands into reusable units               | `greet() { echo Hello; }`           |
    | 14. Process control                 | Runs jobs in the background and manages processes | `sleep 10 &`                        |
    | 15. Here-documents and here-strings | Supplies text as command input                    | `grep error <<< "$line"`            |
    | 16. Process substitution            | Presents command output/input as a file-like path | `diff <(sort a) <(sort b)`          |
    | 17. Shell options and builtins      | Changes shell behavior and uses built-in commands | `set -o pipefail`                   |
    | 18. Arrays and special parameters   | Stores multiple values and accesses shell state   | `arr=(a b c)`                       |

- We will see only the most important among them and also some of them are already know to us (Also some in Week 5).
- Globbing is also called pathname expansion or filename expansion.
- Single quotes protect the `regex` from shell interpretation, while with double quotes variables and command substitutions still work, but the wildcard is not expanded.

## Wildcards In Bash
Wildcards are special characters that can be used to match filenames or patterns.

**Common Wildcards:**
-   `*` (matches any number of characters, including none)
-   `?` (matches a single character)
-   `[...]` (matches any one of the enclosed characters)

1. Asterisk : `*`
Matches zero or more characters in a filename component.
```bash
touch file{8..13}.txt # creating files, file8.txt to file13.txt
ls *.txt # List all text files
```
- `*` : normally excludes hidden names.
- `.*` : matches dot-prefixed names, but can also match `.` and `..`.
- `.??*` : matches hidden names with at least two characters after the leading dot, avoiding `.` and `..`.
2. Question mark : `?`
Matches exactly one character in a filename component.
```bash
touch file{8..13}.txt # creating files, file8.txt to file13.txt
ls file?.txt # List only file8.txt to file9.txt
```
3. Bracket expression : `[ ... ]`
Matches only one character from a set or range. with `start-end` matches any one b/w start to end.
```bash
touch file{1..13}.txt # creating files, file1.txt to file13.txt
ls file[1389].txt # List file1.txt file3.txt file8.txt file9.txt
ls file[1-8].txt # List file1.txt to file8.txt
ls file[!2-9].txt # List only file1.txt as file10.txt, file11.txt, etc., have two characters (10, 11) in that position and bracket pair [...] can only ever test one character, the shell skips multi-digit filenames entirely.
```

## Quoting and escaping
Quoting is one of the most important Bash concepts because it determines which characters Bash interprets and which characters remain literal.
1. Single quotes: `'...'`
```bash
echo '$HOME *.txt'
# Output is the literal text $HOME *.txt. 
# Most characters inside single quotes lose their special meaning.
```
2. Double quotes: `"..."`
```bash
echo "$HOME" # Variables and command substitutions still work 
echo "*.txt" # But the wildcard is not expanded output '*.txt'
```
3. Backslash: `\`
```bash
echo \$HOME # output : $HOME
# backslash escapes the next character, preventing its usual special interpretation in this context.
```
## Brace Expansion
Brace expansion is a mechanism by which sequences of characters are generated.
- Firstly `shell` evaluates the command, generating the sequence and rewriting the command line.
- As a result, command actually recives the expanded version of it.
- WE :`command f{1..3}` --> Shell:`command f1 f2 f3` --> command: `process`
```bash
# Syntax:
{start..end}
touch file{1..3}.txt # shell evaluates as 'touch file1.txt file2.txt file3.txt'
echo {a,b}{1,2}	# output : a1 a2 b1 b2
echo {a,b,c} # Comma-separated output : a b c
```
## Other topices
- Parameter expansion: `$` and `${...}` : Covered in Week 5
- Redirection symbols : In week 3
- (...) : Run a command list in a subshell `(ps --forest)`

# Regular Expressions (regex) & `grep` command
Regular expressions (Regex) in Linux are strings of characters that form a search pattern used to find, match, and manipulate text within files or command output (i.e. `regex` is patten template to filter text). They are natively supported by core text-processing utilities like `grep`, `sed`, and `awk`.
- Due to the powerfullness of regular expressions, there are many engines in different languages and tools that implement regular expressions, but we stick to the ([IEEE 1003.1-2001](https://standards.ieee.org/standard/1003_1-2001.html)) POSIX standard which defines the regular expressions to be of two types:
    - **BRE** : POSIX Basic Regular Expression engine (Default)
    - **ERE** : POSIX Extended Regular Expression (Using `egrep` or `grep -E`)
engine
- **ERE** is easy to write.
- Basic Syntax of `grep` command :
```bash
grep [options] 'pattern(regex)' [file_name]
command | grep [options] 'pattern(regex)' # using pipe
```
## Special characters (BRE & ERE)
Single quotes must be used to pass the `regex` so that shell treat it as literal and pass it, as it is to the command (gernally `grep` command).

| Symbol | Discription|
| --- | --- |
| `.` | Any single character except **null** or **newline** |
| `*` | Zero or more of the **preceding** character / expression |
| `.*` | Any numbers of characters (could be zero characters) |
| `[ ]` | Any of the enclosed characters; hyphen (-) indicates character range |
| `[^ ]` | Any of the characters except enclosed; hyphen (-) indicates character range |
| `^` | Anchor for beginning of line (or negation of enclosed characters i.e. `[^ ]`) |
| `$` | Anchor for end of line |
| `\b` | End of the Word boundary |
| `\` | Escape special characters |

- Now some symboles are different between  **BRE** & **ERE**
### Special characters (BRE)
| Symbol | Discription |
| --- | --- |
| `\{n,m\}` | Range of occurances of preceding pattern at least **n** and utmost **m** times |
| `\( \)` | Grouping of regular expressions |

### Special characters (ERE)
| Symbol | Discription |
| --- | --- |
| `{n,m}` | Range of occurances of preceding pattern at least **n** and utmost **m** times |
| `( )` | Grouping of regular expressions |
| `+` | One or more of preceding character / expression |
| `?` | Zero or one of preceding character / expression |
| `\|` | Logical OR over the patterns |

### Backreferences in Regex
In simple words, you tell regex : 

“Remember what you matched earlier, and check whether that exact text appears again later.”

- Backreferences are useful for detecting repeated words, duplicate characters, matching paired text, and finding patterns where the same text must appear twice (or more times).
- `\number` : Patten must be present number + 1 times (`\1` must present two times, etc)
```bash
# suppose a file 'tem.txt' having some line as "hello humans hello !"
cat tem.txt | grep '(hello).*\1' # output : hello humans hello !
# as 'hello' is present two times, while '.*' make sure any thing in between
```
### `grep` OR `egrep` command
Commands `grep` or `egrep` is a powerful tools in Linux for searching patterns in files or input streams, `grep` use **BRE** while `egrep` use **ERE** and it is equivalent to `grep -E`.
- Must use single quotes shell treat pattern as litral and pass as it is to `grep` or `egrep`, which then process patterns.
```bash
grep [OPTIONS] 'PATTERN' [FILE...]
command | grep [OPTIONS] 'PATTERN'
egrep [options] 'PATTERN' [files]
command | egrep [options] 'PATTERN'
```

| Options | Discription | Example |
| --- | --- | --- |
| `-i` | case insensitive matching | `egrep -i 'error' logfile.txt` |
| `-v` | Invert match (show lines that do not match) | `egrep -v 'error' logfile.txt` |
| `-c` | Count the number of matching lines | `egrep -c 'error' logfile.txt` |
| `-n` | Show line numbers with matches | `egrep -n 'error' logfile.txt` |
| `-r` | Recursive search in directories | `egrep -r 'error' /var/logs/` |
| `-w` | Match whole words only | `egrep -w 'error' logfile.txt` |

## Linux Command-Line Exercises: grep and egrep
Below is the complete record of file inspection and pattern-matching commands performed on names.txt.

1. File Inspection & Basic Matching
```bash
gphani@icme:~$ cat names.txt
MM22B901 Mary Manickam
ED22B902 Raman Singh
ME22B903 Umair Ahmad
CS22B904 Charles M. Sagayam
EE22B905 Anu K. Jain
NA22B906 Anupama Sridhar
PH22B907 Vel Sankaran

gphani@icme:~$ grep 'Sa' names.txt
CS22B904 Charles M. Sagayam
PH22B907 Vel Sankaran

gphani@icme:~$ cat names.txt | grep 'Sa'
CS22B904 Charles M. Sagayam
PH22B907 Vel Sankaran
```

2. Wildcards and Literal Characters
```bash
# Match single character before 'am'
gphani@icme:~$ cat names.txt | grep '.am'
MM22B901 Mary Manickam
ED22B902 Raman Singh
CS22B904 Charles M. Sagayam
NA22B906 Anupama Sridhar

# Literal dot matching (escaped)
gphani@icme:~$ cat names.txt | grep '\.'
CS22B904 Charles M. Sagayam
EE22B905 Anu K. Jain
```

3. Anchors and Boundaries
```bash
# Match 'an' at the end of the line
gphani@icme:~$ cat names.txt | grep 'an$'
PH22B907 Vel Sankaran

# Match 'an' at a word boundary
gphani@icme:~$ cat names.txt | grep 'an\b'
ED22B902 Raman Singh
PH22B907 Vel Sankaran

# Match lines starting with 'E'
gphani@icme:~$ cat names.txt | grep '^E'
ED22B902 Raman Singh
EE22B905 Anu K. Jain

# Case-sensitive search for starting 'e' (no match)
gphani@icme:~$ cat names.txt | grep '^e'

# Case-insensitive search using -i
gphani@icme:~$ cat names.txt | grep -i '^e'
ED22B902 Raman Singh
EE22B905 Anu K. Jain
```

4. Character Classes and Ranges
```bash
gphani@icme:~$ cat names.txt | grep '[ME][ED]'
ED22B902 Raman Singh
EE22B905 Anu K. Jain
ME22B903 Umair Ahmad

gphani@icme:~$ cat names.txt | grep 'B90[1-4]'
MM22B901 Mary Manickam
ED22B902 Raman Singh
ME22B903 Umair Ahmad
CS22B904 Charles M. Sagayam

# Negated character range
gphani@icme:~$ cat names.txt | grep 'B90[^1-4]'
EE22B905 Anu K. Jain
NA22B906 Anupama Sridhar
PH22B907 Vel Sankaran
```

5. Repetitions and Backreferences
```bash
# 'M' occurs exactly twice
gphani@icme:~$ cat names.txt | grep 'M\{2\}'
MM22B901 Mary Manickam

gphani@icme:~$ cat names.txt | grep 'M\{1,2\}'
MM22B901 Mary Manickam
ME22B903 Umair Ahmad
CS22B904 Charles M. Sagayam

# Backreference: 'ma' occurs twice
gphani@icme:~$ cat names.txt | grep '\(ma\).*\1'
ME22B903 Umair Ahmad

# Extended grep: 'ma' occurs 1 or more times
gphani@icme:~$ cat names.txt | egrep '(ma)+'
ED22B902 Raman Singh
ME22B903 Umair Ahmad
NA22B906 Anupama Sridhar

gphani@icme:~$ cat names.txt | grep '\(.a\).*\1'
MM22B901 Mary Manickam
ME22B903 Umair Ahmad

gphani@icme:~$ cat names.txt | grep '\(a.\)\{3\}'
CS22B904 Charles M. Sagayam

gphani@icme:~$ cat names.txt | grep '\(a.\)\{2\}'
ED22B902 Raman Singh
CS22B904 Charles M. Sagayam
NA22B906 Anupama Sridhar
PH22B907 Vel Sankaran
```

6. Alternation (egrep)
```bash
# Here | is used as OR in regex
gphani@icme:~$ cat names.txt | egrep '(ED|ME)'
ED22B902 Raman Singh
ME22B903 Umair Ahmad

gphani@icme:~$ cat names.txt | egrep '(Anu|Raman)'
ED22B902 Raman Singh
EE22B905 Anu K. Jain
NA22B906 Anupama Sridhar
```

## Character classes
Character classes match specific types of characters in text files. These classes are enclosed within `[: :]` and used inside square brackets `[ ]`. They are particularly useful for pattern matching in Linux/Unix systems.

| Character class | Usage/Meaning |
| --- | --- |
| `[[:print:]]` | Printable 
| `[[:blank:]]` | Space / Tab
| `[[:alnum:]]` | Alphanumeric 
| `[[:space:]]` | Whitespace
| `[[:alpha:]]` | Alphabetic 
| `[[:punct:]]` | Punctuation
| `[[:lower:]]` | Lower case 
| `[[:xdigit:]]` | Hexadecimal
| `[[:upper:]]` | Upper case 
| `[[:graph:]]` | Non-space
| `[[:digit:]]` | Decimal digits 
| `[[:cntrl:]]` | Control characters

```bash
gphani@icme:~$ cat chartypes.txt
hello : alphabetical stuff : 5g
l : start lower end upper : H
L : start upper end lower : h
5g : alpha numeric stuff : 42
42 : solution to everything :
: start with control C end with dot : .
 
, : start with comma end with equals : =
  : start with blank end with control char :
gphani@icme:~$ cat chartypes.txt | grep '^[[:alpha:]]' # Those lines which starts with alphabet character
hello : alphabetical stuff : 5g
l : start lower end upper : H
L : start upper_end lower : h
gphani@icme:~$ cat chartypes.txt | grep '[[:digit:]]'
hello : alphabetical stuff : 5g
5g : alpha numeric stuff : 42
42 : solution to everything :
gphani@icme:~$ cat chartypes.txt | grep -v '[[:cntrl:]]' # With -v those lines are shown which don't match the pattern
hello : alphabetical stuff : 5g
l : start lower end upper : H
L : start upper end lower : h
5g : alpha numeric stuff : 42

, : start with comma end with equals : =
gphani@icme:~$ cat chartypes.txt | egrep -v '^$' # '^$' means enpty line with -v opposite of pattern, prints non empty lines only
hello : alphabetical stuff : 5g
l : start lower end upper : H
L : start upper end lower : h
5g : alpha numeric stuff : 42
42 : solution to everything :
: start with control C end with dot : .
, : start with comma end with equals : =
: start with blank end with control char :
```
- Suppose a file named `patterns.txt` with content given blow.
```text
Aadhar card number contains 12 digits and can look like 123456781234 for example.
Pincodes of cities in India contain 6 digits and that of IITM is 600036.
Phone numbers without the country code or 0 prefix for std code are 10 digits.
An example for my office landline is 4422574770 and prefix with 0 to dial me in my office
Roll numbers in IIT for regular students are of the pattern MM22B001 where the first two
letters correspond to the Department code, 2 digits for the year of joining, then the program
code character and then a 3 digit number for ther roll number within the class.
URLs can be given these days without the protocol like https://www.iitm.ac.in/ They can be
given as just github.com for example.
```

```bash
gphani@icme:~$ egrep '[[:digit:]]{12}' patterns.txt # grep line with at least 12 digits
Aadhar card number contains 12 digits and can look like 123456781234 for example.

gphani@icme:~$ egrep '[[:digit:]]{6}' patterns.txt 
Aadhar card number contains 12 digits and can look like 123456781234 for example.
Pincodes of cities in India contain 6 digits and that of IITM is 600036.
An example for my office landline is 4422574770 and prefix with 0 to dial me in my office.

gphani@icme:~$ egrep '\b[[:digit:]]{6}\b' patterns.txt # use word boundary of exect 6 digits
Pincodes of cities in India contain 6 digits and that of IITM is 600036.

gphani@icme:~$ egrep '\b[[:alpha:]]{2}[[:digit:]]{2}[[:alpha:]][[:digit:]]{3}\b' patterns.txt # grep word starting with 2 alphabets followed 2 digits followed alphabet and 3 digits at ending
Roll numbers in IIT for regular students are of the pattern MM22B001 where the first two

gphani@icme:~$ egrep '\b[[:alnum:]]+\.[[:alnum:]]+\b' patterns.txt # '+' means alphanumeric can occure one or more times
URLs can be given these days without the protocol like https://www.iitm.ac.in/ They can be
given as just github.com for example.
```

# `cut` command (Horizontal Trim)
We have trimmed output from top or botton using `head` & `tail`, `cut` trim the text from right or left (either by no. of fields or characters)
- `-c` : The `-c` option tells `cut` to operate on character positions.
- `-f` : Select the specified field, should use with `-d` to set the delimiter.

```bash
gphani@icme:~$ cat fields.txt
1234;hello world,line-1
234567;welcome cmdline,line-2
3456;parse text,line-3

gphani@icme:~$ cut -c 1-4 fields.txt # first 4 characters 
1234
2345
3456
gphani@icme:~$ cat fields.txt | cut -d " " -f 2 # space is delimiter and '-f 2' means second field
# '1234;hello' -- 1st field & 'world,line-1' -- 2nd field
world,line-1
cmdline,line-2
text,line-3

gphani@icme:~$ cat fields.txt | cut -d " " -f 1-2 # '-f 1-2' ie 1 to 2 field
1234;hello world,line-1
234567;welcome cmdline,line-2
3456;parse text,line-3

gphani@icme:~$ cat fields.txt | cut -d ',' -f 2
line-1
line-2
line-3

gphani@icme:~$ cat fields.txt | cut -d ";" -f 2 | cut -d "," -f 1
hello world
welcome cmdline
parse text

gphani@icme:~$ cat fields.txt | cut -d ";" -f 2 | cut -d "," -f 1 | head -n 2 | tail -n 1
welcome cmdline
```

# `find` command (search files & directorys)
The `find` command is a powerful Unix utility used to search for files and directories in a directory tree based on specified criteria. It can also perform actions such as printing, deleting, or executing commands on the matched files.
- Basic Syntax, If no path is provided, `.` (the current directory) is assumed.
```bash
find <path> <conditions> <actions>
```
- Suppose PWD structure is
```text
. # Current working directory
├── pic_1.jpg
├── pic_2.jpg
├── pic_3.jpg
├── pic_4.jpg
├── Test_Dir
│   ├── Sub_Dir
│   │   ├── subdir_file1.txt
│   │   ├── subdir_file10.txt
│   │   ├── subdir_file2.txt
│   │   ├── subdir_file3.txt
│   │   ├── subdir_file4.txt
│   │   ├── subdir_file5.txt
│   │   ├── subdir_file6.txt
│   │   ├── subdir_file7.txt
│   │   ├── subdir_file8.txt
│   │   └── subdir_file9.txt
│   ├── test_1.txt
│   ├── test_10.txt
│   ├── test_2.txt
│   ├── test_3.txt
│   ├── test_4.txt
│   ├── test_5.txt
│   ├── test_6.txt
│   ├── test_7.txt
│   ├── test_8.txt
│   └── test_9.txt
└── Website_Demo
    ├── .DS_Store
    └── public
        ├── images
        │   ├── background.jpg
        │   ├── header.jpg
        │   └── wallpaper.jpg
        ├── index.html
        ├── scripts
        │   ├── main.js
        │   └── script.js
        └── static
            └── styles.css
```
- List all the files & directorys with in given path.
```bash
gphani@icme:~$ find ./Website_Demo # or find Website_Demo
Website_Demo
Website_Demo/.DS_Store
Website_Demo/public
Website_Demo/public/images
Website_Demo/public/images/background.jpg
Website_Demo/public/images/header.jpg
Website_Demo/public/images/wallpaper.jpg
Website_Demo/public/index.html
Website_Demo/public/scripts
Website_Demo/public/scripts/main.js
Website_Demo/public/scripts/script.js
Website_Demo/public/static
Website_Demo/public/static/styles.css
```
## Common options
- `-type` : Filter results by file type : `f` for file, `d` for directory, `l` for symlink.
```bash
gphani@icme:~$ find . -type d
.
./My_Project
./My_Project/project
./Test_Dir
./Test_Dir/Sub_Dir
./Website_Demo
./Website_Demo/public
./Website_Demo/public/images
./Website_Demo/public/scripts
./Website_Demo/public/static
```
- `-name` : Match filenames using a case-sensitive pattern.
```bash
gphani@icme:~$ find . -type f -name "test_1.txt"
./Test_Dir/test_1.txt
gphani@icme:~$ find . -type f -name "test*"
./Test_Dir/test_1.txt
./Test_Dir/test_10.txt
./Test_Dir/test_2.txt
./Test_Dir/test_3.txt
./Test_Dir/test_4.txt
./Test_Dir/test_5.txt
./Test_Dir/test_6.txt
./Test_Dir/test_7.txt
./Test_Dir/test_8.txt
./Test_Dir/test_9.txt
gphani@icme:~$ find . -type f -name "*.py"
./My_Project/project/__init__.py
./My_Project/project/my_module.py
./My_Project/project/sample.py

```

- `-iname` : Same as `-name` but case-insensitive matching.
```bash
gphani@icme:~$ find . -type f -iname "test*"
./Test_Dir/TEST1.txt
./Test_Dir/TEST10.txt
./Test_Dir/TEST2.txt
./Test_Dir/TEST3.txt
./Test_Dir/TEST4.txt
./Test_Dir/TEST5.txt
./Test_Dir/TEST6.txt
./Test_Dir/TEST7.txt
./Test_Dir/TEST8.txt
./Test_Dir/TEST9.txt
./Test_Dir/test_1.txt
./Test_Dir/test_10.txt
./Test_Dir/test_2.txt
./Test_Dir/test_3.txt
./Test_Dir/test_4.txt
./Test_Dir/test_5.txt
./Test_Dir/test_6.txt
./Test_Dir/test_7.txt
./Test_Dir/test_8.txt
./Test_Dir/test_9.txt
```

- `-mmin -/+min` : Search those file which are modified, `-10`: for files modified with in 10 minutes ago OR `+10` for files modified more than 10 minutes ago.
```bash
gphani@icme:~$ touch new_1.txt
gphani@icme:~$ find . -type f -mmin -10
./new_1.txt
# wait for a minute then run below command
gphani@icme:~$ find . -type f -mmin +1 -mmin -5 # combine for time range (more than a minute and less then 5 minutes)
./new_1.txt
```
- `-mtime -/+day` : Similar to `-mmin`, but here it's for days like : `-10` is with in 10 days while `+10` for files modified more than 10 days ago.
- `-amin` or `-atime` : For access minutes or access days
- `cmin` or `ctime` : For changed file, minutes or days.

- `-size -/+size` : This search for files that are above or below certain size, use `M`: Megabytes, `k`: Kilobytes and `G`: Gigabytes
    - Use `-empty` in place of `-size` for files of size 0 bytes (empty) and directories containing no entries.
```bash
gphani@icme:~$ find . -size +5M # List file in PWD with size more than 5 Megabytes
./Website_Demo/public/images/wallpaper.jpg
gphani@icme:~$ find . -empty # List empty files & directorys
```

- `-perm PERMISSION` : List files & directorys with the given permissions.
```bash
gphani@icme:~$ find . -perm 777
./Website_Demo
./Website_Demo/public
./Website_Demo/public/images
./Website_Demo/public/images/background.jpg
./Website_Demo/public/images/header.jpg
./Website_Demo/public/images/wallpaper.jpg
./Website_Demo/public/index.html
./Website_Demo/public/scripts
./Website_Demo/public/scripts/main.js
./Website_Demo/public/scripts/script.js
./Website_Demo/public/static
./Website_Demo/public/static/styles.css
```

- `-exec` : Execute a command for each matched file. Use `{}` as a placeholder for the filename. End with `\;` (for single execution) or `+` for batching.
    - `find . -exec ls {} \;` : suppose 3 files then for each file ls run (i.e. ls file1 ; ls file2 ; ls file3)
    - `find . -exec ls {} +` : ls runs with multiple files (i.e. ls ./file1 ./file2 ./file3)
```bash
gphani@icme:~$ find Website_Demo -type d -exec chmod 775 {} + # change the directorys premission to 775 
gphani@icme:~$ find Website_Demo -perm 775
Website_Demo
Website_Demo/public
Website_Demo/public/images
Website_Demo/public/scripts
Website_Demo/public/static
gphani@icme:~$ find Website_Demo -type f -exec chmod 664 {} + # change the files premission to 664 
gphani@icme:~$ find Website_Demo -perm 664
Website_Demo/.DS_Store
Website_Demo/public/images/background.jpg
Website_Demo/public/images/header.jpg
Website_Demo/public/images/wallpaper.jpg
Website_Demo/public/index.html
Website_Demo/public/scripts/main.js
Website_Demo/public/scripts/script.js
Website_Demo/public/static/styles.css
```

- `-maxdepth` or `-mindepth` : Limit how deep find will search in the directory tree.
```bash
gphani@icme:~$ find . -type f -name "*.jpg" # without mentioning the depth
./pic_1.jpg
./pic_10.jpg
./pic_2.jpg
./pic_3.jpg
./pic_4.jpg
./pic_5.jpg
./pic_6.jpg
./pic_7.jpg
./pic_8.jpg
./pic_9.jpg
./Website_Demo/public/images/background.jpg
./Website_Demo/public/images/header.jpg
./Website_Demo/public/images/wallpaper.jpg
gphani@icme:~$ find . -type f -name "*.jpg" -maxdepth 1 # search only in current directory
./pic_1.jpg
./pic_10.jpg
./pic_2.jpg
./pic_3.jpg
./pic_4.jpg
./pic_5.jpg
./pic_6.jpg
./pic_7.jpg
./pic_8.jpg
./pic_9.jpg
```

- `-delete` : Delete the files & directorys that are in find results, similar to `-exec rm {} +` while `-delete` is more efficient.
```bash
gphani@icme:~$ find . -type f -name "*.jpg" -maxdepth 1 -delete
gphani@icme:~$ ls
My_Project  Test_Dir  Website_Demo  new_1.txt
```

- `-print` : Print matched filenames (the default action in most cases).
```bash
gphani@icme:~$ find . -type d -print # or find . -type d 
# List all directorys in PWD
```