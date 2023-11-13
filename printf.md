# Printf and Echo

print text to standard output.

- printf and echo do NOT read from STDIN. They work with command line arguments.
- printf does not print a newline by default, echo does

## Printf
`printf [-v var] FORMAT_STRING ARGUMENT(S)`

```bash
printf "foobar\n"               # acts like echo. Uses only format string, no additional arguments
printf "%10s\n" "foobar"        # prints: 3 spaces followed by foobar (10 chars, right justified)
printf "%-10s\n" "foobar"       # prints: foobar followed by 3 spaces (10 chars, left justified)
printf "%.3s\n" "foobar"        # prints: foo (truncate at 3 chars, no padding)
printf "%0.3s\n" "foobar"       # same
printf "%-10.3s\n" "foobar"     # prints: foo followed by 7 spaces (3 string chars + 7 space = 10 chars total)

printf "%'d" 123456             # prints: 123,456 (thousands separator)
printf "%.2f" 123.456           # prints: 123.46 (floating point, rounds to 2 deciaml places)
printf "%-10.2f" 123.456        # prints: 123.46 followed by 4 spaces (10 chars, left justified)
printf '%010.2f\n' 123.456      # prints: 0000123.46 (pad with zeros instead of spaces)
printf "%'.2f" 1234.567         # prints: 1,234.57   (thousands separator, rounds to 2 decimal places)

printf "%d\n" 0xff              # prints: 255   (hex to decimal)
printf "%x\n" 255               # prints: ff    (decimal to hex)
printf "%#x\n" 255              # prints: 0xff  (decimal to hex)

printf "%d\n" 377               # prints: 255   (octal to decimal)
printf "%o\n" 255               # prints: 377   (decimal to octal)

printf "%x\n" "'a"              # prints: 0x61  (character to hex)
printf "%b\n" "\x61"            # prints: a     (hex to character)

printf "%o\n" "'a"              # prints: 141   (character to octal)
printf "%b\n" "\0141"           # prints: a     (octal to character)

printf '%(%F)T\n' -1            # prints: current date in YYYY-MM-DD format (-1 denotes current time)

# Print dashes under a string, the length of the string:
str="Hello World"
printf '%*s\' "${#str}" | tr ' ' '-'
# Result
Hello World
-----------

# Generate a string of spaces the width of the terminal, assign to variable str
printf -v str '%*s' "$(tput cols)" "")
# Uses special * option that specifies length as an argument,
# Instead of trying to embed the tput command in the format string.
# Optionally, change spaces to dash
str=${str// /-}
```

### Format string

- ordinary characters - printed unchanged to the standard output.
- format specifiers - example: %10s = print with minimum length of 10
- escape sequences - example: \n = newline character.
- put format strings in quotes to protect it from the shell.
- weird things happen when # of format specifiers != # of arguments

### Format Specifiers
(see `man 3 printf` for complete list)

| Format | Description                                                    |
| ------ | -------------------------------------------------------------- |
| `%s`   | String, do not interpret backslash escapes                     |
| `%b`   | Like %s, does interpret backslash escapes                      |
| `%q`   | Prints argument **shell-quoted**, reusable as input            |
| `%d`   | Integer                                                        |
| `%x`   | Hexadecimal, lower-case (a-f)                                  |
| `%#x`  | same, with leading `0x`                                        |
| `%X`   | Hexadecimal, upper-case (A-F)                                  |
| `%#X`  | same, with leading `0X`                                        |
| `%f`   | Floating point, defaults to 6 decimal places                   |
| `%e`   | Floating point, in exponential notation `12345±e10` format     |
| `%E`   | Same as `%e`, but with an upper-case `E` in the printed format |
| `%%`   | Produces a literal `%` (percent sign)                          |

### Format modifiers

These are specified **between** the introductory `%` and the character
that specifies the format:

| format  | description                                                                                             |
| :------ | :------------------------------------------------------------------------------------------------------ |
| `N`     | Specifies a **minimum field width**, if text is shorter, pad with spaces, if longer, print entire field |
| `.`     | Together with a field width, **truncate** when the text is longer                                       |
| `*`     | the width is supplied as an argument. Usage (`printf "%*s\n" 20 "test string"`)                         |
| `-`     | **Left-bound** text printing in the field (standard is **right-bound**)                                 |
| `0`     | Pads numbers with zeros, not spaces                                                                     |
| `space` | Pad a positive number with a space, where a minus (`-`) is for negative numbers                         |
| `+`     | Prints all numbers **signed** (`+` for positive, `-` for negative)                                      |
| `'`     | **A Single Quote**: Display the thousands grouping separator for integers.                              |

### Escape codes

- in the format string, or 
- in an argument, if using `%b` format

| Code         | Description                                                          |
| ------------ | -------------------------------------------------------------------- |
| `\"`         | double quote                                                         |
| `\'`         | single quote                                                         |
| `\?`         | Prints a `?`                                                         |
| `\\`         | backslash                                                            |
| `\a`         | alert (BEL) (ASCII code 7 decimal)                                   |
| `\b`         | backspace                                                            |
| `\c`         | produce no further output                                            |
| `\e`         | escape                                                               |
| `\f`         | form feed                                                            |
| `\n`         | newline                                                              |
| `\r`         | carriage-return                                                      |
| `\t`         | horizontal tab                                                       |
| `\v`         | vertical tab                                                         |
| `\0nnn`      | eight-bit character with octal value nnn (one to three octal digits) |
| `\xHH`       | eight-bit character with hex value HH (one or two hex digits)        |
| `\uHHHH`     | Unicode character with hex value HHHH (one to four hex digits)       |
| `\UHHHHHHHH` | Unicode character with hex value HHHHHHHH (one to eight hex digits)  |


## Echo

    echo [-neE] [arg ...]

`echo` outputs its args to stdout, separated by spaces, followed by a newline.
The return status is always `0`.
`echo` doesn't interpret `--` as the end of options, and will simply print this string if given.

### Options

| Option | Description                          |
| :----- | :----------------------------------- |
| `-n`   | No trailing newline                  |
| `-e`   | Interpret escape codes (like printf) |

