# Command Substitution
execute commands in a subshell, substitute output

## Examples
``` bash
echo "Current path is $(pwd)"   # Preferred syntax
echo "Current path is `pwd`"    # Backtick syntax, not recommended
foo=$(pwd)                      # assign to variable
foo="$(pwd)"                    # Ok, but you don't need quotes in variable assignments
foo=$( <somefile )              # assign contents of a file to a variable, without using "cat"
foo=$(cp file /some/path 2>&1)  # capture both stdout and stderr
foo=$(                          # Multi-line
    pwd                         # Don't need to escape newlines
    ls                          # can contain comments
)
```

## Notes
- **trailing** newlines are removed from command substitutions.
- **if not quoted**, the results undergo word splitting and pathname expansion.
- Word splitting also removes embedded newlines and other `IFS` characters.


## Backticks vs `$()`
The backtick syntax is discouraged because 
- backticks look similar to single quotes
- it's clumsy to "nest" ("inner" backticks need to be escaped)
- backtick syntax is based on simple character substitution,
- whereas every `$()`-construct opens an own, subsequent parsing step
- Everything inside `$()` is interpreted as if written normal on a command line. No special escaping is needed.
