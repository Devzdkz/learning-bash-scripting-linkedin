### 02_01 Understanding Bash script syntax

```bash
nano myscript.sh

#!/usr/bin/env bash

# Tell the user hello!
echo Hello!

# Bash scripts execution
bash myscript.sh
chmod +x myscript.sh

./myscript.sh
```

### 02_02 Displaying text with 'echo'

Bash treats single quotes and double quotes differently.

```bash
echo hello
echo hello world

worldsize=big
echo hello $worldsize world

echo "The kernel is $(uname -r)"    # This is ok. Recommended!
echo The kernel is $(uname -r)      # This is ok
echo The (kernel) is $(uname -r)    # This throws errors because of the (kernel) part
echo The \(kernel\) is $(uname -r)  # This is ok

echo 'The kernel is $(uname -r)'    # OUTPUT: The kernel is $(uname -r)
echo "The (kernel) is $(uname -r)"  # OUTPUT: The (kernel) is 6.8.0-1030-generic
echo "The (kernel) is \$(uname -r)" # OUTPUT: The (kernel) is $(uname -r)
clear

echo
echo; echo "More space!"; echo

# -n removes the new line character
echo -n "No newline!"
echo -n "Part of"; echo -n " a statement"
echo -n "Part of"; echo " a statement"
```

### 02_03 Working with variables

```bash
mygreeting=Hello
mygreeting = Hello
mygreeting2="Good morning"
number=16
echo $mygreeting
echo $mygreeting2
echo $number
```

```bash
nano myscript.sh

#!/usr/bin/env bash

myvar="Hello!"
echo "The value of the myvar variable is: $myvar"
myvar="Bonjour!"
echo "The value of the myvar variable is: $myvar"

declare -r myname="Scott"
echo "The value of the myname variable is: $myname"
myname="Michael"
echo "The value of the myname variable is: $myname"

declare -l lowerstring="This is some TEXT!"
echo "The value of the lowerstring variable is: $lowerstring"
lowerstring="Let's CHANGE the VALUE!"
echo "The value of the lowerstring variable is: $lowerstring"

declare -u upperstring="This is some TEXT!"
echo "The value of the upperstring variable is: $upperstring"
upperstring="Let's CHANGE the VALUE!"
echo "The value of the upperstring variable is: $upperstring"

./myscript.sh
```

```bash
declare -p
echo $USER
```

### 02_04 Working with numbers

```bash
echo $(( 4 + 4 ))
echo $(( 8 - 5 ))
echo $(( 2 * 3 ))
echo $(( 8 / 4 ))
echo $(( (3+6) - 5 * (5*2) ))
a=3
((a+=3))
echo $a
((a++))
echo $a
((a++))
echo $a
((a--))
echo $a
(($a++))
clear
((a+=3))
((a-=3))
((a*=2))
((a/=2))
echo $a
a=$a+2
echo $a
declare -i b=3
b=$b+4
echo $b
echo $(( 1 / 3 ))
echo "1/3" | bc
echo "scale=3; 1/3" | bc
declare -i c=1
declare -i d=3
e=$(echo "scale=3; $c/$d" | bc)
echo $e
echo $RANDOM
echo $RANDOM
echo $RANDOM
echo $RANDOM
echo $RANDOM
echo $(( RANDOM % 10 ))
echo $(( 1 + RANDOM % 10 ))
echo $(( RANDOM % 2 ))
```

### 02_05 Comparing values with test

```bash
help test | less
[ -d ~ ]
echo $?
[ -d /bin/bash ]; echo $?
[ -d /bin ]; echo $?
[ "cat" = "dog" ]; echo $?
[ "cat" = "cat" ]; echo $?
pet=cat
[ $pet = "cat" ]; echo $?
pet=dog
[ $pet = "cat" ]; echo $?
clear
[ 4 -lt 5 ]; echo $?
[ 4 -lt 3 ]; echo $?
[ ! 4 -lt 3 ]; echo $?
(( 4 > 3 )); echo $?
[ $pet = "dog" -a -d ~ ]; echo $?
[ $pet = "dog" -o -d ~ ]; echo $?
[ $pet = "cat" -o -d ~ ]; echo $?
```

### 02_06 Comparing values with extended test

```bash
[[ 4 -lt 3 ]]; echo $?
[[ -d ~ && -a /bin/bash ]]; echo $?
[[ -d ~ && -a /bin/mash ]]; echo $?
[[ -d ~ || -a /bin/bash ]]; echo $?
[[ -d ~ ]] && echo ~ is a directory
[[ -d /bin/bash ]] && echo /bin/bash is a directory
ls && echo "listed the directory"
clear
true && echo "success!"
false && echo "success!"
[[ "cat" =~ c.* ]]; echo $?
[[ "bat" =~ c.* ]]; echo $?
```

### 02_07 Formatting and styling text output

```bash
echo -e "Name\t\tNumber"; echo -e "Scott\t\t123"
echo -e "This text\nbreaks over\nthree lines"
echo -e "\a"
```

```bash
nano myscript.sh

#!/usr/bin/env bash
echo -e "\033[33;44mColor Text\033[0m"
echo -e "\033[30;42mColor Text\033[0m"
echo -e "\033[41;105mColor Text"
echo "some text that shouldn't have styling"
echo -e "\033[0m"
echo "some text that shouldn't have styling"
echo -e "\033[4;31;40mERROR:\033[0m\033[31;40m Something went wrong.\033[0m"

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

ulinered="\033[4;31;40m"
red="\033[31;40m"
none="\033[0m"

echo -e $ulinered"ERROR:"$none$red" Something went wrong."$none

./myscript.sh
```

### 02_08 Formatting output with printf

```bash
echo "The results are: $(( 2 + 2 )) and $(( 3 / 1 ))"
printf "The results are: %d and %d\n" $(( 2 + 2 )) $(( 3 / 1 ))

printf "The results is: %d\n" $(( 2 + 2 )) $(( 3 / 1 ))
```

```bash
nano myscript.sh

#!/usr/bin/env bash

echo "----10----| --5--"

echo "Right-aligned text and digits"
printf "%10s: %5d\n" "A Label" 123 "B Label" 456

echo "Left-aligned text, right-aligned digits"
printf "%-10s: %5d\n" "A Label" 123 "B Label" 456

echo "Left-aligned text and digits"
printf "%-10s: %-5d\n" "A Label" 123 "B Label" 456

echo "Left-aligned text, right-aligned and padded digits"
printf "%-10s: %05d\n" "A Label" 123 "B Label" 456

echo "----10----| --5--"

./myscript.sh
```

```bash
nano myscript.sh

printf "%(%Y-%m-%d %H:%M:%S)T\n" 1746020520
date +%s
date +%Y-%m-%d\ %H:%M:%S
printf "%(%Y-%m-%d %H:%M:%S)T\n" $(date +%s)
printf "%(%Y-%m-%d %H:%M:%S)T\n"
printf "%(%Y-%m-%d %H:%M:%S)T is %s\n" -1 "the time"

./myscript.sh
```

### 02_09 Working with arrays

```bash
snacks=("apple" "banana" "orange")
declare -a snacks=("apple" "banana" "orange")
echo ${snacks[2]}
echo ${snacks[1]}
echo ${snacks[0]}
snacks[5]="grapes"
snacks+=("mango")
echo ${snacks[@]}
for i in {0..6}; do echo "$i: ${snacks[$i]}"; done
echo ${snacks[@]: -1}
declare -A office
office[city]="San Francisco"
office["building name"]="HQ West"
echo ${office["building name"]} is in ${office[city]}
```
