# Bash "select"
Requires Bash, won't work in Dash, Busybox, etc.
- Automatically creates a menu from a list of items.
- Otherwise, you use the same techniques as "Posix style"

Advantages:
- Less code required compared to "Posix style"
- No dependencies

Disadvantages:
- No mouse support
- No control over the "presentation". It does its thing and you have to live with it.

Notes:
- `select` displays the menu once, then runs an infinite loop prompting and getting user input
- This is wrapped in another infinite loop, to restart `select` each time to show the list of options again.
- To exit out of the nested loops, need `break 2`
- `PS3` is a special Bash Variable for the `select` prompt
- `REPLY` is a special Bash Variable for user input

``` bash
#!/bin/bash

PS3="Select an item: "

items=("Item 1" "Item 2" "Item 3")

while true; do
    clear
    echo "This is a menu"
    select item in "${items[@]}" Quit; do
        case $REPLY in
            1) echo "Selected $REPLY which means $item"; break;;
            2) echo "Selected $REPLY which means $item"; break;;
            3) echo "Selected $REPLY which means $item"; break;;
            $((${#items[@]}+1)) ) echo "Goodbye"; break 2;;
            *) echo "Error: Unknown choice $REPLY"; break;
        esac
    done
    read -rsn1 -p "Press any key to continue..."
done

```

`select` has some quirks.

- Sometimes menu items are arbitrarily displayed horizontally.
- This seems to be fixed somewhat in recent Bash versions since 5.0.
- To force single column display, add COLUMNS=1 to the script.
