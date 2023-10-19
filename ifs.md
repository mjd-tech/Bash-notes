# IFS Tricks
IFS (Internal Field Separator) is a Bash environment variable.
By default it is "space tab newline".  
Bash will recognize either of those 3 characters as an IFS.  
When you ask bash for a list, bash will use the first IFS character, space by default

```bash
arr=( Fred Barney "Mr. Slate" )
echo "${arr[*]}"    # OLD IFS
Fred Barney Mr. Slate

OLDIFS=$IFS
IFS=$'\n'

echo "${arr[*]}"    # NEW IFS, each element separated by newline
Fred
Barney
Mr. Slate
IFS=$OLDIFS         # Remember to set it back!

# find out what the IFS is with xxd (hexdump)
echo -n "$IFS" | xxd
00000000  20 09 0a             | ..|
0000003
# 20=space 09=tab 0a=newline

# Set it back to default, if you didn't save the original IFS.
IFS=$' \t\n'
```
