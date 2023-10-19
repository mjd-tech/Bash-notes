# Pause until keypress

Use the read command, do not interpret backslash escapes (-r) do not
echo keystroke (-s), read 1 character without waiting for newline (-n1),
display a prompt (-p).

``` bash
read -rsn1 -p "Press any key to continue..."
```

The -r isn't necessary in this case, but shellcheck will complain if you
don't use it.

Color Prompt

``` bash
# Bold Green prompt. Reset to normal after displaying prompt. Use "ANSI C Quoting"
read -rsn1 -p $'\e[1;32mPress any key to continue...\e[0m'
# Or define the ANSI escape codes in variables
grn=$'\e[32m' bld=$'\e[1m' rst=$'\e[0m'
read -rsn1 -p "${bld}${grn}Press any key to continue...${rst}"
```

As a function, can supply different prompt, or use default prompt

``` bash
# shellcheck disable=SC2120
Pause () { read -rsn1 -p "${1:-Press any key to continue...}"; }
# Shellcheck complains "Pause references arguments, but none are ever passed."
# True, but using ${1:-something} is totally valid Bash code, a common technique to provide a default value when none is given 
```

Display prompt in Bold Green text, erase prompt after user presses key.

``` bash
Pause () {
    echo -en '\e[1;32m' # bold green text
    read -s -n1 -p "${1:-Press any key to continue...}"
    echo -e '\e[0m' # reset text color
    # Move cursor up 1 line, clear to end of line, clear to beginning of line
    tput cuu1; tput el; tput el1
}
```
