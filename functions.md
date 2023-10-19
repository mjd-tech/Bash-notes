# Functions

- Really just subroutines
- Must be defined before it is called
- Can process arguments passed to them and return an exit status
- Arguments are processed as postitional parameterers (`$1`, `$2`, etc)
- Can use "shift" with these positional parameters.
- Arguments are "passed by value" not "reference"
- Variable names (without the dollar sign `$`) passed as arguments are treated as string literals.
- Functions return the exit status of last command in the function
  unless an explicit "return" statement used.
- Exit status is limited to 0-255.

## Define a Function

```bash
myfunc () {
    some code here
}

# Can also have opening brace on a new line
myfunc ()
{
    some code here
}

# One line. Notice the semi-colon ; and a space before closing brace.
myfunc () { some code here; }

# Can also have function run in a subshell.
# Use parentheses instead of braces.
myfunc ()
(
    some code here
)
```

## Call a Function
```bash
# Function must be defined before it is called.
# Shell reads and executes a script line by line. 

myfunc () { echo "Hello World"; }
myfunc      # Hello World.
```

## Default exit status
By default, a function's exit status is the exit status of the last
command executed by the function.
```bash
myfunc () { echo "Hello, World"; }
myfunc; echo $?                     # Hello, World (newline) 0
```

## Using "return" to set exit status
The function ends when it encounters "return". Can optionally supply a
return value, the largest value possible is 255.

```bash
myfunc ()
{
    cd /some/dir || { 
      echo "Can't cd to /some/dir"    # This command has exit status 0
      return 1                        # So set function's exit status here
    }
    do something here               # Won't run if cd didn't work
}
```

## Local variables
Can set and use global variables directly. Variables declared as local
are only in scope within the function.


```bash
foo=Fred
myfunc () { local foo=Barney; echo $foo; } 
myfunc                      # Barney.
echo $foo                   # Fred. foo not changed in function.
```

Local variables can also be used within a Block of Code.
```bash
foo=Fred
{ local foo=Barney; echo $foo; }    # Barney.
echo $foo                           # Fred.
```
## Pass parameters to a function

Functions use positional parameters.

- `$0` is the function's name.
- `$1`, `$2`, `$3` etc. are the function's arguments.
- `$#` is the number of arguments, excluding `$0`.

```bash
add2num () 
{
    [ $# -ne 2 ] && { echo "Needs two numbers"; return 1; } 
    echo $(( $1 + $2 ))     # This does integers only
}
```

## Set a variable by calling a function
Use echo or printf in the function.  
Call function using command substitution.

```bash
myfunc () {
    echo "$(( $1 + $2 ))"
}
result=$(myfunc 2 3)
echo $result            # 5.
```

Use a global variable to receive results of function.

```bash
result=
myfunc () { result=$(( $1 + $2 )); }
myfunc 2 3
echo $result            # 5.
```

## Pass variables by reference
To do this, pass the name of the variable to the function (without the
leading `$`). Then use eval within the function.

First way. Put an escaped `$` in the eval string using \\

```bash
foo=Fred
myfunc () {
    eval echo "Value of $1 is \$$1"
}
myfunc foo      # Value of foo is Fred
```
Alternate way. Enclose eval string in single quotes. Unquote the
referenced variable.

```bash
myfunc () {
    eval 'echo "Value of $1 is $'$1'"'
}
```
Use indirect variable references:
```bash
foo=fred; bar=foo; echo ${!bar}
fred
```
