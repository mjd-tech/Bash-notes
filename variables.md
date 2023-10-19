# Variables

Bash documentation talks a lot about "Parameters", other languages call
them "variables".

## Parameters

Parameters are a named space in memory you can use to retrieve or store
information. Generally speaking, they will store string data, but can
also be used to store integers or arrays.

Bash parameters come in two flavors:

- **variables** - parameters that you can create and update yourself
- **special parameters** - read-only, pre-set by BASH, and used to
  communicate some type of internal status.

## Variables

Variable names must consist only of **letters, digits and underscores,
and cannot begin with a digit.** The name can be long, there are no
limits imposed by Bash, but there are limits imposed by the kernel. Any
reasonable length is fine.

To store (assign) data in a variable, use the following syntax:

    varname=vardata

Please note that you **cannot use spaces around the = sign** in an
assignment. If you write this:

    # This is wrong!
    varname = vardata

Bash will attempt to run a command named *varname* with 2 arguments: *=*
and *vardata*.

Another common error:

    # This is wrong!
    $varname=vardata

This is not Perl or PHP. The `$` symbol means "the value of". Use the
"bare" name in assignments. Use the `$` symbol when you want to get the
contents.

## Special Parameters

These are parameters used by Bash itself. They use numbers and other
characters that you can't use in "variables" Here's a summary of most of
the Special Parameters:

| Parameter Name  | Usage       | Description                                                                                                                                                                            |
|-----------------|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `0`             | `"$0"`      | Contains the name, or the path, of the script. This is not always reliable.                                                                                                            |
| `1 2 etc.`      | `"$1"` etc. | *Positional Parameters* contain the arguments that were passed to the current script or function. Parameters greater than 9 need to be access with braces, ie. `${12}`                 |
| `*`             | `"$*"`      | Expands to all the words of all the positional parameters. Double quoted, it expands to a single string containing them all, separated by the first character of the **IFS** variable. |
| `@`             | `"$@"`      | Expands to all the words of all the positional parameters. Double quoted, it expands to a list of them all as individual words.                                                        |
| `#`             | `$#`        | Expands to the number of positional parameters that are currently set.                                                                                                                 |
| `?`             | `$?`        | Expands to the exit code of the most recently completed foreground command.                                                                                                            |
| `$`             | `$$`        | Expands to the PID (process ID number) of the current shell.                                                                                                                           |
| `!`             | `$!`        | Expands to the PID of the command most recently executed in the background.                                                                                                            |
| `_`             | `"$_"`      | Expands to the last argument of the last command that was executed.                                                                                                                    |

And here are a few examples of **Variables** that the shell provides for you:

- **BASH_VERSION**: Contains a string describing the version of Bash.
- **HOSTNAME**: Contains the hostname of your computer. Either short or
  long form, depending on how your computer is set up.
- **PPID**: Contains the PID of the parent process of this shell.
- **PWD**: Contains the current working directory.
- **RANDOM**: Each time you expand this variable, a (pseudo)random
  number between 0 and 32767 is generated.
- **UID**: The ID number of the current user. Not reliable for
  security/authentication purposes, alas.
- **COLUMNS**: The number of characters that fit on one line in your
  terminal. (The width of your terminal in characters.) This does not
  seem to work reliably within a script. Use `$(tput cols)` in scripts.
- **LINES**: The number of lines that fit in your terminal. (The height
  of your terminal in characters.) This does not seem to work reliably
  within a script. Use `$(tput lines)` in scripts.
- **HOME**: The current user's home directory.
- **PATH**: A colon-separated list of paths that will be searched to
  find a command, if it is not an alias, function, builtin command, or
  shell keyword, and no pathname is specified.
- **PS1**: Contains a string that describes the format of your shell
  prompt.
- **TMPDIR**: Contains the directory that is used to store temporary
  files (by the shell).

(There are many more -- see the manual for a comprehensive list.)

## Variable Types

Although Bash is not a typed language, it does have a few different
types of variables. These types define the kind of content they are
allowed to have. Type information is stored internally by Bash.

- **Array**: `declare -a variable`: an array of strings.
- **Associative array**: `declare -A variable`: an associative array of strings (bash 4.0 +)
- **Integer**: `declare -i variable`: an integer, automatically triggers Arithmetic Evaluation.
- **Read Only**: `declare -r variable`: The variable can no longer be modified or unset.
- **Export**: `declare -x variable`: variable will be inherited by any child process.

Arrays are basically indexed lists of strings. They are very convenient
for their ability to store multiple strings together without relying on
a *delimiter* to split them apart (which is tedious when done correctly
and error-prone when not).

Defining variables as integers has the advantage that you can leave out
some syntax when trying to assign or modify them:

    # Without declaring as integer, concatenates the string.
    $ a=5; a+=2; echo $a; unset a
    52
    # Without declaring as integer, using explicit numeric command.
    $ a=5; let a+=2; echo $a; unset a
    7
    # Declare as integer, works as expected
    $ declare -i a=5; a+=2; echo $a; unset a
    7
    # Regular declaration, treats the expression as a string
    $ a=5+2; echo $a; unset a
    5+2
    # Declare as integer, treats expression as expression
    $ declare -i a=5+2; echo $a; unset a
    7

However, in practice the use of `declare -i` is exceedingly rare. In
large part, this is because it creates behavior that can be surprising
to anyone trying to maintain the script, who misses the `declare`
statement. Most experienced shell scripters prefer to use explicit
arithmetic commands ( `let` or `((...))` ) when they want to perform
arithmetic.

It is also rare to see an explicit declaration of an array using
`declare -a`. It is sufficient to write `array=(...)` and Bash will know
that the variable is now an array. The exception to this is the
associative array, which *must* be declared explicitly:
`declare -A myarray`.
