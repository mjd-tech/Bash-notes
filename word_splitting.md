# Word Splitting
Word splitting occurs once any of the following expansions are done (and
only then!)

- [Parameter expansion](./parameter_expansion.md)
- [Command Substitution](command_substitution)
- [Arithmetic Expansion](arithmetic_expansion)

Bash will scan the results of these expansions for special `IFS`
characters that mark word boundaries. This is only done on results that
are **not double-quoted**! If they are double quoted, Bash considers the
resulting string one word.

Example

    $ echo Fred   Flintstone      # 3 spaces between Fred and Flintstone.
    Fred Flintstone               # Huh? where did my extra spaces go?

Bash interpreted this as "The first word is a command, echo. The next
word is Fred, that's an argument to echo, the next word is Flintstone,
another argument." Echo prints each argument separated by a space. Bash
is looking for "words", found 3 words, and ignored extra spaces. To
preserve the spaces, use quotes

    $ echo "Fred   Flintstone"    # 3 spaces between Fred and Flintstone.
    Fred   Flintstone             # That's better.

This time Bash finds only TWO words, and executes echo with ONE
argument. Now, the same with variable expansion:

    $ myvar="Fred   Flintstone"   # 3 spaces between words. Bash removes the quotes when assigning the variable
    $ echo $myvar                 # This looks like only two words, but not after word-splitting, it then becomes three words.
    Fred Flintstone               # Huh? where did my extra spaces go?

    $ echo "$myvar"               # Use double quotes.
    Fred    Flintstone            # That's better.

What happens with literal double-quotes embedded in the variable?

    $ myvar='-n "Fred   Flintstone"'  # Trying to be clever, echo without newline, preserve extra spaces.
    echo $myvar                       # Expecting this would be the same as entering echo -n "Fred   Flintstone" at a prompt.
    "Fred Flintstone"                 # the -n worked. But lost the spaces and added the quotes!

Bash word-split myvar into 3 words, using whitespace as a delimiter. It
treated the double-quotes as LITERAL characters. Here are the 3 words
that became arguments to echo

1.  -n
2.  "Fred
3.  Flintstone"

You can't put Bash "syntax stuff" like double quotes into a variable,
then expand the variable, and expect it to work. Bash has already
scanned for this kind of stuff. Similarly, you cannot use reserved
words, pipe symbols, redirections, or any other control sequences, and
expect them to retain their special meaning. They will be taken
literally. All the special meaning stuff has already been processed, and
at this point Bash is looking for one command and zero or more
arguments.  

## Internal Field Separator IFS

The `IFS` variable holds the characters that Bash sees as word
boundaries in this step. The default contains the characters

- space
- tab
- newline

These characters are also assumed when IFS is **unset**. When `IFS` is
**empty** (nullstring), no word splitting is performed at all.

To check the value of IFS:

    printf "%q\n" "$IFS"
    $' \t\n'

    This prints IFS in "ANSI C Quoting" format: $'something'
    In this case, within the single quotes we have: space; \t (tab); \n (newline)

## Behaviour

The results of the expansions mentioned above are scanned for
`IFS`-characters. If **one or more** (in a sequence) of them is found,
the expansion result is split at these positions into multiple words.

This doesn't happen when the expansion results were **double-quoted**.

When a null-string (e.g., something that before expanded to
nothing) is found, it is removed, unless it is quoted (`''` or
`""`).

**Again note:** Without any expansion beforehand, Bash won't
perform word splitting! In this case, the initial token parsing is
solely responsible.
