# Loops
for, while, until

## for loop
``` bash
# do something with files in current directory
for file in *; do   # * expands to a list of all files, each file is one "word",
    echo "$file"    # even if filename has spaces in it
done

# use a variable. we want word-splitting, variable is unquoted
names="fred barney wilma betty"
for name in $names; do
    echo "$name"
done

# use an array. we don't want word-splitting, one of the items has a space in it.
names=(fred barney 'Mr. Slate')
for name in "${names[@]}"; do
    echo "$name"
done

# C-style for loop
for (( i=1; i<=10; i++ )); do
    echo $i
done

# C loop can have multiple variables,
for (( i=1,j=2; i<=10; i++,j*=2 )); do 
    echo $i $j
done
```

## while loop
``` bash
# infinite loop. Can use : instead of true
while true; do
    echo "infinite loop"
    [[ $foo == bar ]] && break
done

# read lines from a file
while IFS= read -r line; do
    echo "$line"
done < file.txt

# process arguments
while [[ $1 ]]; do
    echo "$1"
    shift
done
```
## until loop
Rarely used. Same as "while ! ..."

``` bash
# keep trying to do a git pull when host is temporarily down.
until git pull &> /dev/null
do
    echo "Waiting for the git host ..."
    sleep 1
done

# same thing using a while loop
while ! git pull &> /dev/null
do
    echo "Waiting for the git host ..."
    sleep 1
done
```

## break and continue
loops can be:
- terminated (broken) by the `break` command, optionally as `break N` to
  break `N` levels of nested loops
- forced to immediately do the next iteration using the `continue`
  command, optionally as `continue N` analog to `break N`

## Piping and redirecting
the do..done block can be piped to another command, or redirected to a file.

``` bash
while true; do
    echo "foo bar"
    echo "foobar foobar"
    break
done | column -t
# result
foo     bar
foobar  foobar
```
## Return status
The return status is that of the last command executed in the do..done
block.
