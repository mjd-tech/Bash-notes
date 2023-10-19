# Patterns and "Globbing"

BASH offers three kinds of pattern matching.

- glob
- extended glob
- regular expressions

## Glob

| Pattern   | Description |
|-----------|--------------------------------------------------------------- |
| `*`        | Matches any string, of any length                             |
| `foo*`     | Matches any string beginning with foo                         |
| `*foo*`    | Matches any string containing foo (beginning, middle or end)  |
| `*.tar.gz` | Matches any string ending with .tar.gz                        |
| `*.[ch]`   | Matches any string ending with .c or .h but not .ch           |
| `foo?`     | Matches food or foot but not foobar                           |

When matching **filenames**, the `*` and `?` characters will NOT match a slash (/) character.  
When matching **strings**, the `*` and `?` characters WILL match a slash (/) character.

With filename globbing, filenames are substituted **after word splitting**,
all resulting filenames are literal and each filename is a separate word,
even if filenames have spaces in them.

To disable filename globbing: `set -f`

### Rules for brackets [ ]

If the first character following the `[` is a `!` or a `^` then any
character NOT enclosed is matched. A literal `-` may be matched by
including it as the first or last character in the set. A literal `]`
may be matched by including it as the first character in the set.

### Character classes

| Class        | Description                               | Equivalent                                      |
|:-------------|:------------------------------------------|:------------------------------------------------|
| `[:alnum:]`  | Alphanumeric (letters or digits)          | `a-zA-Z0-9`                                   |
| `[:alpha:]`  | Alphabetic (upper and lower case letters) | `a-zA-Z`                                      |
| `[:blank:]`  | Space and tab                             | ` \t`                                         |
| `[:cntrl:]`  | Control characters                        | `octal codes 000 through 037, and 177 (DEL)`   |                            |
| `[:digit:]`  | Numbers in decimal                        | `0-9`                                         |
| `[:graph:]`  | Printable characters excluding space      | `[:alnum:]` and `[:punct:]`                   |
| `[:lower:]`  | Lowercase letters                         | `a-z`                                         |
| `[:print:]`  | Printable characters including space      | `[:alnum:]` `[:punct:]` and space             |
| `[:punct:]`  | Punctuation and symbols                   | `!"#$%&'() ...etc`                              |
| `[:space:]`  | All whitespace characters                 | ` \t\r\n\v\f`                                 |
| `[:upper:]`  | Uppercase letters                         | `A-Z`                                         |
| `[:xdigit:]` | Hexadecimal digits                        | `A-Fa-f0-9`                                   |

**Note:** In actual usage these character classes will be inside brackets, thus:  
`[[:alpha:]]`` (match a letter), but could also be ``[[:alpha:]7]` (match a letter or 7)

### globstar
Off by default. Enable with: `shopt -s globstar`

The standard `*` matches files and directories at the current level.  
With **globstar**, `**` matches zero or more directories.  

Example: `**/*.mp3` matches `*.mp3` files recursively: in current directory,
and all subdirectories below.

### dotglob and nullglob

Off by default. Enable with: `shopt -s dotglob nullglob`

By default, `*` doesn't match hidden files.  
Doing `.*` matches hidden files, but also matches the . and .. directory entries (this dir and the parent dir).
Probably not what you want.

**dotglob** fixes this. But in an empty directory, this happens:

    ls *
    ls: cannot access '*': No such file or directory

Probably not what you want.  
**nullglob** fixes this, so that `ls *` displays nothing in an empty directory.

### Extended Glob

Off by default. Enable with: `shopt -s extglob`

    ?(pattern-list)  Matches ZERO or ONE occurrence of the given patterns
    *(pattern-list)  Matches ZERO or MORE occurrences of the given patterns
    +(pattern-list)  Matches ONE or MORE occurrences of the given patterns
    @(pattern-list)  Matches exactly one of the given patterns
    !(pattern-list)  Matches anything EXCEPT one of the given patterns

The list inside the parentheses is a list of globs or extended globs
separated by the pipe `|` character.

### Extended Glob Example

    $ shopt -s extglob
    $ ls
    names.txt  tokyo.jpg  california.bmp
    $ echo !(*jpg|*bmp)
    names.txt


## Regular Expressions

- Regex can **only** be used with the Bash `[[` keyword in Bash 3.0 or later.
- **cannot** be used for filename matching.
- The syntax is: `[[ string =~ regex ]]`, usually used within an `if...then...else` statement

| Condition                     | exit code                         |
|-------------------------------|-----------------------------------|
| string matches pattern        | 0 (true)                          |
| string does not match pattern | 1 (false)                         |
| invalid pattern syntax        | shell **aborts** with exit code 2 |

- Bash uses the **Extended Regular Expression** (ERE) dialect.
- The **BASH_REMATCH** array variable holds the results of Regular
  Expressions that use **capturing groups** (parentheses).

The entire matched string: `${BASH_REMATCH[0]}`
First capture group: `${BASH_REMATCH[1]}`

For best results:

- put the regex in a **variable**, single quoted.
- use this variable, **unquoted** in the `[[` statement
- if you quote the regex, it gets treated as a literal string.

### Regular Expression Example

Here we capture the `$LANG` environment variable into 2 capture groups,
and display the results.

``` bash
# First, let's see what the $LANG environment variable is
echo $LANG
# Result:
en_US.UTF-8

# Put the regex in a variable, single quoted
regex='(..)_(..)'

# Use the regex, unquoted
if [[ $LANG =~ $regex ]]; then
    echo "Country code is ${BASH_REMATCH[2]}"
    echo "Language code is ${BASH_REMATCH[1]}"
else
    echo "Your locale is not recognized"
fi

# Result:
Country code is US
Language code is en
```

