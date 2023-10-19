# Keyboard Shortcuts

Note: these shortcuts are for Bash in emacs mode (default). See vi mode
below for vim-style keybindings.

| THE Most useful shortcut! |                                                                 |
|---------------------------|-----------------------------------------------------------------|
| `Tab`                     | Complete partially typed command, path, variable, hostname, etc |
| `Ctrl-v Tab`              | Insert a literal tab. Can also do `Ctrl-v Ctrl-i`               |

## Emacs mode (default)

### Movement

|          |                                |
|:---------|:-------------------------------|
| `Home`   | Move to the beginning of line. |
| `End`    | Move to the end of line.       |
| `→`      | Move forward a character.      |
| `←`      | Move back a character.         |
| `Ctrl →` | Move forward a word.           |
| `Ctrl ←` | Move backward a word.          |

### Editing

|                 |                                                                                                      |
|:----------------|:-----------------------------------------------------------------------------------------------------|
| `Del`           | Delete the character under the cursor.                                                               |
| `Backspace`     | Delete the character to the left of the cursor.                                                      |
| `Alt r`         | Revert. Undo all changes made to this line.                                                          |
| `Ctrl x Ctrl e` | Open command line in \$EDITOR, if \$EDITOR is vim, :wq to execute, :q to leave vim without executing |

### Other

|                |                                                                                  |
|----------------|----------------------------------------------------------------------------------|
| `Ctrl c`       | Kill the current process.                                                        |
| `Ctrl z`       | Suspend. Stop the program and put it in the background. Type `fg` to restart it. |
| `Ctrl d`       | End of File. If typed at the command prompt, exits the shell.                    |
| `Ctrl l` (ell) | Clear the screen.                                                                |

## Vi Mode

Enter vi mode:

    # put this in your .bashrc file to make vi mode default
    set -o vi

Show whether you are in insert or command. In ~/.inputrc put this:

    show-mode-in-prompt on

We want Ctrl l (ell) to clear the screen like it does in emacs mode. Put
this in .bashrc

    bind -m vi-insert "\C-l":clear-screen

