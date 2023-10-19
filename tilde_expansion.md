# Tilde expansion

    ~
    ~/...

    ~NAME
    ~NAME/...

    ~+
    ~+/...

    ~-
    ~-/...

The tilde expansion is used to expand to several specific pathnames:

- home directories
- current working directory
- previous working directory

Tilde expansion is only performed, when the tilde-construct is at the
beginning of a word, or a separate word.

If there's nothing to expand, i.e. in case of a wrong username or any
other error condition, the tilde construct is not replaced, it stays
what it is.

Tilde expansion is also performed everytime a variable is assigned:

- after the **first** `=`: `TARGET=~moonman/share`
- after **every** `:` (colon) in the assigned value:
  `TARGET=file:~moonman/share`

This way you can correctly use the tilde expansion in your PATH:

    PATH=~/mybins:~peter/mybins:$PATH

**Spaces in the referenced pathes?** A construct like...

    ~/"my directory"

...is perfectly valid and works!

## Home directory

    ~
    ~<NAME>

This form expands to the home-directory of the current user (`~`) or the
home directory of the given user (`~<NAME>`).

If the given user doesn't exist (or if his home directory isn't
determinable, for some reason), it doesn't expand to something else, it
stays what it is. The requested home directory is found by asking the
operating system for the associated home directory for `<NAME>`.

To find the home directory of the current user (`~`), Bash has a
precedence:

- expand to the value of HOME if it's defined
- expand to the home directory of the user executing the shell
  (operating system)

That means, the variable `HOME` can override the "real" home directory,
at least regarding tilde expansion.

## Current working directory

    ~+

This expands to the value of the PWD variable, which holds the currect
working directory:

    echo "CWD is $PWD"

is equivalent to (note it **must** be a separate word!):

    echo "CWD is" ~+

## Previous working directory

    ~-

This expands to the value of the OLDPWD variable, which holds the
previous working directory (the one before the last `cd`). If `OLDPWD`
is unset (never changed the directory), it is not expanded.

    $ pwd
    /home/bash
    $ cd /etc
    $ echo ~-
    /home/bash
