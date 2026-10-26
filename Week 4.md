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
