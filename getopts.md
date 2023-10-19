# Getopts
Process options (arguments that begin with dash -), passed into a script or function.
Getopts is a shell builtin command. There is a standalone command getopt (no s). Not covered here.

Note: getopts is not able to parse GNU-style long options (--myoption) or XF86-style long options (-myoption)!

## Usage
`getopts OPTSTRING var`

- OPTSTRING is a list of characters to be interpreted as options.
- If the first character of OPTSTRING is :, suppress default error msg.
- If colon follows any character, it expects an argument.
- Uses builtin variables OPTIND and OPTARG

```bash
myfunc () {
  OPTIND=1      # It's a good idea to set this to 1 before invoking getopts
  while getopts ":ab:c" opt; do
    case $opt in
      a)  echo "option a set" ;;
      b)  echo "option b set with argument: $OPTARG" ;;
      c)  echo "option c set" ;;
      ?) echo "$FUNCNAME: Invalid option: $OPTARG" >&2; return 1 ;;
      :)  echo "$FUNCNAME: Required argument missing: $OPTARG" >&2; return 1 ;;
      # We can also combine these 2 error messages, like so
      # *) echo "$FUNCNAME: Error with option: $OPTARG" >&2; return 1 ;;
    esac
  done
  shift $((OPTIND - 1))  # Point to first non-option argument
  while [[ $1 ]]; do     # shortcut form of [[ -n $1 ]]
    echo "additional argument: $1"
    shift
  done
}

# Normal, no errors
myfunc -a -b fred -c barney wilma

option a set
option b set with argument: fred
option c set
additional argument: barney
additional argument: wilma

# Normal, no errors, combined options
myfunc -ab fred -c barney wilma

option a set
option b set with argument: fred
option c set
additional argument: barney
additional argument: wilma

# Invalid option
$ myfunc -a -b fred -c -z barney wilma
option a set
option b set with argument: fred
option c set
myfunc: Invalid option: z

# Missing argument
$ myfunc -a -b
option a set
myfunc: Required argument missing: b

# However, this is not considered a missing argument
$ myfunc -a -b -c barney wilma
option a set
option b set with argument: -c
additional argument: barney
additional argument: wilma
```
