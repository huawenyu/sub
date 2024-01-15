# Doc
https://github.com/abhi-72/Linux-Shell-Scripting
https://tldp.org/LDP/abs/html/internalvariables.html
https://stackoverflow.com/questions/918886/how-do-i-split-a-string-on-a-delimiter-in-bash

# comments
https://stackoverflow.com/questions/43158140/way-to-create-multiline-comments-in-bash/43158193

```sh
: '
This is multiline comments:
foo
far
'
  #Special Variables
echo Hello World
echo Our shell name is :$BASH
echo Our shell version is :$BASH_VERSION
echo Our home directory :$HOME
echo Our present working directory :$PWD

  #Declaring Variable
name=MARK
echo The name is :$name

  #Read from input
echo Enter Username:
read user
echo Enter Password:
read -s pass
echo Username: $user, Password: $pass
  #Prompt user input in the same line
read -p 'Username : ' user
read -sp 'Password : ' pass
echo Entered, $user:$pass

  #Array and Inbuilt REPLY
echo 'Enter Array1 names : '
read -a names
echo ${names[0]}
echo 'Enter Array2 names : '
read -p 'Inbuilt REPLY value : '
echo $REPLY
```
# variable declare

https://unix.stackexchange.com/questions/254367/in-bash-scripting-whats-the-different-between-declare-and-a-normal-variable

# arg

```sh

  ### This is multiline comments
: '
>>> {fileout} foo bar
'

  #arguments from command line and number of arguments
echo 'Number of arguments:' $#
echo 'Your arguments:'
args=("$@")
echo "  args=${args[@]}"
echo "  args by index="${args[0]} ${args[1]}
```

# function

```sh
 ## function's name and arguments
function foo(){
  echo "Function name:  ${FUNCNAME}"
  echo "The number of positional parameter : $#"
  echo "All parameters or arguments passed to the function: '$@'"
  echo
}

foo bar
```

# if-else

```sh
  # If Syntax
  #if []
  #then
  #    statement
  #fi

count=11
name='abhi'

if [ $count -eq 10 ]
then
    echo Equals to 10
elif [ $count -gt 10 ]
then
    echo greater than 10
else
    echo Not Equals to 10
fi

if [ $name = 'adbhi' ]
then
    echo Equals to abhi
else
    echo Not Equals to abhi
fi
```

# case

```sh
  #Syntax of Case
  #case expression in
  #    pattern1 )
  #        statements ;;
  #    pattern2 )
  #        statements ;;
  #    ...
  #esac

echo -e "Enter a character"
read char

case $char in
    [a-z] )
        echo "Entered character is an alphaber (smaller case)" ;;
    [A-Z] )
        echo "Entered character is an alphaber (Bigger case)" ;;
    [0-9] )
        echo "Entered character is number" ;;
    ? )
        echo "Entered character is Special Character" ;;
    * )
        echo "Input exceeds character length" ;;
esac
```

# loop

```sh

  #Syntax
  #until [ condition ]
  #do
  #    command1
  #    command2
  #    ...
  #done

  #for i in 1 2 3 4 5
  #for i in {1..5}
  #{start..end..increment}
  #for i in {1..10..2}

for (( i=1; i<=5; i++ ))
do
	echo $i
done


for item in *
do
    if [ -f $item ]
    then
        echo $item
    fi
done



  # while
echo -e "Enter start value : \c"
read start
echo -e "Enter limit value : \c"
read limit 
if [ $start -gt $limit ]
then 
    echo "Invalid start and limit values"
else
    while [ $start -le $limit ]
    do
        echo "$start"
        #start=$(( start +1 ))
        (( ++start ))
    done
fi

```



# readfile

```sh
  #Input Re-direction
  #while read p
  #do
  #    echo $p
  #done < readfile.sh

  #cat readfile.sh | while read p
  #do
  #    echo $p
  #done

  #Internal field Seperator -> how to do word splitting
while IFS= read -r line
do
    echo $line
done < readfile.sh
```

# return value

## succ with 0

```sh
echo "this will work"
RESULT=$?
if [ $RESULT -eq 0 ]; then
  echo success
else
  echo failed
fi

if [ $RESULT == 0 ]; then
  echo success 2
else
  echo failed 2
fi
```

## echo vs return

https://stackoverflow.com/questions/51755356/whats-the-difference-between-echo-and-return-in-bash
https://superuser.com/questions/1320691/print-echo-and-return-value-in-bash-function

You're conflating two separate things: output and exit status.
- `echo` generates `output`. A command substitution like `$(...)` captures that output, but if you run a command without it, that output will go to the terminal.
- `return` sets `exit status`. This is what's used to determine which branch is taken when running if your_function; then ..., or to populate `$?`.

```sh
myfunc() {
    echo "This is output"
    return 3
}

myfunc_out=$(myfunc); myfunc_rc=$?
echo "myfunc_out is: $myfunc_out"
echo "myfunc_rc is: $myfunc_rc"
```

# write file

```sh
echo -e "Enter filename : \c"
read file_name
ext=0

write_to_file(){
	if [ -f $file_name ]
	then
		echo "$file_name already exists."
		echo "Do you want to append (A) or override (O) or quit (Q)"
		echo -e "Type (A/O/Q) : \c"
		read a_o
		if [[ "$a_o" = "A" ]]
		then
			echo "You choose to Append"
			echo "Enter your data and press ctrl+d to save and exit"
			cat >> $file_name
			ext=1
		elif [[ "$a_o" = "O" ]]
		then 
			echo "You choose to Override"
			echo "Enter your data and press ctrl+d to save and exit"
			cat > $file_name
			ext=1
		elif [[ "$a_o" = "Q" ]]
		then
			exit
		else
			echo "You have choosen wrong option"
		fi
	else
		echo "$file_name doesn't exist"
		echo -e "Do you want to create a new file $file_name (Y/N) : \c"
		read y_n
		if [[ "$y_n" = "Y" ]]
		then
			echo "You choose to Create"
			echo "Enter your data and press ctrl+d to save and exit"
			cat > $file_name
			ext=1
		elif [[ "$y_n" = "N" ]]
		then 
			echo "You choose not to create"
			ext=1
		else
			echo "You have choosen wrong option"
		fi
	fi 
}

while [ $ext -eq 0 ]
do
	write_to_file 
done
```

# Howto

## Check var exist

```sh
    ### https://stackoverflow.com/questions/3601515/how-to-check-if-a-variable-is-set-in-bash
    if [ -z ${var+x} ]; then echo "var is unset"; else echo "var is set to '$var'"; fi
```

## check var string beginwith

```sh
[[ $a == z* ]]   # Should be this one: True if $a starts with a "z" (wildcard matching).
[[ $a == "z*" ]] # True if $a is equal to z* (literal matching).

 # Or case like
case $HOST in node*)
    : # Your code here
esac

 # Or regex match
if [[ "$HOST" =~ ^user.* ]]; then
    echo "yes"
fi

```

