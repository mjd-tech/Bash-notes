# Arithmetic expansion

    # Substitute the result of expression. The output is one "word" and a "numeric string".
    $(( <EXPRESSION> ))
    
    # Evaluate the expression and optionally do something with the return code of the command.
    (( <EXPRESSION> ))

- Do not need to backslash-escape special chars like `< >`
- Variables within the ((...)) syntax do not need a leading `$`

``` bash
int=1
echo $(( 2 + int ))     # 3
echo $((2+int))         # 3, but harder to read
echo $(( 2 + $int ))    # 3, but don't need $
echo $(( 2 > int ))     # 1, "arithmetic" true

(( 2 > int )) && echo yes || echo no    # yes
(( 2 == int )) && echo yes || echo no   # no
(( 2 = int )) && echo yes || echo no    # error, tries to assign $int to 2, use ==

arr=(1 2 3)
echo $(( arr[1] + 2 ))  # 4, don't need $ or braces {}, arrays are zero-based

echo $((x))       # Good.
echo $(($x))      # Ok. But you don't need the $. Can use variables directly.
echo $(("$x"))    # Error. expands to $(("1")), an invalid expression.
echo $((x[0]))    # Good.
echo $((${x[0]})) # Ok. But you don't need the $ and the braces.
echo $(($x[0]))   # Error. expands to $((1[0])), an invalid expression.
                  # Either get rid of $ or use braces.
```
## Truth

- if the arithmetic expression evaluates to NOT 0 (arithmetic true), it
  returns 0 (shell true)
- if the arithmetic expression evaluates to 0 (arithmetic false), it
  returns 1 (shell false)

That means, the following `if`-clause will execute the `else`-thread:

``` bash
if ((0)); then
  echo "true"
else
  echo "false"
fi

# Result: false
```

## Operators

| Operator      | Meaning                                            |
| :------------ | :------------------------------------------------- |
| `++ --`       | Increment and decrement, prefix and postfix        |
| `+ - ! ~`     | Unary plus and minus; logical and bitwise negation |
| `* / %`       | Multiplication, division, and remainder            |
| `+ -`         | Addition and subtraction                           |
| `**`          | Exponential                                        |
| `<< >>`       | Bit-shift left and right                           |
| `< <= > >=`   | Comparisons                                        |
| `== !=`       | Equal and not equal **comparisons**                |
| `&`           | Bitwise AND                                        |
| `^`           | Bitwise Exclusive OR                               |
| `\|`          | Bitwise OR                                         |
| `&&`          | Logical AND (short-circuit)                        |
| `\|\|`        | Logical OR (short-circuit)                         |
| `?:`          | Conditional expression                             |
| `= += -= *=`  | Assignment operators                               |
| `/= %= &= ^=` |                                                    |
| `<<= >>= \|=` |                                                    |

POSIX shell only supports these:

`+ - * /`  
`%` (modulus)  
`& |` (bitwise and/or)

Parentheses can be used to group subexpressions

``` bash
echo $(( (13+2) / (8-3) ))  # displays 3
```

**Variables** used inside the arithmetic expansion, as in all arithmetic
contexts, can be used without the leading `$`