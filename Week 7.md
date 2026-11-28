# Loops in Bash
## `break`, `continue`, and their numeric arguments
## for do loop :
Commands are executed once for each item in the list.
- Syntax :
```bash
for var in list
do
commands
done
```
### Loop over a List :
```bash
for item in one two "3 joint" fourth
do
    echo $item
done 
# OR
for file in file_make{01..10}
do
    echo $file
done
# NOTE
start=1
end=5
for i in {$start..$end} # not work
do
    command
done
# Important: Brace expansion is performed by Bash before variable expansion. Therefore, this does not work as a dynamic range:
```
- If we omit the `in` list, Bash uses the positional parameters (`"$@"`).
```bash
for arg
do
    echo "$arg"
done
# This is equivalent to
for arg in "$@" # all positional arguments
do
    echo "$arg"
done
```
### C-style for loop :
Syntax :
```bash
for (( initialization(s); condition; updation(s) ))
do
    commands
done
```
- We can also do multiple initializations and updations, but there is usually one condition, while we can use `&&` (logical AND) and `||` (logical OR).
- We can omit any of the three expressions (initialization, condition, updation), If condition is empty then loops infinitely unless we exit it using `break` or another control mechanism.
```bash
for (( i=0; i<=10; i+=2 )); do echo "$i"; done

for (( i=0, j=15; i<=10 || j>=10; i++, j--))
do
    commands
done
```
### Looping through Arrays :
Syntax :
```bash
# FOR Indexed arrays
for item in "${array_name[@]}" # It expands each array element as a separate word, preserving spaces inside elements.
do
    commands
done
# FOR Associative arrays
for index in "${!array_name[@]}" # gives indices of array
do
    commands
done
# OR
for item in "${array_name[@]}" # directly gives all element
do
    commands
done
```
### Using `IFS` with a for loop :
`IFS` stands for **Internal Field Separator**. It controls how Bash splits unquoted expansions into words, The default value includes (Space, Tab, Newline).
- Split a comma-separated string
```bash
data="apple,mango,orange,banana"
IFS=','
for item in $data
do
    echo "$item"
done
```
- Use a subshell to keep the change local
    - The changed `IFS` does not affect the parent shell.
```bash
data="apple,mango,orange"
(
    IFS=','
    for item in $data
    do
        echo "$item"
    done
)
```
### Looping through command output :
We can use command substitution to generate the list for a for loop.
- Syntax :
```bash
for item in $(command)
do
    commands
done
# Example
for file in $(ls)
do
    echo "$file"
done
```
- Warning: This approach is unsafe for filenames containing spaces, tabs, or newlines. For example, a file named `my report.txt`.

### Looping through Files & Directory using glob patterns :
Using bash wildcards
```bash
for file in *.txt *.pdf *.jpg # OR a single file extensions
do
    echo "$file"
done
```
- By default, `*` does not match hidden files (names beginning with `.`), Enable dotglob using `shopt -s dotglob`.
```bash
shopt -s dotglob

for file in /usr/bin/*
do
    echo "$file"
done
```
### Read stdin using `read` OR `mapfile` :
- `read -a` splits the input according to `IFS` and stores the resulting words in an indexed array.
- Using a custom delimiter with `read`:
```bash
IFS=',' read -r -a fruits <<< "apple,mango,orange" # <<<, this tells is a here-string.

for fruit in "${fruits[@]}"
do
    echo "$fruit"
done
```
- For a file containing one item per line, you can load its lines into an array using `mapfile` (also called `readarray`).
```bash
mapfile -t names < names.txt # without < (arrow) reads from stdin
# name is indexed array
for name in "${names[@]}"
do
    echo "$name"
done
```
- Process substitution makes command output available through a file-like path.
    - Syntax : creates a file-like input source from the command's output.
     ```bash
     <(command)
     ```
    - Example :
    ```bash
    mapfile -t files < <(find . -maxdepth 1 -type f)

    for file in "${files[@]}"
    do
        echo "$file"
    done
    ```
