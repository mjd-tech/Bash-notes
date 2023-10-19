# "Short Circuit" Operators

These operators (`&&` and `||`) are used to link commands together. They
check the exit code of the previous command to determine whether or not
to execute the next command in the sequence.

## &&

Execute the command on the left. If it returns an exit code of '0'
(success), then execute the command on the right. Otherwise, do not
execute the command on the right. In other words, a logical AND
operation. Example:

    $ mkdir d && cd d

This is a shortcut for the following:

    $ if mkdir d; then cd d; fi

In first example, BASH will execute mkdir d, then && will check the
result of the mkdir application after it finishes. If the mkdir
application was successful (exit code 0), then Bash will execute the
next command, cd d. If mkdir d failed, and returned a non-0 exit code,
Bash will skip the next command, and we will stay in the current
directory.

Sometimes you need to do more than one thing on the right hand side. Use
command grouping:

    $ mkdir d && { cd d; echo "It's OK"; }

Notice that the braces need to be surrounded by whitespace, and a
command terminator needs to come before the closing brace.

## `||`

Execute the command on the left. If it returns an exit code that is NOT
'0', indicating failure, then execute the command on the right.
Otherwise, do not execute the command on the right. In other words, a
logical OR operation. Example:

    $ mkdir d || echo "I couldn't create the directory"

This is a shortcut for

    $ if ! mkdir d; then echo "I couldn't create the directory"; fi

The logical OR form is very common for error checking.

## Using Both Together

In general, it's not a good idea to string together multiple different
control operators in one command. However, the following is a very
common way to use both && and \|\| in a shortcut for if..then..else

    $ [[ $(head -c2 file1) = '#!' ]] && echo "file1 looks like a script" || echo "file1 does not look like a script"

Also the && and \|\| serve as a "command separator" so you can use them
at the end of a line, like this:

    [[ some_command ]]  &&
        echo "true" ||
        echo "false"

Here, we did not need to use a backslash (line continuation) at the end
of the lines.
