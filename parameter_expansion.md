# Variable Expansions
The Bash manual uses the term "parameter expansion" to mean "the value of a variable"

In these examples, foo is a variable, bar is a literal string.

## Curly Braces

| Syntax      | Description                                     |
| ----------- | ----------------------------------------------- |
| `$foo`      | The value of foo                                |
| `${foo}`    | Same.                                           |
| `${foo}bar` | The value of foo, concatenated with literal bar |

## Variable is Unset/null

| Syntax        | Description                                                                           |
| ------------- | ------------------------------------------------------------------------------------- |
| `${foo:-bar}` | If foo unset or null, then bar, else the value of foo.                                |
| `${foo:=bar}` | If foo unset or null, then set foo=bar, else the value of foo.                        |
| `${foo:?bar}` | If foo unset or null, then exit script and write bar to stderr, else the value of foo |
| `${foo:+bar}` | If foo unset or null, then nothing is substituted, else bar is substituted.           |

Note: A variable can be **unset** (does not exist) or it can exist, but have **null** value. There's a difference.  
if colon (:) is omitted, then only test for unset, not for null.

Some scripts may have "set -o nounset" ( No unset variables allowed).
But sometimes you don't know if a variable is going to be unset, you can test it like so:

    if [ "${foo:-}" = something ]; then...

So if foo is unset, the null value is substituted, and the script won't error out.

## Length

| Syntax       | Description              |
| :----------- | :----------------------- |
| `${#foo}`    | length of foo            |
| `${#foo[@]}` | number of array elements |

## Chop

All "patterns" are shell "glob" type of patterns, not regular expressions.

| Syntax            | Description                                                                         |
| :---------------- | :---------------------------------------------------------------------------------- |
| `${foo#pattern}`  | Remove from Beginning: Delete the *shortest* part that matches and return the rest. |
| `${foo##pattern}` | Remove from Beginning: Delete the *longest* part that matches and return the rest.  |
| `${foo%pattern}`  | Remove from End: Delete the *shortest* part that matches and return the rest.       |
| `${foo%%pattern}` | Remove from End: Delete the *longest* part that matches and return the rest.        |

Note: One way to remember these is to look at the keyboard keys 3,4,and 5.
`#` removes the beginning of string `$`, and `%` removes the end

## Slice

| Syntax                    | Description                                 |
| ------------------------- | ------------------------------------------- |
| `${foo:offset}`           | from offset to end                          |
| `${foo:offset:length}`    | from offset, length characters              |
| `${foo[@]:offset}`        | array elements from offset to last element  |
| `${foo[@]:offset:length}` | array elements from offset, length elements |

## Find/Replace

| Syntax                | Description                                                           |
| :-------------------- | :-------------------------------------------------------------------- |
| `${foo/pattern/bar}`  | Find pattern (greedy), within foo, replace *first* occurance with bar |
| `${foo//pattern/bar}` | Find pattern (greedy), within foo, replace *all* occurances with bar  |
| `${foo/#pattern/bar}` | if foo begins with pattern, replace match with bar                    |
| `${foo/%pattern/bar}` | if foo ends with pattern, replace match with bar                      |

## Upper/Lower case

| Syntax            | Description                              |
| :---------------- | :--------------------------------------- |
| `${foo,pattern}`  | First character in pattern to lower case |
| `${foo,,pattern}` | All characters in pattern to lower case  |
| `${foo^pattern}`  | First character in pattern to upper case |
| `${foo^^pattern}` | All characters in pattern to upper case  |

Pattern is optional. If no pattern, operate on entire variable foo.

## Bang
| Syntax        | Description                                                                            |
| ------------- | -------------------------------------------------------------------------------------- |
| `${!var}`     | Reference. Expands to the contents of the variable name held by var.                   |
| `${!prefix*}` | Expands to the names of variables whose names begin with prefix.                       |
| `${!prefix@}` | As above, if used within double quotes, each variable name expands to a separate word. |
| `${!foo[@]}`  | Expands to list of array indices                                                       |

## Transform

| Syntax        | Description                                              |
| ------------- | -------------------------------------------------------- |
| `${foo@U}`    | all upper-case                                           |
| `${foo@u}`    | first character upper-case                               |
| `${foo@L}`    | all lower-case                                           |
| `${foo@Q}`    | quoted, can be re-used as input                          |
| `${foo@E}`    | expand backslash escapes like the $'…' quoting mechanism |
| `${foo[@]@K}` | Print key-value pairs of array                           |

