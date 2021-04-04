# BASH

- Variables must begin with a letter and can contain alphanumerics and understrokes.
- If no arguments are passed to the `for` loop it takes the arguments passed to script.
- Returns must be betwen 0 and 255.
- `\` can be used at the end to write a command on multiple lines
- **When `cat` sees the string `-` as a filename, it treats it as a synonym for `stdin`. Example:** A program has a buffer overflow vulnerability. When it is exploited, it opens a shell then closes it immediately. Some `cat and dash` can be used to keep the shell open.

```bash
cat <(python -c "print 'A'*40 + '\xef\xbe\xad\xde'") - | ./executable
```

```bash
#!/bin/bash

$$ # id of the shell being executed
$# # number of arguments
$n # argument number n
$* # all arguments except the script name in one string
$@ # all arguments except the script name in multiple strings
$? # return value
env || printenv ## print list of exported variables
set # print list of declared variables
unset # delete environment variable
shift n # shifts args to the left by n steps

basename # returns the last part of the path
$> # redirect stderr
#substitute a command with it execution's result
$(command)
`command`

### Variable assignment ###
let i = 10
$(( i = 10 ))
$[ i = 10 ]

### String manipulation ###
${variableName/old/new} #replace first occurance
${variableName//old/new} #replace all
${var^} # make first letter uppercase
${var^^} # make all letters uppercase
${var,} # make first letter lowercase
${var,,} # make all letters lowercase
${var~} # inverse first letter capitalization
${var~~} # invers all letters
tr a-z A-Z # translate to capital letters
tr -d ' ' # delete spaces in a string
tr -s ' ' # remove repeated spaces (only leave one)
# Sub value of variable
${name:offset:length}

${varname:?message}	# if variable is unset, print error
${variable:-value}	# if variable is unset, value is used
${variable:=value}	# if variable is unset, value is used and variable=value
# This tries to execute value, to avoid the execution 
: ${variable:=value}

touch file{1,2,3} # creates file1, file2 and file3

echo -n # delete trailing new line
echo -e # allow interpretation of backslash escapes
echo '$var' # prints $var
cut -d'.' -f1 # first field before a dot

### Comparisons ###
test value1 = value2 # (10 = "10" is true) 
test value1 == value2 # (10 == "10" is false)
-eq -ne -lt -le -gt -ge
-n # not empty
-z # zero length or empty
-e # file exsists
-f # file exists and is a file
-d # file exists and is a directory
-s # file exists and has size > 0
-r|w|x # test user permissions
-a # logical AND
-o # logical OR
-nt # file1 is older than file2

### Input ###
read -n # return after reading NCHARS characters rather than waiting
		# for a newline, but honor a delimiter if fewer than
    	# NCHARS characters are read before the delimiter
	-t # timeout after TIMEOUT seconds
	-s # do not take input from terminal
	-p # PROMPT

### Loops ###
if command
	then
		commands # : if no command
	else
		commands
fi
if command
	then
		commands # : for nothing
	elif command
	then
		commands
fi
#############
case $value in
val1 | val2 | val3)
	#code
	;;
*)
	#code
	;;
esac
# putting ;;& makes script search for other valid cases
# putting ;& makes script execute next cases I think
#############
for i in list
# for i in {start..end..step}
# for i in $(seq start step end)
do
	#commands
done
#############
#infinite for loop
for (( ; ; ))
do
	#commands
done
#############
while condition # : for nothing
do
	commands
done
#############
until condition # : for nothing
do
	commands
done
```