### 01_01 What's Bash?

```bash
bash --version
echo $SHELL
# change your user's default shell with chsh
echo $PATH
env
```

### 01_02 Pipes and redirection

- `|` -> Data Pipe ( Send data to another application )
- `>` -> Output Redirect ( Redirect the output to another place/file )
    - `>` -> Truncate existing data and rewrite.
    - `>>` -> Append data to the existing data.
- `<` -> Input Redirection
    - `<` -> Single import
    - `<<` -> Called "Here Document"
        - Style 01: `{{APP}} << [Input ends indicator keyword]`. This treats the end keyword as a part of the data.
        - Style 02: `{{APP}} <<- [Input ends indicator keyword]`. This pattern exclude the end keyword from the data.

```bash
cat lorem.txt
clear

# Pipe
cat lorem.txt | less
cat lorem.txt | wc

# Redirect
ls > list.txt
cat list.txt
ls >> list.txt
clear

ls /notreal
ls /notreal 1> output.txt 2>error.txt
cat output.txt
cat error.txt
cat < list.txt
clear

cat << EndOfText
This is a
multiline
text string
EndOfText
```

### 01_03 Bash builtins and other commands

**Built-in commands** are integrated directly into the Bash shell and execute faster since they don't require loading a separate program, while **external commands** are standalone programs that the shell runs, which can offer broader functionality but are slower to execute. Examples of built-ins include `cd` and `echo`, whereas `ls` and `grep` are external commands.
- External Commands executed only if no builtin, function, or alias with the same name exists. ( Built-in commands has the higher priority )

```bash
# echo - ends with a new line character
echo some text

# printf - ends without a new line character
printf hello

command echo hello
builtin echo hello

command -V echo

# Enable external command over built-in
enable -n echo
command -V echo
enable -n

# Reenable built-in command
enable echo
help echo
help -d
```

Side Note: :-p 
- `()` -> Parentheses
- `{}` -> Braces
- `[]` -> Brackets

### 01_05 Bash expansions and substitutions

| Representation    | Name                  |
| :---              | :----:                |
| ~                 | Tilde expansion       |
| {...}             | Brace expansion       |
| ${...}            | Parameter expansion   |
| $(...)            | Command expansion     |
| $((...))          | Arithmetic expansion  |


```bash
# ~ -> Home directory
echo ~
whoami
cd Solutions
cd

# Previous pwd
echo ~-
```

### 01_06 Brace expansion

Brace expansion is a feature in Bash and other shells that allows you to generate a set of strings from a pattern. It's a quick, shorthand way to create lists of filenames or arguments for commands without having to type them all out.

```bash
echo {1..10} # -> 1 2 3 4 5 6 7 8 9 10
echo {10..1} # -> 10 9 8 7 6 5 4 3 2 1
echo {01..10}
echo {01..100}
echo {a..z}
echo {Z..A}
echo {1..30..3}
echo {a..z..2}

touch test_{1..3}.text # -> test_1.txt test_2.txt test_3.txt
touch file_{01..12}{a..d} # file_01a file_01b file_02a file_02b
ls
clear

echo {cat,dog,fox} # -> cat dog fox
echo {cat,dog,fox}_{1..5}
head -n1 {dir1,dir2,dir3}/lorem.txt
```

### 01_07 Parameter expansion

Bash parameter expansion is a feature that allows you to manipulate and retrieve the values of variables and parameters in shell scripts. It includes various operations such as setting default values, checking if a variable is set, and modifying strings.

```bash
# **IMPORTANT** : NO Spaces allowed before and after the equal symbol "=" in bash
greeting="hello there!"

# Retrieve value from a variable
echo $greeting

# ${VARIABLE:START_INDEX:END_INDEX}
echo ${greeting:6} # -> there!
echo ${greeting:6:3} # -> the

# ${VARIABLE/FIRST_MATCH_STRING/REPLACE_STRING}
echo ${greeting/there/everybody}
echo $greeting

# ${VARIABLE//ALL_MATCH_STRINGS/REPLACE_STRING}
echo ${greeting//e/_} # -> h_llo th_r_!
echo ${greeting/e/_} # -> h_llo there!

echo $greeting:4:3 # -> hello there!:4:3
```