| Shortcut                                                       | Description                                                                                                 |
|:---------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------|
| ESC                                                            | Switch to command mode.                                                                                     |
| i                                                              | Insert before cursor.                                                                                       |
| a                                                              | Insert after cursor.                                                                                        |
| I                                                              | Insert at the beginning of line.                                                                            |
| A                                                              | Insert at the end of line.                                                                                  |
| c\<mov. comm\>                                                 | Change text of a movement command \<mov. comm\> (see below).                                                |
| C                                                              | Change text to the end of line (equivalent to c\$).                                                         |
| cc or S                                                        | Change current line (equivalent to 0c\$).                                                                   |
| s                                                              | Delete a single character under the cursor and enter input mode (equivalent to c\[SPACE\]).                 |
| r                                                              | Replaces a single character under the cursor (without leaving command mode).                                |
| R                                                              | Replaces characters under cursor.                                                                           |
| v                                                              | Edit (and execute) the current command in the text editor, defined in \$VISUAL or \$EDITOR variables, or vi |
| Basic Movement Commands (in command mode):                     |                                                                                                             |
| h or ←                                                         | Move one character left.                                                                                    |
| l or →                                                         | Move one character right.                                                                                   |
| w                                                              | Move one word or token right.                                                                               |
| b                                                              | Move one word or token left.                                                                                |
| W                                                              | Move one non-blank word right.                                                                              |
| B                                                              | Move one non-blank word left.                                                                               |
| e                                                              | Move to the end of the current word.                                                                        |
| E                                                              | Move to the end of the current non-blank word.                                                              |
| 0                                                              | Move to the beginning of line                                                                               |
| ^                                                              | Move to the first non-blank character of line.                                                              |
| \$                                                             | Move to the end of line.                                                                                    |
| %                                                              | Move to the corresponding opening/closing bracket.                                                          |
| Character Finding Commands (these are also Movement Commands): |                                                                                                             |
| fc                                                             | Move right to the next occurance of char c.                                                                 |
| Fc                                                             | Move left to the previous occurance of c.                                                                   |
| tc                                                             | Move right to the next occurance of c, then one character backward.                                         |
| Tc                                                             | Move left to the previous occurance of c, then one character forward.                                       |
| ;                                                              | Redo the last character finding command.                                                                    |
| ,                                                              | Redo the last character finding command in opposite direction.                                              |
| \|                                                             | Move to the n-th column (you may specify the argument n by typing it on number keys, for example, 20\|)     |
| Deletion Commands:                                             |                                                                                                             |
| x                                                              | Delete a single character under the cursor.                                                                 |
| X                                                              | Delete a character before the cursor.                                                                       |
| d\<mov. comm\>                                                 | Delete text of a movement command \<mov. comm\> (see above).                                                |
| D                                                              | Delete to the end of the line (equivalent to d\$).                                                          |
| dd                                                             | Delete current line (equivalent to 0d\$).                                                                   |
| CTRL-w                                                         | Delete the previous word.                                                                                   |
| CTRL-u                                                         | Delete from the cursor to the beginning of line.                                                            |
| Undo, Redo and Copy/Paste Commands:                            |                                                                                                             |
| u                                                              | Undo previous text modification.                                                                            |
| U                                                              | Undo all previous text modifications.                                                                       |
| .                                                              | Redo the last text modification.                                                                            |
| y\<mov. comm\>                                                 | Yank a movement into buffer (copy).                                                                         |
| yy                                                             | Yank the whole line.                                                                                        |
| p                                                              | Insert the yanked text at the cursor.                                                                       |
| P                                                              | Insert the yanked text before the cursor.                                                                   |
| Command History:                                               |                                                                                                             |
| k                                                              | Insert the yanked text before the cursor.                                                                   |
| j                                                              | Insert the yanked text before the cursor.                                                                   |
| G                                                              | Insert the yanked text before the cursor.                                                                   |
| /string or CTRL-r                                              | Search history backward for a command matching string.                                                      |
| ?string                                                        | Search history forward for a command matching string.                                                       |
| n                                                              | Repeat search in the same direction as previous.                                                            |
| N                                                              | Repeat search in the opposite direction as previous.                                                        |
| Completion commands:                                           |                                                                                                             |
| TAB or = or                                                    | List all possible completions.                                                                              |
| CTRL-i                                                         |                                                                                                             |
| \*                                                             | Insert all possible completions.                                                                            |
| Miscellaneous commands:                                        |                                                                                                             |
| ~                                                              | Invert case of the character under cursor and move a character right.                                       |
| \#                                                             | Prepend '#' (comment character) to the line and send it to the history.                                     |
| \_                                                             | Inserts the n-th word of the previous command in the current line.                                          |
| 0, 1, 2, ...                                                   | Sets the numeric argument.                                                                                  |
| CTRL-v                                                         | Insert a character literally (quoted insert).                                                               |
| CTRL-r                                                         | Transpose (exchange) two characters.                                                                        |

## Emacs mode - complete List

