# Command Substitution
execute commands in a subshell, substitute output

## Examples
``` bash
echo "Current path is $(pwd)"   # Preferred syntax
echo "Current path is `pwd`"    # Backtick syntax, not recommended
```

## Notes
All **trailing** newlines are removed (below is an example for a workaround).

Like many Bash "expansions", **if not quoted**, the results undergo word splitting
and pathname expansion. Word splitting also removes embedded newlines and other `IFS` characters.
**If you need the literal results, quote the command substitution!**

The second form `` `COMMAND` `` is more or less obsolete for Bash, since
it has some trouble with nesting ("inner" backticks need to be escaped)
and escaping characters. Use `$(COMMAND)`, it's also POSIX!

When you call an explicit subshell `(COMMAND)` inside the command
substitution `$()`, then take care, this way is **wrong**:

    $((COMMAND))

Why? because it collides with the syntax for arithmetic expansion. You
need to separate the command substitution from the inner `(COMMAND)`:

    $( (COMMAND) )

## Specialities

When the inner command is only an input redirection, and nothing else,
for example
```
$( <FILE )
# or
` <FILE `
```
then Bash attempts to read the given file and act just if the given
command was `cat FILE`.

## A closer look at the two forms
All is based on the fact that the backquote-form is simple character
substitution, while every `$()`-construct opens an own, subsequent
parsing step. Everything inside `$()` is interpreted as if written
normal on a commandline. No special escaping is needed:

    echo "$(echo "$(ls)")" # nested double-quotes - no problem

## Examples

**To get the date:**

    DATE="$(date)"

**To copy a file and get `cp` error output:**

    COPY_OUTPUT="$(cp file.txt /some/where 2>&1)"

Attention: Here, you need to redirect `cp` `STDERR` to its `STDOUT`
target, because command substitution only catches `STDOUT`!

**Catch stdout and preserve trailing newlines:**

    var=$(echo -n $'\n'); echo -n "$var"; # $var == ""
    var=$(echo -n $'\n'; echo -n x); var="${var%x}"; echo -n "$var" # $var == "\n"

This adds "x" to the output, which prevents the trailing newlines of the
previous commands' output from being deleted by `$()`

By removing this "x" later on, we are left with the previous commands'
output with its trailing newlines.
