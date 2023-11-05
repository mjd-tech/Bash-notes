# Subshells
Here is a classic example of the subshell "gotcha"
```bash
linecount=0
echo -e "foo\nbar" |
    while read -r; do
        linecount=$((linecount + 1))
    done
echo "Line Count = $linecount"
# Displays 0, we expected 2
```

- The "while" loop is the last element of a pipeline, it runs in a subshell.
- The subshell receives a **copy** of linecount, not a "pointer" to linecount
- linecount is incremented **only within the subshell**

Fix #1: Don't use a pipe. Use command substitution with "herestring" redirection 
```bash
linecount=0
while read -r; do
    linecount=$((linecount + 1))
done <<< "$(echo -e "foo\nbar")"
echo "Line Count = $linecount"
# Displays 2, as expected
```
- "while" loops can use redirection just like a regular command
- command substitution produces a string.
- "herestring" feeds the string to the stdin of the while loop

Fix #2: prevent the "while" loop running in a subshell"
```bash
# Put this in your script
shopt -s lastpipe
```

## Intentional and unintentional subshells
Intentional Subshells are used when you need to execute a block of code that uses
a different working directory or some different environment variables,
and you don't want to bother saving and restoring these things.

You can use a subshell explicitly, by putting a block of code in
parentheses `( cmds... )`

But the shell can use a subshell without your asking for it. The most
notorious case is the last command in a pipeline.

## Things that launch a subshell
- Command expansion: `$(grep foo myfile)`
- Process substitution: `somecmd < <(grep foo myfile)`
- Commands explicitly subshelled with (): `( grep foo myfile )`
- pipelines: somecmd | anothercmd | yetanothercmd

## Using subshells to your advantage
Setting a variable, option or working directory within a subshell will
not be visible in the "parent" shell.

``` bash
# cd to each dir and run make
for dir in */; do ( cd "$dir" && make ); done

# Compare to the more fragile
for dir in */; do cd "$dir"; make; cd ..; done

# mess with important variables
fields=(a b c); ( IFS=':'; echo ${fields[*]})

# Compare to the cumbersome
fields=(a b c); oldIFS=$IFS; IFS=':'; echo ${fields[*]}; IFS=$oldIFS; 

# Limit scope of options
( set -e; foo; bar; baz; ) 
```

See:   
https://unix.stackexchange.com/questions/421020/what-is-the-exact-difference-between-a-subshell-and-a-child-process

## From the Bash Manual...

The shell has an execution environment, which consists of the following:

- open files inherited by the shell at invocation, as modified by
  redirections supplied to the exec builtin
- the current working directory as set by *cd*, *pushd*, or *popd*, or
  inherited by the shell at invocation
- the file creation mode mask as set by *umask* or inherited from the
  shell’s parent
- current traps set by *trap*
- shell parameters that are set by variable assignment or with set or
  inherited from the shell's parent in the environment
- shell functions defined during execution or inherited from the shell’s
  parent in the environment
- options enabled at invocation (either by default or with command-line
  arguments) or by set
- options enabled by *shopt* (see The Shopt Builtin)
- shell aliases defined with *alias* (see Aliases)
- various process IDs, including those of background jobs (see Lists),
  the value of \$\$, and the value of \$PPID

When a simple command other than a builtin or shell function is to be
executed, it is invoked in a separate execution environment that
consists of the following. Unless otherwise noted, the values are
inherited from the shell.

- the shell's open files, plus any modifications and additions specified
  by redirections to the command
- the current working directory
- the file creation mode mask
- shell variables and functions marked for export, along with variables
  exported for the command, passed in the environment (see Environment)
- traps caught by the shell are reset to the values inherited from the
  shell’s parent, and traps ignored by the shell are ignored

A command invoked in this separate environment cannot affect the shell's
execution environment.

Command substitution, commands grouped with parentheses, and
asynchronous commands are invoked in a subshell environment that is a
duplicate of the shell environment, except that traps caught by the
shell are reset to the values that the shell inherited from its parent
at invocation. Builtin commands that are invoked as part of a pipeline
are also executed in a subshell environment. Changes made to the
subshell environment cannot affect the shell’s execution environment.

Subshells spawned to execute command substitutions inherit the value of
the -e option from the parent shell. When not in POSIX mode, Bash clears
the -e option in such subshells.

If a command is followed by a ‘&’ and job control is not active, the
default standard input for the command is the empty file /dev/null.
Otherwise, the invoked command inherits the file descriptors of the
calling shell as modified by redirections.
