# Trim leading/trailing whitespace from a string

This method uses shell parameter expansion and should work in any POSIX
shell, bash, zsh, dash, etc. It looks complicated, but it does allow for
the string to have embedded spaces. Those do not get stripped, only the
leading/trailing spaces.

``` bash
var="    Fred  Flintstone    "

# remove leading whitespace characters
var="${var#"${var%%[![:space:]]*}"}"

# remove trailing whitespace characters
var="${var%"${var##*[![:space:]]}"}"   

echo "===$var==="
```

The variable "var" has 4 leading and 4 trailing spaces.  

For leading spaces:  
the expression `${var%%[![:space:]]*}` returns the four leading spaces.  
It says, take var and greedily remove from the end anything that's not a
space followed by everything else.  
This pattern matches the "F" in Fred and everything that follows,
and that is what gets removed, leaving the four leading spaces.  
Now when combined with `${var# ... }` this says take var and non-greedily
remove the 4 leading spaces we just generated. This leaves us with "Fred
Flintstone" followed by 4 trailing spaces.

Removing trailing spaces works similarly.  
the expression `${var##*[![:space:]]}`returns the four trailing spaces.  
It says, take var and greedily remove from the front everything upto
something that's not a space.  
So this pattern will match the "e" in Flintstone and everything that
precedes, and that is what gets removed, leaving the four trailing
spaces.  
Now when combined with \${var% ... } this says take var and non-greedily
remove the 4 trailing spaces we just generated.

You can combine these two into a function

``` bash
Trim() {
    # Usage: Trim "   example   string   "
    : "${1#"${1%%[![:space:]]*}"}"
    : "${_%"${_##*[![:space:]]}"}"
    printf '%s\n' "$_"
}
```

This function uses the "no-op" trick.

- the "no-op" command `:` does nothing (it just returns "true")
- Since it's an actual command, it can take arguments.
- Bash has a special variable, the underscore `_` which means "the last
  argument of the last command"
- By using the no-op command and `$_` the temporary variable is avoided.
- You cannot just put something like `${1#*}` alone on a line. Bash
  would expand it and try to execute it as a command.

## Another way - Bash specific
- Use "extended glob" syntax. Slightly less confusing...
- extglob is often enabled in "interactive" bash sessions
- but usually NOT enabled in "non-interactive" bash sessions, ie. scripts
- check it: `shopt extglob`

``` bash
shopt -s extglob
var="    Fred  Flintstone    "

# remove leading whitespace characters
var="${var##*([[:space:]])}"

# remove trailing whitespace characters
var="${var%%*([[:space:]])}"

echo "===$var==="


# the trim function, using extglob
# Notice the function is encased in () rather than {}. Function runs in a subshell
# This way, we can enable extglob within the function,
# without affecting the extglob setting in the main script

Trim() (
    # Usage: Trim "   example   string   "
    shopt -s extglob
    : "${1##*([[:space:]])}"
    : "${_%%*([[:space:]])}"
    printf '%s\n' "$_"
)
```