---
## while do loop :
Commands are executed only if condition returns `true`
```bash
while condition
do
commands
done
```


---
## until do loop :
Commands are executed only if condition returns `false`, opposite of while loop.
```bash
until condition
do
commands
done
```


---
## select loop :



---
# Shell Arithmetic
## Shell arithmetic with built-in bash commands & features
Shell arithmetic allows us to perform calculations using integers and variables directly in Bash scripts, using built-in commands & features.

1. Using `$((  ))`:

2. Using `let` : It is a built-in Bash command used to perform arithmetic calculations and assign the result to a variable.
- Syntax:
```bash
let expression
```
- Inside `let`, we can use variable names directly (i.e. need not to use with `$num`, simplly `num` works)
```bash
let a=5+10 # whole expression is passed to let command & result is saved in a
let "a = 5 + 10" # we spaces is given then it should be quoted 
# Because without quotes, spaces separate shell arguments. Quoting keeps the complete arithmetic expression together as one argument to let.
num=10
let a=$1+num # num is variable while $1 is argument passed with script, variable can be used without $variable
```
- **Remember:** let is useful, but for modern Bash scripts, $((...)) is generally easier to read.

3. Using `expr`: Unlike `let`, `expr` is a command that prints the result to standard output, and is another way to perform arithmetic in shell scripting.
- Syntax:
```bash
expr number operator number
```
- Must put space between the numbers and the operator. Because expr treats its arguments separately. It expects the numbers and operator as individual arguments (i.e. Incorrect: `expr 10+20`, Correct: `expr 10 + 20`).
- **`expr` command operators**:

| Expressions | Meaning |
| --- | --- |
| `a + b` | Return arithmetic sum of a and b |
| `a - b` | Return arithmetic difference of a and b |
| `a \* b` | Return arithmetic product of a and b |
| `a / b` | Return arithmetic quotient of a divided by b |
| `a % b` | Return arithmetic remainder of a divided by b |
| `a \> b` | Return 1 if a greater than b; else return 0 |
| `a >= b` | Return 1 if a greater than or equal to b; else return 0 |
| `a \< b` | Return 1 if a less than b; else return 0 |
| `a <= b` | Return 1 if a less than or equal to b; else return 0 |
| `a = b` | Return 1 if a equals b; else return 0 |
| `a \\| b` | Return a if neither argument is null or 0; else return b |
| `a \& b` | Return a if neither argument is null or 0; else return 0 |
| `a != b` | Return 1 if a is not equal to b; else return 0 |
| `str : reg` | Return the position upto anchored pattern match with BRE str |
| `match str reg` | Return the pattern match if reg matches pattern in str |
| `substr str n m` | Return the substring m chars in length starting at position n |
| `index str chars` | Return position in str where any one of chars is found; else return 0 |
| `length str` | Return numeric length of string str |
| `+ token` | Interpret token as string even if its a keyword |
| `\(exprn\)` | Return the value of expression exprn |
- Note : those with `\` need to be escape or passed as literal using single quotes (like `'*'`, `'>'`, `'<'`, `'|'`, `'&'`, `'('`, `')'`) passes that exact string as a literal argument to `expr`, just like escaping with a backslash (`\*`, `\>`, `\<`, `\|`, `\&`, `\(`, `\)`).
- But if we want to store the answer in another variable?, we use command substitution.
```bash
b=$(expr "$a" + 20) # $(  ) runs a command in subshell
```

4. Usinng `$[ expression ]` : `$[ ... ]` is an old Bash arithmetic syntax and is deprecated. we may encounter it in older scripts, but you should use `$(( ... ))` in new scripts.
- Syntax:
```bash
a=10
b=$[ a + 5 ]
echo "$b" # Output: 15
```

5. **Using `$(( expression ))`** : This is the most important & frequently used arithmetic expansion.
- Syntax:
```bash
$(( expression ))
variable=$(( expression ))
```
| Operators | Example | Meaning |
| :--- | :--- | :--- |
| `id++` `id--` | `x=5; echo $(( x++ ))` | Post-increment, post-decrement on variable `id` |
| `++id` `--id` | `x=5; echo $(( ++x ))` | Pre-increment, pre-decrement on variable `id` |
| `+` `-` (unary) | `x=-5; echo $(( -x ))` | Unary plus, unary minus |
| `~` `!` | `echo $(( ~5 ))`, `echo $(( !0 ))` | Bitwise NOT, logical NOT |
| `**` | `echo $(( 2 ** 3 ))` | Exponentiation |
| `*` `/` `%` | `echo $(( 7 * 2 ))`, `echo $(( 7 / 2 ))`, `echo $(( 7 % 2 ))` | Multiplication, integer division, remainder (modulo) |
| `+` `-` (binary) | `echo $(( 10 + 4 ))`, `echo $(( 10 - 4 ))` | Addition, subtraction |
| `<<` `>>` | `echo $(( 3 << 2 ))`, `echo $(( 16 >> 2 ))` | Bitwise left shift, bitwise right shift |
| `<=` `>=` `<` `>` | `echo $(( 5 <= 5 ))`, `echo $(( 5 > 8 ))` | Less than or equal, greater than or equal, less than, greater than |
| `==` `!=` | `echo $(( 5 == 5 ))`, `echo $(( 5 != 5 ))` | Equal to, not equal to |
| `&` | `echo $(( 6 & 3 ))` | Bitwise AND |
| `^` | `echo $(( 6 ^ 3 ))` | Bitwise XOR |
| `\|` | `echo $(( 6 \| 3 ))` | Bitwise OR |
| `&&` | `echo $(( 1 && 0 ))` | Logical AND |
| `\|\|` | `echo $(( 0 \|\| 5 ))` | Logical OR |
| `expr ? true_expr : false_expr` | `x=10; echo $(( x > 5 ? 100 : 200 ))` | Ternary conditional operator |
| `=` `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` `\|=` | `x=4; echo $(( x += 6 ))` | Simple and compound assignment |
| `,` | `echo $(( a=2, b=3, a+b ))` | Comma operator |

- Variable names can bw written normally without $.
- Also, script arguments (`$1`, `$2`, etc.) can also be used directly inside `$(( ... ))`.
    - Make sure arguments used are of numeric type.
    - Operations such as (`$1++`, `++$1`) are invalid.

6. Using `(( expression ))` : 
They look almost identical, but they have different purposes.
- `$((...))` is arithmetic expansion. It calculates a value and substitutes that value into your command.
- `((...))` is an arithmetic command. It evaluates an arithmetic expression directly.
    - It is especially useful when you want to update variables.
```bash
#!/bin/bash
count=1
while (( count <= 5 ))
do
    echo "$count"
    (( count++ ))
