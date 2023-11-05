# Tests

There are two forms of testing in Bash

1.  The "POSIX" `test` and `[` built-in commands.
2.  The `[[` keyword.

If portability is not a concern, the second form is highly recommended.

## The `test` and `[` command

These two are equivalent:

    test expr
    [ expr ]   # must have space around [ and ] brackets

The test builtin command returns 0 (True) or 1 (False), depending on the
evaluation of an expression, expr. You can examine the return value by
displaying `$?`; you can use the return value with && and `||`; or you
use it in conditional constructs ( if, while, etc. )

Some simple tests:

    $ test 3 -gt 4 && echo True || echo false
    false

    $ [ "abc" != "def" ]; echo $?
    0

    $ test -d "$HOME" ; echo $?
    0

## String Tests

| Operator | Description                                   |
| -------- | --------------------------------------------- |
| =        | equal                                         |
| !=       | not equal                                     |
| `<`      | less than. First string sorts before second   |
| `>`      | greater than. First string sorts after second |
| -n       | string not empty                              |
| -z       | null string                                   |

If no operator given, it is equivalent to -n  
``` bash
# These are equivalent
[ -n $var ]
[ $var ]
```

Note: the `<` and `>` operators are used by the shell for redirection, you
must escape them using `\`.

## Arithmetic tests

| Operator | Description           |
| -------- | --------------------- |
| -eq      | equal                 |
| -ne      | not equal             |
| -lt      | less than             |
| -le      | less than or equal    |
| -gt      | greater than          |
| -ge      | greater than or equal |

## File tests

| Operator | Description                                       |
| -------- | ------------------------------------------------- |
| -b       | block device                                      |
| -c       | character device                                  |
| -d       | directory                                         |
| -e       | exists (also -a)                                  |
| -f       | regular file, not a directory or device file      |
| -g       | set-group-id (sgid) flag set on file or directory |
| -G       | group id same as yours                            |
| -h       | symbolic link (also -L)                           |
| -k       | sticky bit set                                    |
| -N       | file modified since it was last read              |
| -O       | you are the owner of file                         |
| -p       | named pipe                                        |
| -r       | readable by you                                   |
| -s       | not zero size                                     |
| -S       | socket                                            |
| -t       | terminal device                                   |
| -u       | set-user-id (suid) flag set on file               |
| -w       | writable by you                                   |
| -x       | executable by you                                 |
| -nt      | file 1 is newer than file 2                       |
| -ot      | file 1 is older than file 2                       |
| -ef      | file 1 and file 2 are hard links to same file     |
| !        | "Not" - reverses the sense of above tests         |

## Other

| Operator | Description                                           |
| -------- | ----------------------------------------------------- |
| -a       | binary operator, combine expressions with logical AND |
| -o       | binary operator, combine expressions with logical OR  |
| -o       | as a unary operator, test if shell option is set      |

## `[[` and `((`

- The `[[` operator is a shell keyword, whereas test and `[` are commands.
- `[[` is easier to use as it does not word split parameter expansions
and does not require escaping `<` and `>`. 
- It also adds features that test and `[` do not have.


STRING = (or ==) PATTERN

By default, shell pattern matching is performed. True if the string
matches the glob pattern.
If the pattern on the right hand side is quoted, then it will act as a
string comparison.


STRING =~ REGEX

True if the string matches the regex pattern. The regex pattern
should be unquoted.
Best to put the regex in a variable, then expand the unquoted
variable.


( EXPR )

Parantheses can be used to change the evaluation precedence


EXPR && EXPR

Much like the '-a' operator of test, but does not evaluate the
second expression if the first already turns out to be false


EXPR `||` EXPR

Much like the '-o' operator of test, but does not evaluate the
second expression if the first already turns out to be true


-v varname

True if the shell variable varname is set (has been assigned a
value). This will work with the test and [ built-ins, but is not
portable, ie wont work in Dash or Busybox Ash.


The `(( ))` compound command evaluates an arithmetic expression. It is a
shortcut for the built-in "let" command.

| operator                             | description                             |
| ------------------------------------ | --------------------------------------- |
| `id++ id--`                          | variable post-increment, post-decrement |
| `++id --id`                          | variable pre-increment, pre-decrement   |
| `- +`                                | unary minus, plus                       |
| `! ~`                                | logical and bitwise negation            |
| `**`                                 | exponentiation                          |
| `* / %`                              | multiplication, division, remainder     |
| `+ -`                                | addition, subtraction                   |
| `<< >>`                              | left and right bitwise shifts           |
| `<= >= < >`                          | comparison                              |
| `== !=`                              | equality, inequality                    |
| `&`                                  | bitwise AND                             |
| `^`                                  | bitwise XOR                             |
| `\|`                                 | bitwise OR                              |
| `&&`                                 | logical AND                             |
| `\|\|`                               | logical OR                              |
| `expr ? expr : expr`                 | conditional operator                    |
| `= *= /= %= += -= <<= >>= &= ^= \|=` | assignment                              |

You do not need to escape operators between (( and )). Arithmetic is
done on integers. Division by 0 causes an error, but overflow does not.

NOTE: a variable (or expression) that evaluates to zero will be
considered "false" for the purposes of the arithmetic evaluation.
Because the evaluation is false, it will exit with a status of 1.
Likewise, if the expression is non-zero, it will be considered true; and
since the evaluation is true, it will exit with status 0.

This is potentially very confusing, even to experts, so you should take some
time to think about this. Nevertheless, when things are used the way
they're intended, it makes sense in the end.
