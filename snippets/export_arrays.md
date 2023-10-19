# Exporting arrays
You can't export a Bash array. You can only export variables and functions.

**Work-arounds**

1.  Temp file
2.  "Serialize" the array into a string variable

## Temp File

Use `declare -p` to dump array definition into temp file. Then `source`
the temp file in the "callback" function.

``` bash
myarr=("Fred" "Barney" "Mr. Slate")
myarr_file=$(mktemp)
export myarr_file
trap 'rm "$myarr_file"' EXIT
declare -p myarr > "$myarr_file"  
yad --blahblah --list --select-action='bash -c "TheFunction %s"' "Hello" "GoodBye"

# Callback function
TheFunction () {
    source "$myarr_file"
    for name in "${myarr[@]}"; do
        echo "$1 $name"
    done
}
export -f TheFunction   

# Result if user clicks Hello in yad dialog
Hello Fred
Hello Barney
Hello Mr. Slate
```

## "Serialize" the array

In this example, serialize the array to tab-separated string, using IFS trick.

``` bash
myarr=("Fred" "Barney" "Mr. Slate")
IFS=$'\t' myvar="${myarr[*]}"   # Note: ${myarr[@]} doesn't work, whether you quote it or not.
export myvar
# to inspect myvar:
# declare -p myvar | cat -T
# result: declare -- myvar="Fred^IBarney^IMr. Slate"
# tabs show up as ^I

yad --blahblah --list --select-action='bash -c "TheFunction %s"' "Hello" "GoodBye"

# Callback function
TheFunction () {
    # "De-serialize" the string back into an array.
    IFS=$'\t' read -r -a myarr <<< "$myvar"
    # use myarr as normal.
    for name in "${myarr[@]}"; do
        echo "$1 $name"
    done
    # Note: we're in a sub-shell here. myarr is local to this subshell.
    # it's just a copy of the "real" myarr.
    # changes made to this myarr are not made to the "real" myarr
}
```

This is one of the rare cases where you use `*` instead of `@` when working with arrays.  
The Bash manual says (paraphrased)  
- `"${myarr[*]}"` expands to a **single word**, elements separated by **first character of IFS**
- `"${myarr[@]}"` expands each element to a **separate word**

The IFS "trick"
- set IFS=$'\t' on **same line** as variable assignment, and the **same line** as the read command.
- This sets IFS **only** for that line of code.
- IFS reverts back to its previous value for the next lines of code


## Which to use

If your script already uses temp files, might as well use the "Temp File" method.
You already have the "trap" code to remove temp files upon EXIT

Otherwise use the "Serialize" method.

- You have to choose a unique separator character that's not in any array elements.
- Usually `$'\t`' (Tab) is a good choice.
- The "best" separator is `$'\037`' the ASCII "Unit Separator": Octal 37, Dec 31, Hex 1F
- It's extremely unlikely this will be in any text you are dealing with, and it was designed for this job.

## Notes
When using yad (Yet Another Dialog) or fzf (FuZzy Finder), you sometimes use "callback" functions

- The script launches yad, you select something in yad and click it
- yad "calls back" to a function in your script, using your selection as argument(s) to the function
- The only way to do this is by exporting the function, and telling yad to run bash -c 'TheFunction %s'
- %s is a yad shorthand for "what you selected"

In practice you export several functions and variables. Sometimes it's easiest to just export everything in
the script, with either one of these:
```bash
set -a
set -o allexport  # self-documenting version
```
allexport will not export arrays, though. Just variables and functions.