done
```

- **NOTE :** Bash supports integer arithmetic directly. It does not normally handle decimal calculations accurately using its built-in arithmetic.

## **Using Bash Calculator `bc`** :
In Bash, `bc`(Basic Calculator) is an arbitrary‑precision command‑line calculator and mini‑language used to perform mathematical operations—including floating‑point arithmetic—that Bash cannot handle natively. 
- It supports operators, variables, functions, and can run interactively or via piped input.
- If directly run `bc` in bash it open Basic calculator in CLI mode, excape using Ctrl+z.
```bash
# Basic Syntax
echo "expression" | bc # Returns the result of expression
# Example
echo "10 + 5" | bc # Output : 15
```
- To calculate decimal values, use `scale` to specify number of decimal points.
```bash
a=10
b=20
echo "scale=2; ($a + $b) / 2" | bc # Output : 15.00
```
- Or using, `bc -l` which loads the standard mathematical library and sets the default scale to 20 decimal places.
```bash
echo "sqrt(25)" | bc -l # Output : 5.00000000000000000000
```
- Supports various operations:
1. Arithmetic operators :

    | Operator | Example | Meaning |
    |---|---|---|
    | `+` | `echo "10 + 5" \| bc` | Addition |
    | `-` | `echo "10 - 5" \| bc` | Subtraction |
    | `*` | `echo "10 * 5" \| bc` | Multiplication |
    | `/` | `echo "10 / 5" \| bc` | Division |
    | `%` | `echo "10 % 3" \| bc` | Modulus (remainder) |
    | `^` | `echo "2 ^ 3" \| bc` | Exponentiation (power) |

2. Comparison Operators :

    | Operator | Example | Meaning |
    |---|---|---|
    | `<` | `echo "5 < 10" \| bc` | Less than |
    | `>` | `echo "10 > 5" \| bc` | Greater than |
    | `<=` | `echo "5 <= 10" \| bc` | Less than or equal to |
    | `>=` | `echo "10 >= 5" \| bc` | Greater than or equal to |
    | `==` | `echo "5 == 5" \| bc` | Equal to |
    | `!=` | `echo "5 != 10" \| bc` | Not equal to |

3. Logical Operators :

    | Operator | Example | Meaning |
    |---|---|---|
    | `!` | `echo "!0" \| bc` | Logical NOT |
    | `&&` | `echo "1 && 1" \| bc` | Logical AND |
    | `||` | `echo "1 || 0" \| bc` | Logical OR |

4. Assignment Operators :

    | Operator | Example | Meaning |
    |---|---|---|
    | `=` | `a=10` | Assign value |
    | `+=` | `a+=5` | Add and assign |
    | `-=` | `a-=5` | Subtract and assign |
    | `*=` | `a*=5` | Multiply and assign |
    | `/=` | `a/=5` | Divide and assign |
    | `%=` | `a%=3` | Modulus and assign |
    | `^=` | `a^=2` | Power and assign |

5. Mathematical Functions (bc -l) :

    | Function | Example | Meaning |
    |---|---|---|
    | `sqrt(x)` | `sqrt(25)` | Square root |
    | `s(x)` | `s(1)` | Sine (radians) |
    | `c(x)` | `c(1)` | Cosine (radians) |
    | `a(x)` | `a(1)` | Arctangent |
    | `l(x)` | `l(10)` | Natural logarithm |
    | `e(x)` | `e(2)` | Exponential function (e^x) |
    | `j(n,x)` | `j(2,1)` | Bessel function of integer order n |

6. Special Features :

    | Feature | Example | Meaning |
    |---|---|---|
    | `scale` | `scale=2; 10/3` | Sets decimal precision for division |
    | `ibase` | `ibase=2; 1010` | Sets input number base |
    | `obase` | `obase=2; 10` | Sets output number base |
    | `length(x)` | `length(12345)` | Number of digits in a number |
    | `scale(x)` | `scale(12.345)` | Number of fractional digits |
    | `read()` | `a=read()` | Reads a value from standard input |

---
# Heredoc feature in Bash
A heredoc (short for here document) lets you pass multiple lines of text or commands as input to another command.
```bash
# Basic Syntax
command <<- MARKER # A hyphen tells bash to *ignore* leading tabs
    line 1
    line 2
    line 3
MARKER
# OR
command << MARKER
line 1
line 2
line 3
MARKER
```
- `<<` : Starts a heredoc; preserves leading tabs and spaces.
- `<<-` : Starts a heredoc; removes leading tabs.
    - Remember :`<<-` removes leading tabs, not ordinary spaces. If you indent the lines using spaces, they will not be removed.
- `MARKER` is a delimiter that tells Bash where the input ends, can be any word: EOF, ABC, END, STOP, etc.
```bash
# Example
a=2.5
b=3.2
c=4

d=$(bc -l << EOF
scale = 5
($a+$b)^$c
EOF
)

echo $d # Output : 1055.6001
```

# Debugging (`set -x`)