Note `C-a` = `Ctrl a`  
`M-a` = `Alt A`

    .---------------------------------------------------------------------------.
    |                                                                           |
    |                        Readline Emacs Editing Mode                        |
    |                         Default Keyboard Shortcut                         |
    |                               Cheat Sheet                                 |
    |                                                                           |
    '---------------------------------------------------------------------------'
    | Peteris Krumins (peter@catonmat.net), 2007.10.30                          |
    | http://www.catonmat.net  -  good coders code, great reuse                 |
    |                                                                           |
    | Released under the GNU Free Document License                              |
    '---------------------------------------------------------------------------'

     ======================== Keyboard Shortcut Summary ========================

    .--------------.-------------------.----------------------------------------.
    |              |                   |                                        |
    | Shortcut     | Function          | Description                            |
    |              |                   |                                        |
    '--------------'-------------------'----------------------------------------'
    | Commands for Moving:                                                      |
    '--------------.-------------------.----------------------------------------'
    | C-a          | beginning-of-line | Move to the beginning of line.         |
    '--------------+-------------------+----------------------------------------'
    | C-e          | end-of-line       | Move to the end of line.               |
    '--------------+-------------------+----------------------------------------'
    | C-f          | forward-char      | Move forward a character.              |
    '--------------+-------------------+----------------------------------------'
    | C-b          | backward-char     | Move back a character.                 |
    '--------------+-------------------+----------------------------------------'
    | M-f          | forward-word      | Move forward a word.                   |
    '--------------+-------------------+----------------------------------------'
    | M-b          | backward-word     | Move backward a word.                  |
    '--------------+-------------------+----------------------------------------'
    | C-l          | clear-screen      | Clear the screen leaving the current   |
    |              |                   | line at the top of the screen.         |
    '--------------+-------------------+----------------------------------------'
    | (unbound)    | redraw-current-   | Refresh the current line.              |
    |              | line              |                                        |
    '--------------'-------------------'----------------------------------------'
    | Commands for Changing Text:                                               |
    '--------------.-------------------.----------------------------------------'
    | C-d          | delete-char       | Delete one character at point.         |
    '--------------+-------------------+----------------------------------------'
    | Rubout       | backward-delete-  | Delete one character backward.         |
    |              | char              |                                        |
    '--------------+-------------------+----------------------------------------'
    | C-q or C-v   | quoted-insert     | Quoted insert.                         |
    '--------------+-------------------+----------------------------------------'
    | M-TAB or     | tab-insert        | Insert a tab character.                |
    | M-C-i        |                   |                                        |
    '--------------+-------------------+----------------------------------------'
    | a, b, A, 1,  | self-insert       | Insert the character typed.            |
    | ...          |                   |                                        |
    '--------------+-------------------+----------------------------------------'
    | C-t          | transpose-chars   | Exchange the char before cursor with   |
    |              |                   | the character at cursor.               |
    '--------------+-------------------+----------------------------------------'
    | M-t          | transpose-words   | Exchange the word before cursor with   |
    |              |                   | the word at cursor.                    |
    '--------------+-------------------+----------------------------------------'
    | M-u          | upcase-word       | Uppercase the current word.            |
    '--------------+-------------------+----------------------------------------'
    | M-l          | downcase-word     | Lowercase the current word.            |
    '--------------+-------------------+----------------------------------------'
    | M-c          | capitalize-word   | Capitalize the current word.           |
    '--------------+-------------------+----------------------------------------'
    | (unbound)    | overwrite-mode    | Toggle overwrite mode.                 |
    '--------------'-------------------'----------------------------------------'
    | Killing and Yanking:                                                      |
    '--------------.-------------------.----------------------------------------'
    | C-k          | kill-line         | Kill the text from point to the end of |
    |              |                   | the line.                              |
    '--------------+-------------------+----------------------------------------'
    | C-x Rubout   | backward-kill     | Kill backward to the beginning of the  |
    |              | -line             | line.                                  |
    '--------------+-------------------+----------------------------------------'
    | C-u          | unix-line-discard | Kill backward from point to the        |
    |              |                   | beginning of the line.                 |
    '--------------+-------------------+----------------------------------------'
    | M-d          | kill-word         | Kill from point to the end of the      |
    |              |                   | current word.                          |
    '--------------+-------------------+----------------------------------------'
    | M-Rubout     | backward-kill-word| Kill the word behind point.            |
    '--------------+-------------------+----------------------------------------'
    | C-w          | unix-word-rubout  | Kill the word behind point, using      |
    |              |                   | white space as a word boundary.        |
    '--------------+-------------------+----------------------------------------'
    | M-\          | delete-           | Delete all spaces and tabs around      |
    |              | horizontal-space  | point.                                 |
    '--------------+-------------------+----------------------------------------'
    | C-y          | yank              | Yank the top of the kill ring into the |
    |              |                   | buffer at point.                       |
    '--------------+-------------------+----------------------------------------'
    | M-y          | yank-pop          | Rotate the kill ring, and yank the new |
    |              |                   | top                                    |
    '--------------+-------------------+----------------------------------------'
    | (unbound)    | kill-whole-line   | Kill all characters on the current     |
    |              |                   | line                                   |
    '--------------+-------------------+----------------------------------------'
    | (unbound)    | kill-region       | Kill the text between the point and    |
    |              |                   | mark.                                  |
    '--------------+-------------------+----------------------------------------'
    | (unbound)    | copy-region-as-   | Copy the text in the region to the     |
    |              | kill              | kill buffer.                           |
    '--------------+-------------------+----------------------------------------'
    | (unbound)    | copy-backward-    | Copy the word before point to the kill |
    |              | word              | buffer.                                |
    '--------------+-------------------+----------------------------------------'
    | (unbound)    | copy-forward-word | Copy the word following point to the   |
    |              |                   | kill buffer.                           |
    '--------------'-------------------'----------------------------------------'
    | Keyboard Macros:                                                          |
    '--------------.-------------------.----------------------------------------'
    | C-x (        | start-kbd-macro   | Begin saving the chars typed into the  |
    |              |                   | current keyboard macro.                |
    '--------------+-------------------+----------------------------------------'
    | C-x )        | end-kbd-macro     | End saving the chars typed into the    |
    |              |                   | current keyboard macro.                |
    '--------------+-------------------+----------------------------------------'
    | C-x e        | call-last-kbd-    | Re-execute the last keyboard macro     |
    |              | macro             | defined.                               |
    '--------------'-------------------'----------------------------------------'
    | Commands for Manipulating the History:                                    |
    '--------------.-------------------.----------------------------------------'
    | Return       | accept-line       | Accept the line regardless of where    |
    |              |                   | the cursor is.                         |
    '--------------+-------------------+----------------------------------------'
    | C-p          | previous-history  | Fetch the previous command from the    |
    |              |                   | history list.                          |
    '--------------+-------------------+----------------------------------------'
    | C-n          | next-history      | Fetch the next command from the        |
    |              |                   | history list.                          |
    '--------------+-------------------+----------------------------------------'
    | M-<          | beginning-of-     | Move to the first line in the history. |
    |              | history           |                                        |
    '--------------+-------------------+----------------------------------------'
    | M->          | end-of-history    | Move to the end of the input history   |
    '--------------+-------------------+----------------------------------------'
    | C-r          | reverse-search-   | Search backward starting at the        |
    |              | history           | current line (incremental).            |
    '--------------+-------------------+----------------------------------------'
    | C-s          | forward-search-   | Search forward starting at the current |
    |              | history           | line (incremental).                    |
    '--------------+-------------------+----------------------------------------'
    | M-p          | non-incremental-  | Search backward using non-incremental  |
    |              | reverse-search-   | search.                                |
    |              | history           |                                        |
    '--------------+-------------------+----------------------------------------'
    | M-n          | non-incremental-  | Search forward using non-incremental   |
    |              | forward-search-   | search.                                |
    |              | history           |                                        |
    '--------------+-------------------+----------------------------------------'
    | M-C-y        | yank-nth-arg      | Insert the n-th argument to the        |
    |              |                   | previous command at point.             |
    '--------------+-------------------+----------------------------------------'
    | M-. M-_      | yank-last-arg     | Insert the last argument to the        |
    |              |                   | previous command.                      |
    '--------------+-------------------+----------------------------------------'
    | (unbound)    | history-search-   | Search forward for a string between    |
    |              | backward          | start of line and point.               |
    '--------------+-------------------+----------------------------------------'
    | (unbound)    | history-search-   | Search backward for a string between   |
    |              | forward           | start of line and point.               |
    '--------------'-------------------'----------------------------------------'
    | Completing:                                                               |
    '--------------.-------------------.----------------------------------------'
    | TAB          | complete          | Attempt to perform completion on the   |
    |              |                   | text before point.                     |
    '--------------+-------------------+----------------------------------------'
    | M-?          | possible-         | List the possible completions of the   |
    |              | completions       | text before point.                     |
    '--------------+-------------------+----------------------------------------'
    | M-*          | insert-           | Insert all completions of the text     |
    |              | completions       | before point generated by              |
    |              |                   | possible-completions.                  |
    '--------------+-------------------+----------------------------------------'
    | (unbound)    | menu-complete     | Similar to complete but replaces the   |
    |              |                   | word with the first match.             |
    '--------------+-------------------+----------------------------------------'
    | (unbound)    | delete-char-or-   | Deletes the car if not at the          |
    |              | list              | beginning of line or acts like         |
    |              |                   | possible-completions at the end of     |
    |              |                   | the line.                              |
    '--------------'-------------------'----------------------------------------'
    | Miscellaneous:                                                            |
    '--------------.-------------------.----------------------------------------'
    | C-x C-r      | re-read-init-file | Read and execute the contents of       |
    |              |                   | inputrc file.                          |
    '--------------+-------------------+----------------------------------------'
    | C-g          | abort             | Abort the current editing command and  |
    |              |                   | ring the terminal's bell.              |
    '--------------+-------------------+----------------------------------------'
    | M-a, M-b,    | do-uppercase-     | If the metafield char 'x' is lowercase |
    | M-x, ...     | version           | run the command that is bound to       |
    |              |                   | uppercase char.                        |
    '--------------+-------------------+----------------------------------------'
    | ESC          | prefix-meta       | Metafy the next character typed.       |
    |              |                   | For example, ESC-p is equivalent to    |
    |              |                   | Meta-p                                 |
    '--------------+-------------------+----------------------------------------'
    | C-_ or       | undo              | Incremental undo, separately           |
    | C-x C-u      |                   | remembered for each line.              |
    '--------------+-------------------+----------------------------------------'
    | M-r          | revert-line       | Undo all changes made to this line.    |
    '--------------+-------------------+----------------------------------------'
    | M-&          | tilde-expand      | Perform tilde expansion on the current |
    |              |                   | word.                                  |
    '--------------+-------------------+----------------------------------------'
    | C-@ or       | set-mark          | Set the mark to the point.             |
    | M-<space>    |                   |                                        |
    '--------------+-------------------+----------------------------------------'
    | C-x C-x      | exchange-point-   | Swap the point with the mark.          |
    |              | and-mark          |                                        |
    '--------------+-------------------+----------------------------------------'
    | C-]          | character-search  | Move to the next occurance of current  |
    |              |                   | character under cursor.                |
    '--------------+-------------------+----------------------------------------'
    | M-C-]        | character-search- | Move to the previous occurrence of     |
    |              | backward          | current character under cursor.        |
    '--------------+-------------------+----------------------------------------'
    | M-#          | insert-comment    | Without argument line is commented,    |
    |              |                   | with argument uncommented (if it was   |
    |              |                   | commented).                            |
    '--------------+-------------------+----------------------------------------'
    | C-e          | emacs-editing-    | When in vi mode, switch to emacs mode. |
    |              | mode              |                                        |
    '--------------+-------------------+----------------------------------------'
    | M-C-j        | vi-editing-mode   | When in emacs mode, switch to vi mode. |
    '--------------+-------------------+----------------------------------------'
    | M-0, M-1,    | digit-argument    | Specify the digit to the argument.     |
    | ..., M--     |                   | M-- starts a negative argument.        |
    '--------------+-------------------+----------------------------------------'
    | (unbound)    | dump-functions    | Print all of the functions and their   |
    |              |                   | key bindings.                          |
    '--------------+-------------------+----------------------------------------'
    | (unbound)    | dump-variables    | Print all of the settable variables    |
    |              |                   | and their values.                      |
    '--------------+-------------------+----------------------------------------'
    | (unbound)    | dump-macros       | Print all of the key sequences bound   |
    |              |                   | to macros.                             |
    '--------------+-------------------+----------------------------------------'
    | (unbound)    | universal-        | Either sets argument or multiplies the |
    |              | argument          | current argument by 4.                 |
    '--------------'-------------------'----------------------------------------'

     ===========================================================================

    .---------------------------------------------------------------------------.
    | Peteris Krumins (peter@catonmat.net), 2007.10.30                          |
    | http://www.catonmat.net  -  good coders code, great reuse                 | 
    |                                                                           |
    | Released under the GNU Free Document License                              |
    '---------------------------------------------------------------------------'

## Reference

- <https://catonmat.net/bash-emacs-editing-mode-cheat-sheet>
- <https://catonmat.net/bash-vi-editing-mode-cheat-sheet>
