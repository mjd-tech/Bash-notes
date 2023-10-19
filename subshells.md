# Subshells

A subshell is a child process launched by a shell (or shell script).

- A separate instance of the command processor.
- inherits a *copy* of the current "environment" variables (i.e. PATH,
  HOME, etc), file descriptors, and all the variables and functions that
  you have defined in your script or your interactive session.
- Shell scripts run in a subshell (child process) of the parent shell.
- A shell script can itself launch subshells.
- Within the subshell, if you change any variables, working directory,
  etc, they will not be visible back in the parent shell.
- The subshell is destroyed after the last command is executed.

Subshells are useful when you need to execute a block of code that uses
a different working directory or some different environment variables,
and you don't want to bother saving and restoring these things.

You can use a subshell explicitly, by putting a block of code in
parentheses ( code... )

But the shell can use a subshell without your asking for it. The most
notorious example is the last command in a pipeline.

Here is a classic example of the subshell "gotcha"

    # Works only in ksh88/ksh93, or bash 4.2 with lastpipe enabled, i.e. shopt -s lastpipe
    # In other shells, this will print 0

    linecount=0

    printf '%s\n' foo bar |
      while read -r line
      do
          linecount=$((linecount + 1))
      done

      echo "total number of lines: $linecount"

The reason for this is that the "while" loop is the last element of a
pipeline, and by default Bash runs the last element of a pipeline in a
subshell. The while loop above is executed in a new subshell, so it gets
its own copy of the variable *linecount* created with the initial value
of '0' taken from the parent shell. This copy then is used for counting.
When the while loop is finished, the subshell copy is discarded, and the
original variable *linecount* of the parent (whose value hasn't changed)
is used in the echo command.

Here is a list of commands that in some way fail to set foo=bar for
subsequent commands  
(note that all the examples set it in some subshell, and can use it
until the subshell ends):

# Commands that launch a subshell

**Executing other programs or scripts**

    ./setmyfoo
    foo=bar ./something

**Anywhere in a pipeline in Bash**

    true | foo=bar | true

**Any command that executes new shells**

    awk '{ system("foo=bar") }'h
    find . -exec bash -c 'foo=bar' \;

**Backgrounded commands and coprocs**

    foo=bar &
    coproc foo=bar

**Command expansion** true "\$(foo=bar)"

**Process substitution** true \< \<(foo=bar)

**Commands explicitly subshelled with ()** ( foo=bar )

and probably some more that I'm forgetting.

# Using subshells to our advantage

Trying to set a variable, option or working dir within a subshell will
result in the changes not being visible in the "parent" shell.

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
