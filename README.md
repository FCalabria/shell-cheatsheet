# Shell Cheatsheet

* [Commands](#commands)
  + [Chaining commands](#chaining-commands)
* [Writing scripts](#writing-scripts)
  + [Variables](#variables)
  + [Functions](#functions)
  + [Loops and conditionals](#loops-and-conditionals)
  + [Fail checks](#fail-checks)
* [Running scripts](#running-scripts)
* [Resources](#resources)



## Commands

- `cat <filename>` logs a file
- `head -n <number> <filename>` logs the first *number* lines of a file
- `tail -n <number> <filename>` logs the last *number* lines of a file
- `grep <text> <filename>`logs the lines containing *text* on the file
  - `-v --invert-match` logs the lines not containing *text* on the file
  - `-i --ignore-case` matches independently of the case
  - `-E` support for all regex instead of `<text>`
  - `-r` recursively searchs in all files. Use it with a directory instead of *filename*
  
- `tree` logs the directory structure like a tree, including nested
- `sed 's/<oldExpresion>/<newExpresion>/' <filename>` logs the file substituting *oldExpresion* for *newExpresion*
  - `-E` support for all regex instead of `<oldExpresion>`
  - `-i 'subfix'` modify the original file and save the original one adding `subfix` to the name (for example, .old). If no subfix is provided, will not create the backup
- `mv <filename1> <filename2>` renames *filename1* to *filename2*
- `> <filename>` Will write the shell *stdout* to the file (replacing content)
- `2> <filename>` Will write the shell *stderror* to the file (replacing content)
- `&> <filename>` Will write the shell *stdout* and *stderror* to the file (replacing content)
- `>> <filename>` Will append the shell *stdout* to the file
- `echo $?` Will print the exit code of the last command. 0 means no error.

### Chaining commands

- `<command> | <command>` Will run the first *command* and pass its output as the argument for the second *command*
- `<command> ; <command>` Will run both commands, independently of their output (i/ errors)

## Writing scripts

`set -e` The script will exit early if any line fails

`set -o pipefail` The script will mark lines as failed if any piped command on it fails

### Variables

Variables are declared just like this `varname=value` but to be referenced need to have a $ before `$varname`

Double quotes enable variable interpolation, while single quotes don't. Use with care.

If you don't use quotes at all and your variable has whitespaces, it will be interpreted as different arguments.

```shell
myVar=1

echo "This is my var $myVar"
# This is my var 1
echo 'This is my var $myVar'
# This is my var $myVar
```

To assign the output of a command to a variable, you need to encapsulate it like `myVar=$(echo "here")`.

```shell
# This will error
myVar=echo "here"

# This will set myVar to here
myVar=$(echo "here")

```

### Functions

```shell
# Declaration
<functionName>() {
  # Code here
  firstArgument=$1
  # etc
}

# Call
<functionName> <args>

# For example

printSomething(){
  echo "$1 and also $2"
}

printSomething "text" "other text"

# will print
# text and also other text
```


### Loops and conditionals

```shell
for <varname> in <collection>; do
  # Code here
done

# For example
for file in src/*.md; do
  echo "$file"
done
```

If is done to work with standard error codes and if a comand exits with anything but a 0, it will evaluate to false.

We can use the `-q` flag to avoid outputs of anything on the `if` check

```shell
if <command>; then
  # Code here
fi

# For example
if grep -q port "$file"; then
  echo "$file"
fi
```

Or to compare expresions


```shell
if [ <comparison> ]; then
  # Code here
fi

# For example
if [ "$port" -lt 5000 ]; then
  echo "Port is under 5000"
fi
```


```shell
if <condition>; then
  # Code here
else
  # Code here
fi
```

Some comparators:

- `<a> -lt <b>` less than
- `<a> -gt <b>` greater than
- `<a> -eq <b>` numeric equal to
- `-f <filename>` *filename* is a file and exists
- `-z "$<variableName>"` *variableName* has a value

### Fail checks

You can check the number of arguments provided to a script with `$#`.

So a fail check can be

```shell
if [ $# -eq 0 ]; then
  echo "Arguments missing"
  exit 64
fi
```

## Running scripts

`#!/bin/sh` at the begining allows the script to be run writing just `myscript.sh` instead of `sh myscript.sh` on the command line.

For this to work you may need to set the file as executable (`chmod +x <filename>.sh`)

You can pass arguments to a script the same way you pass them to a function

```shell

# Command line
./my-script.sh arg1 arg2

# Script

echo "$1 - $2"

# will print
# arg1 - arg2
```

## Resources

- https://learnxinyminutes.com/docs/bash/
