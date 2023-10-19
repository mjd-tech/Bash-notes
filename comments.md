# Comments
Comment out sections of code

``` bash
# this entire line is commented
echo "Hello World"  # this is commented to end of line
```

## Block comments
Use a "heredoc" fed into the : (no-op) command.
``` bash
: <<'COMMENT'
echo "Hello World"
...more commands...
COMMENT
```

## Inline Comments
For pipe-connected commands, we can add a comment after the pipe `|`
``` bash
echo -e "Aabcbb\nAabcbD\nAabcbb" |  # generate the content
tr a-z A-Z |                        # translate to upper case
sort |                              # sort the text
uniq                                # remove duplicated lines
```

For multi-line commands separated by escaped newline, We can use "backtick command substitution".

``` bash
# this only works with backticks.
echo abc `# put your comment here` \
def `# another comment` \
xyz  # last argument, use regular comment here
```

You must use backticks here. You cannot do this: `$(#comment)`  
This technique is expensive, it creates a subshell for each "inline comment".

Another way: dummy variable, and a "parameter expansion" trick.

``` bash
comment=
echo abc    ${comment# put your comment here} \
def         ${comment# another comment} \
xyz         # last argument, use regular comment here
```
the "parameter expansion" attempts to left-trim the "comment" variable.
Since there is nothing to trim, the variable expands to "nothing" and is ignored.

Note: this is not a good technique to "comment out lines of code"
