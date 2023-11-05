# Classic POSIX style
Uses an "echo-read-case" sequence. Runs on any POSIX shell such as Dash, Busybox, etc
- the oldest and most "portable" solution
- Typically, menu is wrapped in an infinite loop that exits when the user chooses "quit" or "exit".
- To prevent screen clutter, use the **clear** command at strategic places in the script.
- You often need a "Press any key to continue" function. (similar to the "Pause" command from MS-DOS)

Advantages: 
- You have total control of the menu's presentation.
- No dependencies

Disadvantages:
- no mouse support
- the "presentation" that you worked so hard to achieve is ruined if the user resizes the terminal.
- you can work around this with lots of extra lines of code.


``` bash
#!/bin/sh
# basic main menu

while : ; do
    clear
    echo "This is a Menu"
    echo "1. Option 1"
    echo "2. Option 2"
    echo "3. Option 3"
    echo "0. Quit"
    printf "Type a number and press Enter: "
    read -r choice
    case $choice in
      1)  echo "You selected Option 1" ;;
      2)  echo "You selected Option 2" ;;
      3)  echo "You selected Option 3" ;;
      0)  echo "Goodbye"; exit ;;
      *)  echo "Invalid option selected" ;;
    esac
    printf "Press Enter to continue..."; read -r choice
done
```

- If this is a "one time" menu, put a "break" in each "case" except "Invalid option selected"
- To keep the display from getting cluttered, put a "clear" in each "case"
