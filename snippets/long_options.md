# "Long" options - in Bash Scripts

The getopts built-in handles "short" options, but not long options (with
a double dash).

Here is one way to handle both types, including combined short options.

``` bash
while test $# -gt 0
do
  case $1 in

  # Normal option processing
    -h | --help)
      # usage and help
      ;;
    -v | --version)
      # version info
      ;;
  # ...

  # Special cases
    --)
      break
      ;;
    --*)
      # error unknown (long) option $1
      ;;
    -?)
      # error unknown (short) option $1
      ;;
      
  # FUN STUFF HERE:
  # Split apart combined short options
    -*)
      split=$1
      shift
      set -- $(echo "$split" | cut -c 2- | sed 's/./-& /g') "$@"
      continue
      ;;

  # Done with options
    *)
      break
      ;;
  esac

  # for testing purposes:
  echo "$1"

  shift
done
```
