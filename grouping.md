# Grouping commands
Use `{ commands }` or `( commands )`

## Within same shell

```bash
# Avoids redirecting each command individually.
{
  echo "PASSWD follows"
  cat /etc/passwd
  echo
  echo "GROUPS follows"
  cat /etc/group
} >output.txt

# One liner: spaces around braces. last command terminated with ;
{ echo foo; cat somefile; echo bar; } > output.txt

# Any properly terminated compound command will work without an extra terminator.
{ while sleep 1; do echo ZzZzzZ; done }
```

## Body of a function:
```bash
print_help() {
  echo "Options:"
  echo "-h           This help text"
  echo "-f FILE      Use config file FILE"
  echo "-u USER      Run as user USER"
}
```

## Within a subshell
Example: Execute a command in a different directory.

``` bash
echo "$PWD"
( cd /usr; echo "$PWD" )  # displays /usr
echo "$PWD"               # Still in the original directory.
```

