# Using Shell History

Note: C-a means "Press the Ctrl key, hold it down, and press the A key,
then release both." M-b means "Press the Meta key (Alt), hold it down,
and press the B key, then release both." If there is no Meta key, press
and release the Esc key, then press and release the B key.

**Most useful commands**
| Action | Description |
|---|---|---|
| `Up, Down arrows` | move backwards and forwards through previous command lines. |
| `history` | display numbered list of commands executed previously. |
| `!487` | execute command number 487 from history list. |
| `!!` | repeats the previous command. |
| `!-3` | repeats the command three command lines back. |
| `C-r string` | Search backwards for command(s) matching string. |
|             | Type `C-r` again to go back further. |
|             | Press `Enter` to execute the command, or use the normal editing keys to modify the command-line as desired. |
|             | Press `Tab` to exit searching and put the selected command at the command prompt, without executing. |
|             | Press `Down arrow` to abort. |
| `!$` | use last argument from previous command. |

Note: install fzf for much better ctrl-r history search

**Other commands**
| Action | Description |
| `!:2` | use the second argument from previous command. |
| `!*` | use all arguments from previous command. |
| `!-3$` | use the last argument from 3 commands ago. |
| `!-3:2` | use the second argument from 3 commands ago. |
| `!-3*` | use all arguments from 3 commands ago. |
| `!string` | execute the last command that begins with string. (dangerous) |
| `!string:p` | print the last command that begins with with string without executing it. |
| `^find-str^repl-str`   | Replaces the first instance of the find-string, but only the first. | 
| `!!:gs/find-str/repl-str/` | general substitution modifier. (":s/…/…/") |
| `^string`   | removes string from the previous command. | 

