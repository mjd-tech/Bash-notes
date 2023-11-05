# Whiptail, Dialog
- These produce various TUI dialog boxes, including menus
- Whiptail is usually installed by default on most Linux distros. But it's buggy, and doesn't work with the mouse
- Dialog is a better choice: more features, mouse works, less bugs, but not always installed by default

Advantages:
- Produces nice looking menus, with colors
- Automatically deals with user resizing the terminal

Disadvantages:
- Fussy syntax, menus need "tags" and "items", output goes to stderr and requires redirection tricks
- Menus are full screen, and clutter the screen buffer.
- If you launch cli commands that produce output, the output is displayed without the fancy colors of the menu.
- This can be resolved somewhat by displaying output in a dialog box, but these boxes don't display ANSI escape sequences.

## Capturing user input

Whiptail and Dialog send output to **stderr** not **stdout**.
Therefore you cannot do something like this:

    # WRONG - do not use!
    choice=$(whiptail --inputbox "Type something here" 0 0)

One solution: Redirect stderr to a temp file. Then read the temp file to get the user input.\
Redirecting stderr to a file is a common idiom in shell scripting.

``` bash
# Create a temporary file and make sure it goes away when we're done
tmpfile=$(mktemp)
trap "rm -f $tmpfile" 0 1 2 5 15

# Generate the dialog box
whiptail --inputbox "Type something here" 0 0 2> $tmpfile

case $? in
  0) choice=$(cat $tmpfile);;
  1) echo "Cancel pressed.";;
  255) echo "ESC pressed.";;
esac
```

To avoid the temp file, and capture the user input **directly into a variable**,
you need to use "command substitution" with redirection.

```bash
choice=$(whiptail-command 2>&1 >/dev/tty)
## Or ##
choice=$(whiptail-command 3>&1 1>&2 2>&3-)    
```
Command substitution: runs the command; substitutes the command's stout, then runs the line of code.
In this case assigning a value to a variable. We need stderr to be assigned to the variable,
and stdout to go to the screen.

The first redirection reads:

- Redirect stderr to stdout 
- Redirect stdout directly to the screen (/dev/tty)
- Works on Linux but maybe not BSD or Macintosh

The second is a bit more complex, but is more commonly used:

- Copy whiptail's stdout to new "File Descriptor 3" (FD 0 is stdin, FD 1 is stdout, FD 2 is stderr)
- Redirect whiptail's stdout to where stderr is currently pointing (the screen)
- Redirect whiptails stderr to FD 3. This gets assigned to the variable.
- The trailing dash - means "close FD 3". Not strictly needed, all FDs are closed when a script or terminal session ends.
- Redirections work from left to right and must be done in the correct order.

Note: whiptail has an option `--output-fd 1` which looks like it would do what we want without the redirection.
Unfortunately, it doesn't work. Dialog has the same option and it does work. Dialog also has `--stdout` which in spite of
warnings in the man page, also works, at least on Linux.

## Infobox bug
There is a bug in whiptail (but not dialog) that makes Info Box not display on some terminals. you can set the terminal emulation to something different and it will work.

    TERM=ansi whiptail --title "Example Dialog" --infobox "This is an example of an info box" 8 78

This sets the TERM variable to ansi, only for the duration of the whiptail command.

## Whiptail colors in Ubuntu
Ubuntu has the "Newt Colors" set to a magenta hue. To get back to the
original blue, set the environment variable to null.

    NEWT_COLORS=  whiptail --title "Example Dialog" --msgbox "This is an example of a message box. You must hit OK to continue." 8 78

To make this global throughout your script, put this near the top.

    NEWT_COLORS=
    export NEWT_COLORS

## Menu function
- Uses dialog but can also use whiptail.
- Avoids the usual "case" statement


``` bash
MenuBox () {
    # Wrapper for "dialog --menu"
    # dialog expects "tag" "item" and returns "tag"
    # We will use "tag" to hold a command and only display "items"
    # Runs in an infinite loop.
    # ARGS:     "title"    Displayed in titlebar
    #           "command" "Item Text"...
    # RETURNS:  None
    # NOTES:    Command should a Bash function, or very simple command
    #           Don't try to embed quotes in the command
    local title choice
    title=$1; shift
    while true; do
        choice=$(
            dialog --title "$title" --notags --menu "" \
            0 0 0 3>&1 1>&2 2>&3 "$@" \
        )
        # $choice will be null if user pressed CANCEL, ESC or CTRL-C
        [[ $choice ]] || return 1
        # execute $choice
        $choice
    done
}
```
