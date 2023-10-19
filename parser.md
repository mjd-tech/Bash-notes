# Bash Parser

## Summary

1.  Read line. If line ends with `\` then read next line.
2.  Process Quotes.
3.  Split line into commands, using ; as command separator
4.  Parse special operators, such as `>` redirection
5.  Perform expansions. Brace expansion {..}, pathname expansion `*`,
    variables `$`, command substitution `$(..)`, etc
6.  Split command(s) into command name and arguments. This is where
    "word splitting" occurs.
7.  Execute command(s)

## Detail
Bash is first and formost an interactive shell. The ultimate result of
most shell commands is to execute some program with a specific set of
arguments. Commands are little "sentences" composed of "words" separated
by spaces. There are certain "punctuation marks" that have special
meaning to the sentence. For example:

    command argument1 argument2 > somefile

The first "word" is a command. Do something. The next two words are
arguments to be supplied to the command. The "greater than" symbol is a
"punctuation mark" that means "take the output of "command" and write it
to "somefile" instead of displaying on the console. Commands and
arguments are separated by space. A newline (Enter key) ends the command
and tells bash to process the line of code.

The parser is concerned with three kinds of "tokens":
1. "words"
2. "reserved words"
3. "operators"

- words will be generally be interpreted as commands or arguments.
- reserved words have special meaning to the shell, usually flow
  control: while, for, case, if, etc.
- operators are one or more "metacharacters" that have special meaning
  to the shell: `| { } ( ) $ < >` etc.

## Step 1: Read data to execute

Bash always reads your script or commands on the bash command prompt
line by line. If your line ends with a backslash character, bash reads
another line before processing the command and appends that other line
to the current.

## Step 2: Process quotes

Once Bash has read in your line of data, it looks for quotes. Quotes are
generally used to when you have some "words" separated by spaces that
you want to be considered one "word". Also quotes are used to remove the
special meaning from certain words or characters. 

The first quote triggers a "quoted state" for all characters that follow up until
the next quote of the same type. Quotes, therefore, cannot be "nested",
they are processed in series, from left to right.

The 3 types of quotes are:

- single quotes: everything inside single quotes loses any "special
  meaning" and is "taken literally", **including backslash**
- double quotes: same as single except these characters retain special meaning: ``$ ` \``
- backslash - the next character loses "special meaning" and is "taken literally".

## Step 3: Split the read data into commands

Our line is now split up into separate commands using ; as a command
separator. Remember from the previous step that any ; characters that
were quoted or escaped do not have their special meaning anymore and
will not be used for command splitting. They will just appear in the
resulting command line literally:

The following steps are executed for each command that resulted from
splitting up the line of data:

## Step 4: Parse special operators

Look through the command to see whether there are any special operators
such as `{..}, <(..) > <  <<< |`  etc. These are all
processed in a specific order. Redirection operators are removed from
the command line, other operators are replaced by their resulting
expression: eg. {a..c} is replaced by a b c.

## Step 5: Perform Expansions

Bash has many operators that involve expansion. The simplest of these is
`$parameter`. The dollar sign followed by the name of a parameter, which
optionally might be surrounded by braces, is called Parameter Expansion.
What Bash does here is basically just replace the Parameter Expansion
operator with the contents of that parameter. Other expansions include
Pathname Expansion `echo *.txt`, Command Substitution `rm "$(which nano)"`, etc.

## Step 6: Split the command into a command name and arguments

The name of the command Bash has to execute is always the first word in
the line. The rest of the command data is split into words which make
the arguments. This process is called Word Splitting. Bash basically
cuts the command line into pieces wherever it sees whitespace. This
whitespace is completely removed and the pieces are called words.

Whitespace in this context means: Any spaces, tabs or newlines that are
not escaped. (Escaped spaces, such as spaces inside quotes, lose their
special meaning of whitespace and are not used for splitting up the
command line. They appear literally in the resulting arguments.)

As such, if the name of the command that you want to execute or one of
the arguments you want to pass contains spaces that you don't want bash
to use for cutting the command line into words, you can use quotes or
the backslash character.

## Step 7: Execute the command

Now that the command has been parsed into a command name and a set of
arguments, Bash executes the command and sets the command's arguments to
the list of words it has generated in the previous step.

If the command type is a function or builtin, the command is executed by
the same Bash process that just went through all these steps.

Otherwise, Bash will first fork off (create a new bash process),
initialize the new bash processes with the settings that were parsed out
of this command (redirections, arguments, etc.) and execute the command
in the forked off bash process (child process). The parent (the Bash
that did these steps) waits for the child to complete the command.
