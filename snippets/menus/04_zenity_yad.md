# Zenity and Yad
Zenity is a GUI dialog box utility, installed by default in most desktop distros.

Advantages:
- easy to use. you can pipe into `zenity --list` to make a menu, or specify command line arguments
- has a nice file/directory selection dialog, much better than Dialog's
- Don't need to check for invalid choice.

Disadvantages:
- Like Dialog and Whiptail, cli command output can be displayed in a dialog box, but no ANSI escape sequences.
- There's no "main" window. You're just popping up a sequence of dialog boxes.

``` bash
title="Select example"
prompt="Pick an option:"
items=("A" "B" "C")

while item=$(zenity --title="$title" --text="$prompt" --list --column="" "${items[@]}"); do
    case "$opt" in
        "${items[0]}" ) zenity --info --text="You picked $item, option 1";;
        "${items[1]}" ) zenity --info --text="You picked $item, option 2";;
        "${items[2]}" ) zenity --info --text="You picked $item, option 3";;
        *) zenity --error --text="Invalid option. Try another one.";;
    esac
done
```

Notes:
- will loop until the user explicitly chooses Cancel.
- If choice is meant to be one-time only, just use *break* after *esac*

## Yad
Yad (Yet another dialog) is Zenity with many enhancements. Usually not installed by default, and the version in the Ubuntu repo is ancient.

Advantages:
- Yad can create an actual GUI app, in addition to just popping up GUI dialog boxes.
- lots of stuff like html viewer, icon viewer, app selector

Disadvantages:
- a VERY STEEP learning curve. Not much documentation, or examples 
- again, cli output can be displayed in a dialog box, but no ANSI escape sequences.
- To use it to its full potential, requires a lot of "housekeeping": named pipes, temp files, exporting variables, callback functions.
