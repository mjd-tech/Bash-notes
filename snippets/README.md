# Bash snippets
Stuff to paste into your Bash scripts

## Where to put scripts
Usually you want scripts to be in the PATH, so you can run them from the terminal.
- `/usr/local/bin`: for system-wide scripts, available to all users. (need to be root to add files here)
- `$HOME/.local/bin`: the "official" location for your own scripts. If this directory exists, it's put in the PATH.
- `$HOME/bin`: Ubuntu also puts this in the PATH, if it exists

A script that gets run by cron doesn't need to be in the PATH. Cron jobs run with a limited PATH, you typically specify the full path to the script.

## Bundles
- Your script uses extra files and needs to be installed as a "bundle"
- You don't want to just dump everything into `$HOME/.local/bin`
- Create a directory for the bundle. Put all the needed files in there.
  
Then do one of the following
- Put a symlink to the script in `$HOME/.local/bin`
- Put a launcher script in `$HOME/.local/bin`

Launcher:
``` bash
#!/bin/sh
exec $HOME/path/to/bundle/myscript
```
Within myscript, get the directory of the bundle
``` bash
script_dir="$(dirname "$(readlink -f "${BASH_SOURCE[0]}")")"
```