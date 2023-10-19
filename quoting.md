# Quoting

Quoting removes the special meaning of certain characters or words to
the shell, such as operators, whitespace, or keywords.

## Backslash

- Preserves the literal meaning of the next character, except for
  newline
- A Backslash preceding a newline is treated as line continuation
- Backslash is the "strongest" quote

## Single Quotes ' '

- Preserves the literal meaning of each character within the quotes. The
  single quote acts as a "toggle" switch. Reading left to right, each '
  toggles quoting on or off
- A literal single quote may not occur in single quoted text, even when
  preceded by a backslash
- Backslashes within single quotes are literal

## ANSI C Quotes `$'\..'

Words of the form `$'string'` are treated specially. The word expands
to string, with backslash-escaped characters replaced as specified by
the ANSI C standard. Backslash escape sequences, if present, are decoded
as follows:

|            |                                                                                                                   |
|:-----------|:------------------------------------------------------------------------------------------------------------------|
| \a         | alert (bell)                                                                                                      |
| \b         | backspace                                                                                                         |
| \e \E      | an escape character (not ANSI C)                                                                                  |
| \f         | form feed                                                                                                         |
| \n         | newline                                                                                                           |
| \r         | carriage return                                                                                                   |
| \t         | horizontal tab                                                                                                    |
| \v         | vertical tab                                                                                                      |
| `\\`       | backslash                                                                                                         |
| \'        | single quote                                                                                                      |
| \"         | double quote                                                                                                      |
| \?         | question mark                                                                                                     |
| \nnn       | the eight-bit character whose value is the **octal** value nnn (one to three octal digits)                        |
| \xHH       | the eight-bit character whose value is the **hexadecimal** value HH (one or two hex digits)                       |
| \uHHHH     | the **Unicode** (ISO/IEC 10646) character whose value is the hexadecimal value HHHH (one to four hex digits)      |
| \UHHHHHHHH | the **Unicode** (ISO/IEC 10646) character whose value is the hexadecimal value HHHHHHHH (one to eight hex digits) |
| \cx        | a control-x character                                                                                             |

## Double Quotes " "

- Preserves the literal value of all characters within the quotes,
  except: `` $ \ ` `` (dollar, backslash, backquote)
- Backslash can escape the following characters: `` $ \ " ` `` or newline.
- A double quote may be quoted within double quotes like so: `\"`
- history expansion (if enabled) will be performed when `!` is
  encountered inside double quotes. This can be bypassed with `\!` But the
  backslash preceding the ! is NOT removed. This only applies to the
  interactive shell, not scripts.
- The parameters `$*` and `$@` have special meaning when in double quotes.

## Quoting Examples

    greeting=hello

    echo $greeting          # hello
    echo \$greeting         # $greeting
    echo '$greeting'        # $greeting
    echo "$greeting"        # hello
    echo "'$greeting'"      # 'hello'
    echo ""$greeting""      # hello
    echo "\"$greeting\""    # "hello"
    echo ~fred              # /home/fred
    echo "~fred"            # ~fred
    echo '~fred'            # ~fred

To insert a single quote in single quoted text,

    $ # Escape literal ' with backslash
    $ echo 'He doesn'\''t have $10'
    He doesn't have $10

    $ # Place literal ' within double quotes
    $ echo 'He doesn'"'"'t have $10'
    He doesn't have $10

    $ # Another way is to use double quotes and escape the $.
    $ echo "He doesn't have \$10"

## The $* and $@ variables

These access all the positional parameters

- passed into the script from the command line
- passed into a funtion
- put into the environment with the "set" command

These behave differently than regular variables.  
Unquoted, `$*` and `$@` behave the same when expanded.

    $ set -- fred barney "Mr. Slate"
    $ for i in $*; do echo $i; done
    fred
    barney
    Mr.
    Slate

The shell word-splits Mr. Slate, even though:

- it was quoted originally
- is stored as a single "word" by the set command.

With quoted `"$*"`, the shell will make the entire set one word, giving

    fred barney Mr. Slate

With quoted `"$@"`, the shell will expand the set without word-splitting,
giving

    fred
    barney
    Mr. Slate

You usually want to use `"$@"`

In the example above:

    for i in "$@";

can be replaced by simply;

    for i; 

The same rules regarding `*` and `@` apply to Bash arrays.

    ${my_array[*]}      all elements, with word splitting.
    ${my_array[@]}      all elements, with word splitting.
    
    "${my_array[*]}"    all elements, as one word.
    "${my_array@*]}"    all elements, no word splitting, 
                        preserves whitespace in elements.
