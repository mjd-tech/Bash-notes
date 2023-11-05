# Shellcheck
Checks your shell script for errors.

## Installation
shellcheck can be installed with your package manager.
On Arch-based systems, install shellcheck-bin from the AUR, 
otherwise you'll get dozens of haskell dependencies that you probably don't want or need.

## Usage

``` bash
shellcheck myscript
```

Sometimes shellcheck reports false positives or irrelevant warnings.

To disable a shellcheck warning, put this directive immediately above
the line (or block) of code in question

``` bash
# shellcheck disable SC2120
Pause () { read -rsn1 -p "${1:-Press any key to continue...}"; }
```

To make the directive apply to the entire script, place it at the top of
the script, before any non-comment code.  

Or, create a .shellcheckrc file in your project folder, like so

``` bash
# .shellcheckrc
disable=SC2059,SC2034 # Disable individual error codes
disable=SC1090-SC1100 # Disable a range of error codes
```

Or, place the .shellcheckrc file in your home directory, or `$HOME/.config` for global effect.

Or, run shellcheck like so

``` bash
shellcheck -e SC2059 myscript
```

Or, in your .bashrc or .zshrc export SHELLCHECK_OPTS='--exclude=SC2016'
