# Line Continuation
What to do with long lines.

# How to
- put a backslash `\` at the end of the line
- this escapes the newline character
- no whitespace after the backslash!

```bash
somecmd --option1 \
    --option2 \
    arg1 "quoted arg"

# Backslash escape not needed in command substitution
foo=$(
    somecmd
)

# But needed here for the command that spans multiple lines
foo=$(
    somecmd \
    --option \
    arg
)

# Backslash escape not needed for array definition
array=(
    foo
    bar
    "quoted array element"
)

# Within quotes
foo="this contains
embedded newline"

bar="this does not contain \
embedded newline"

# Backslash escape not needed for pipes
somecmd | 
anothercmd

# Backslash escape not needed for logical operators
somecmd &&
anothercmd
```