| Syntax    | Description                                                                               | Example                                                       | Result |
| :---      | :---                                                                                      | :---                                                          | :--- |
| **`:-`**  | **Default Value**: Use `word` if `parameter` is unset or null.                            | `echo ${NAME:-"Guest"}` (if `NAME` is not set)                | `Guest` |
| **`:=`**  | **Assign Default**: Assign `word` to `parameter` if it's unset or null.                   | `echo ${NAME:="Guest"}` (if `NAME` is not set)                | `Guest` (and `NAME` is now "Guest") |
| **`#`**   | **Remove Shortest Prefix**: Remove the shortest pattern match from the beginning.         | `FILE="/home/user/doc.txt"`; `echo ${FILE#*/}`                | `user/doc.txt` |
| **`##`**  | **Remove Longest Prefix**: Remove the longest pattern match from the beginning.           | `PATH="/a/b/c"` ; `echo ${PATH##*/}`                          | `c` |
| **`%`**   | **Remove Shortest Suffix**: Remove the shortest pattern match from the end.               | `FILE="doc.txt"` ; `echo ${FILE%.*}`                          | `doc` |
| **`%%`**  | **Remove Longest Suffix**: Remove the longest pattern match from the end.                 | `FILE="archive.tar.gz"` ; `echo ${FILE%%.*}`                  | `archive` |
| **`/`**   | **Replace First Match**: Replace the first occurrence of a pattern with `replacement`.    | `TEXT="hello world"` ; `echo ${TEXT/l/L}`                     | `heLlo world` |
| **`//`**  | **Replace All Matches**: Replace all occurrences of a pattern with `replacement`.         | `TEXT="hello world"` ; `echo ${TEXT//l/L}`                    | `heLLo worLd` |
| **`#`**   | **Length**: Get the length of a variable's value. | `TEXT="abc"`; `echo ${#TEXT}`         | `3`                                                           |
| **`:`**   | **Substring**: Extract a substring from a specific `offset` and `length`.                 | `TEXT="abcdef"` ; `echo ${TEXT:1:3}`                          | `bcd` |
| **`^`**   | **Uppercase**: Convert the first character to uppercase.                                  | `TEXT="hello"`; `echo ${TEXT^TEXT}`                           | `Hello` |
| **`^^`**  | **Uppercase All**: Convert all characters to uppercase.                                   | `TEXT="hello"`; `echo ${TEXT^^TEXT}`                          | `HELLO` |
| **`,`**   | **Lowercase**: Convert the first character to lowercase.                                  | `TEXT="HELLO"`; `echo ${TEXT,TEXT}`                           | `hELLO` |
| **`,,`**  | **Lowercase All**: Convert all characters to lowercase.                                   | `TEXT="HELLO"`; `echo ${TEXT,,TEXT}`                          | `hello` |
| **`!`**   | **Indirect Expansion**: Use the value of one variable as the name of another.             | `VAR_NAME="MY_VAR"`; `MY_VAR="Value"`; `echo ${!VAR_NAME}`    | `Value` |

### 01_08 Command substitution

Command expansion, also known as command substitution, is a feature in Bash that allows you to use the output of a command as part of another command. It effectively replaces a command with the text it prints to standard output. This is one of the most powerful and common forms of expansion in shell scripting.

```bash
uname -r # -> 6.8.0-1030-generic
echo "The kernel is $(uname -r)." #-> The kernel is 6.8.0-1030-generic
echo "The Python version is $(python3 -V)."
echo "Result: $(python3 -c 'print("Hello from Python!")' | tr [a-z] [A-Z])" # -> HELLO FROM PYTHON!
```

### 01_09 Arithmetic expansion

Arithmetic expansion is a feature in Bash that evaluates a mathematical expression and replaces it with the calculated result. It's the standard and most robust way to perform integer arithmetic directly within the shell.

```bash
echo $(( 2 + 2 ))
echo $((4-2))
echo $(( 4*5 ))
echo $(( 4 / 5 )) # 0
```

NOTE: Bash can do calculation with integers only. Doesn't support float values