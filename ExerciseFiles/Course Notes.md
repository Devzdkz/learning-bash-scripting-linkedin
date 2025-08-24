# Course Notes for _Learning Bash Scripting_

This document contains links and code snippets for the LinkedIn Learning course _Learning Bash Scripting_ with Scott Simpson.

## Links and other information

### GitHub repository for this course

Download the files used in this course from [github.com/scottsimpson/learning-bash-scripting](https://github.com/scottsimpson/learning-bash-scripting).

### Related LinkedIn Learning courses

[Learning VirtualBox](https://www.linkedin.com/learning/learning-virtualbox-19862434)

[Learning Linux Command Line](https://www.linkedin.com/learning/learning-linux-command-line-14447912)

[Learning Windows Subsystem for Linux](https://www.linkedin.com/learning/learning-windows-subsystem-for-linux-16134127)

### GitHub Codespaces information

Learn more about [GitHub Codespaces](https://github.com/features/codespaces).

Learn more about [billing and usage](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-codespaces/about-billing-for-github-codespaces) for GitHub Codespaces.

## Commands used in the course


### 03_01 Conditional statements with the 'if' keyword

```bash
nano myscript.sh

#!/usr/bin/env bash

declare -i a=3

if [[ $a -gt 4 ]]
then
    echo "$a is greater than 4!"
else
    echo "$a is not greater than 4!"
fi

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

declare -i a=3

if (( $a > 4 ))
then
    echo "$a is greater than 4!"
else
    echo "$a is not greater than 4!"
fi

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

declare -i a=3

if (( $a > 4 ))
then
    echo "$a is greater than 4!"
elif (( $a > 2 ))
then
    echo "$a is greater than 2"
else
    echo "$a is not greater than 4!"
fi

./myscript.sh
```

### 03_02 Working with while and until loops

```bash
nano myscript.sh

#!/usr/bin/env bash

echo "While Loop"
declare -i n=0
while ((n<10))
do
    echo "n:$n"
    ((n++))
done

echo -e "\nUntil Loop"
declare -i m=0
until ((m==10)); do
    echo "m:$m"
    ((m++))
done

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

echo "While Loop"
declare -i n=0
while ((n<10))
do
    echo "n:$n"
    ((n++))
done

echo -e "\nUntil Loop"
declare -i m=0
until ((m>m)); do
    echo "m:$m"
    ((m++))
done

./myscript.sh
# Interrupt the program with Ctrl-C
```

### 03_03 Introducing 'for' loops

```bash
nany myscript.sh

#!/usr/bin/env bash

for i in 1 2 3
do
    echo $i
done

./myscript.sh
```

```bash
for i in 1 2 3; do echo $i; done
```

```bash
for i in {1..100}; do echo $i; done
```

```bash
nano myscript.sh

#!/usr/bin/env bash

for (( i=1; i<=10; i++ ))
do
    echo $i
done

./myscript.sh

clear
```

```bash
declare -a fruits=("apple" "banana" "cherry")
for i in ${fruits[@]}; do echo $i; done
```

```bash
nano myscript.sh

#!/usr/bin/env bash

declare -A arr
arr["name"]="scott"
arr["id"]="1234"

for i in "${!arr[@]}"
do
    echo $i: ${arr[$i]}"
done

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

for i in $(ls)
do
    echo "Found a file: $i"
done

./myscript.sh

ls
```

```bash
nano myscript.sh

#!/usr/bin/env bash

for i in *
do
    echo "Found a file: $i"
done

./myscript.sh
```

#### 03_04 Selecting behavior using 'case'

```bash
nano myscript.sh

#!/usr/bin/env bash

animal="dog"
case $animal in
    bird) echo "Avian";;
    dog|puppy) echo "Canine";;
    *) echo "No match!";;
esac

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

animal="bird"
case $animal in
    bird) echo "Avian";;
    dog|puppy) echo "Canine";;
    *) echo "No match!";;
esac

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

animal="elephant"
case $animal in
    bird) echo "Avian";;
    dog|puppy) echo "Canine";;
    *) echo "No match!";;
esac

./myscript.sh
```

### 03_05 Using functions

```bash
nano myscript.sh

#!/usr/bin/env bash

greet() {
    echo "Hi there!"
}

echo "And now, a greeting!"
greet

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

greet() {
    echo "Hi $1!"
}

echo "And now, a greeting!"
greet Scott

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

greet() {
    echo "Hi, $1! What a nice $2."
}

echo "And now, a greeting..."
greet Scott Morning
greet Everybody Evening

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

numberthings() {
    i=1
    for f in "$@"; do
        echo "$i: $f"
        ((i+=1))
    done
}

numberthings *
echo
numberthings pine birch maple spruce

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

numberthings() {
    i=1
    for f in "$@"; do
        echo "$i: $f"
        ((i+=1))
        echo "This output brought to you by $FUNCNAME!"
    done
}

numberthings *
echo
numberthings pine birch maple spruce

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

var1="I'm variable 1"

myfunction() {
    var2="I'm variable 2"
    local var3="I'm variable 3"
}
myfunction

echo $var1
echo $var2
echo $var3

./myscript.sh
```

### 03_06 Reading and writing text files

```bash
nano myscript.sh

#!/usr/bin/env bash

for i in 1 2 3 4 5
do
    echo "This is line $i" > ~/textfile.txt
done

./myscript.sh
cat textfile.txt
```

```bash
nano myscript.sh

#!/usr/bin/env bash

for i in 1 2 3 4 5
do
    echo "This is line $i" >> ~/textfile.txt
done

rm textfile.txt
./myscript.sh
cat textfile.txt
```

```bash
nano myscript.sh

#!/usr/bin/env bash

while read f
    do echo "I read a line an it says: $f"
done < ~/textfile.txt

./myscript.sh
```

### 04_01 Working with arguments

```bash
nano myscript.sh

#!/usr/bin/env bash

echo "The $0 script got the argument: $1"

./myscript.sh Apple
```

```bash
nano myscript.sh

#!/usr/bin/env bash

echo "The $0 script got the argument: $1"
echo "Argument 2 is $2"

./myscript.sh Apple Orange
./myscript.sh "Green Apple" Orange
```

```bash
nano myscript.sh

#!/usr/bin/env bash

for i in "$@"
do
    echo $i
done

echo "There were $# arguments."

./myscript.sh apple orange
./myscript.sh apple orange kiwi lemon
./myscript.sh "apple orange" kiwi lemon
```

### 04_02 Working with options

```bash
nano myscript.sh

#!/usr/bin/env bash

while getopts u:p: option; do
    case $option in
        u) user=$OPTARG;;
        p) pass=$OPTARG;;
    esac
done

echo "user: $user / pass: $pass"

./myscript.sh -u scott -p secret
./myscript.sh -p secret -u scott
```

```bash
nano myscript.sh

#!/usr/bin/env bash

while getopts u:p:ab option; do
    case $option in
        u) user=$OPTARG;;
        p) pass=$OPTARG;;
        a) echo "got the A flag";;
        b) echo "got the B flag";;
    esac
done

echo "user: $user / pass: $pass"

./myscript.sh -u scott -p secret
./myscript.sh -u scott -p secret -a
./myscript.sh -u scott -p secret -a -b
./myscript.sh -u scott -p secret -b
```

```bash
nano myscript.sh

#!/usr/bin/env bash

while getopts :u:p:ab option; do
    case $option in
        u) user=$OPTARG;;
        p) pass=$OPTARG;;
        a) echo "got the A flag";;
        b) echo "got the B flag";;
        ?) echo "I don't know what $OPTARG is!";;
    esac
done

echo "user: $user / pass: $pass"

./myscript.sh -u scott -p secret -b -z
```

### 04_03 Getting input during execution

```bash
nano myscript.sh

#!/usr/bin/env bash

echo "What is your name?"
read name
echo "What is your password?"
read -s pass

read -p "What's your favorite animal? " animal

echo "name: $name, pass: $pass, animal: $animal"

./myscript.sh

help read
```

```bash
nano myscript.sh

#!/usr/bin/env bash

select animal in "bird" "dog" "quit"
do
    echo "You selected $animal!"
    break
done

./myscript.sh
```

```bash
nano myscript.sh
#!/usr/bin/env bash

select animal in "bird" "dog" "quit"
do
    case $animal in
        bird) echo "Birds like to fly.":;
        dog) echo "Dogs like to play catch.";;
        quit) break;;
        *) echo "I'm not sure what that is.";;
    esac
done

./myscript.sh
```

### 04_04 Ensuring a response

```bash
nano myscript.sh

#!/usr/bin/env bash

read -ep "Favorite color? " -i "Blue" favcolor
echo $favcolor

./myscript.sh
./myscript.sh
./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

if (($#<3)); then
    echo "This command requires three arguments:"
    echo "username, userid, and favorite number."
else
    # the program goes here
    echo "username: $1"
    echo "userid: $2"
    echo "favorite number: $3"
fi

./myscript.sh
./myscript.sh scott 1234
./myscript.sh scott 1234 blue
```

```bash
nano myscript.sh

#!/usr/bin/env bash

read -p "Favorite animal? " fav
while [[ -z $fav ]]
do
        read -p "I need an answer! " fav
done

echo "$fav was selected."

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

read -p "Favorite animal? [cat] " fav
if [[ -z $fav ]]; then
        fav="cat"
fi

echo "$fav was selected"

./myscript.sh
./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

read -p "What year? [nnnn] " year
until [[ $year =~ ^[0-9]{4}$ ]]; do
        read -p "A year, please! [nnnn] " year
done
echo "Selected year: $year"

./myscript.sh
```
