# Read a file, line by line

``` bash
while IFS= read -r line; do
  printf '%s\n' "$line"
done < "$somefile"
```

## `IFS=`
Sets Internal Field Separator to nothing, prevents trimming of leading
and trailing whitespace. This IFS setting will only be active during the
while loop, after the loop is finished, IFS goes back to original value.
If you want the trimming, do not specify IFS=

## `read -r`
prevents backslash interpretation  
For example: Your file uses a backslash newline pair to continue over
multiple lines.  
read -r will treat the trailing backslash as a literal character. It
won't "escape the newline". It won't combine lines.  
Without -r, read will interpret the backslash newline pair, thus reading
two or more lines into your variable.  
You should almost always use the -r option when reading a file.

## `line`
a variable name, chosen by you. You can use any valid shell variable name.

## `< "$somefile"`
read from the file whose name is in the variable "somefile". You can
also use a literal pathname instead of a variable. If your input source
is the contents of a variable/parameter, BASH can iterate over its lines
using a "here string":

``` bash
while IFS= read -r line; do
  printf '%s\n' "$line"
done <<< "$var"
```

## For files that do not end with newline

First, consider the operation of the "while" loop.

- the read command reads a line of the file and sets the variable
- read returns "true". This tells bash to execute the rest of the
  "while" loop, and repeat the loop
- When the end of file (EOF) is reached, read returns false and the loop
  does not execute.

If the last line of the file is not terminated by a newline character:

- the read command will set the variable
- but since there is no "trailing" newline, read will see EOF and return
  "false"
- The loop will not execute
- The last line of the file remains in the variable but is not processed
  by the loop.

You can process this after the loop:

``` bash
while IFS= read -r line; do
  printf '%s\n' "$line"
done < "$file"
[[ -n $line ]] && printf %s "$line"
```

Alternatively, you can simply add a logical OR to the while test:

``` bash
while IFS= read -r line || [[ -n $line ]]; do
  printf '%s\n' "$line"
done < "$file"

# Or, using a pipe:
printf 'line 1\ntruncated line 2' | while read -r line || [[ -n $line ]]; do echo "$line"; done
```
