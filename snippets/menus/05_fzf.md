# fzf
- Creates a pop-up menu in the terminal from text piped into it.
- programmable keybindings that can run commands on currently selected item.
- It's not installed by default in most Linux distros, but I always install it on all my systems.

Advantages:
- it just pops up, leaves the screen buffer alone, and goes away when it's done.
- has a `--preview` feature which can run a command on the currently selected item, and display in a split screen.
- can display ANSI sequences in both the menu and the preview pane.
- fuzzy find menu entries
- works with mouse
- don't need to use redirections like Dialog and Whiptail
- can also use it for a pop-up input box

Disadvantages:
- fairly steep learning curve. This is offset by good documentation and numerous examples
- needs a lot of arguments to use to its full potential. 

## FZF_DEFAULT_OPTS
These options will be automatically applied when you run fzf. Can be over-ridden. Example:

``` bash
export FZF_DEFAULT_OPTS="--no-info --reverse --height=~50% --cycle"
```

However, when writing scripts it's often easier to just put the fzf
options in an array and pass the array as argument to fzf.

## Menu function
This generates a fzf menu which executes a Bash function based on the menu selection.
This technique avoids the tedious "case" statement by passing in the function names  in addition to the menu items.

Func1 Func2 etc can also be simple commands like _exit_, _return_, or even something like _cat /etc/hosts_,
but don't try to embed quotes in the commands. The functions do not need to be exported

``` bash
Menu () {
    # Usage: Menu "Title" "Item 1" "Func1" "Item 2" "Func2"...
    local choice
    local -a fzf_opts
    fzf_opts=(  --no-info --reverse --height=~50% --cycle" --border --border-label-pos=3
                --border-label="$1" --delimiter=$'\t' --with-nth=1
    )
    shift
    choice=$(
        while [[ $1 ]]; do
            echo -e "${1}\t${2}"
            shift 2
        done | fzf "${fzf_opts[@]}"
    )
    [[ $choice ]] || return 1
    # We intentionally use unquoted variable expansion
    # shellcheck disable=SC2086
    ${choice#*$'\t'}
}

# Functions
Func1 () { echo "This is Func1"; }
Func2 () { echo "This is Func2"; }
```
Note: Shellcheck complains about unquoted variable expansions, but in this case we do want word-splitting to occur.

Example: A main menu, exits if user selects Quit or hits Esc key.

``` bash
while true; do
    Menu "Main Menu" \
        "IP address"        IpAddr  \
        "Kernel info"       Kernel  \
        "RAM usage"         Ram     \
        "Quit"              exit    || exit
done
```
Notes:
- Needs the trailing `|| exit` in case user hits Esc



## Input box
```bash    
name="$( echo '' | fzf --query="${name:-}" --height=1 --border --no-info \
    --print-query --pointer=' ' --color=bg+:-1 --prompt="Enter your name: ")"
[[ $result ]] || do_something

```
- set pointer to space, cannot be null.
- set "current line background" to -1 (default terminal color).
- border is optional
- height will always be at least 2, specifying 1 means "as small as you can"