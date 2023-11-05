# Dialog
TUI dialog boxes

## Scrollback
By default, dialog outputs to the "primary" screen and it clutters the terminal's scrollback buffer.

- To prevent this, use the `--keep-tite` argument. 
- This makes dialog use the "alternate" screen, like vim, htop, less, and many other "TUI" applications.
- You can use `export DIALOGOPTS="--keep-tite"` so don't have to specify it each time in your script.

## Colors
By default, dialog draws a full screen blue background and displays the dialog box on top.
- Sometimes, you don't want the blue background
- you just want dialog to use the terminal's default foreground and background colors.
- Unfortunately, this cannot be achieved with an argument or environment variable.
- You need to use a _dialogrc_ file. 

By default, dialog looks for `$HOME/.dialogrc`  
But you don't want to rely on that. If you run your script on another computer, it may not have a .dialogrc or it may be customized.

Here's a solution using a temp file
``` bash
tmpfile=$(mktemp) && readonly tmpfile || exit 1
trap 'rm -f "$tmpfile"' EXIT
{ echo "use_shadow = OFF" ; echo "use_colors = OFF" ; } > "$tmpfile"
export DIALOGRC="$tmpfile"
```

- use_shadow = OFF    Shadow dialog boxes? This also turns on color.
- use_colors = OFF    Turn color support ON or OFF
