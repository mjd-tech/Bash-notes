# Pause until keypress

Use the read command, do not interpret backslash escapes (-r) do not
echo keystroke (-s), read 1 character without waiting for newline (-n1),
display a prompt (-p).

``` bash
read -rsn1 -p "Press any key to continue..."
```

The -r isn't necessary in this case, but `shellcheck` will complain if you
don't use it.

Color Prompt

``` bash
# Bold Green prompt. Reset to normal after displaying prompt. Use "ANSI C Quoting"
read -rsn1 -p $'\e[1;32mPress any key to continue...\e[0m'

# Or define the ANSI escape codes in variables
grn=$'\e[32m' bld=$'\e[1m' rst=$'\e[0m'
read -rsn1 -p "${bld}${grn}Press any key to continue...${rst}"
```

As a function, can supply different prompt, or use default prompt.

``` bash
# shellcheck disable=SC2120
Pause () { read -rsn1 -p "Press any key to ${1:-continue...}"; }

# Shellcheck complains "Pause references arguments, but none are ever passed."
# True, but using ${1:-something} is totally valid Bash code, a common technique to provide a default value when none is given 
```

Display prompt in Bold Green text, erase prompt after user presses key.

``` bash
# shellcheck disable=SC2120
rst='\e[0m' grn='\e[1;32m' 
Pause () {
    echo -en "${grn}Press any key to ${1:-continue}...${rst}"
    read -rsn1
    # clear to: end of line, beginning of line; move cursor to beginning of line
    tput el; tput el1; echo -en "\r"
}
```