# Process Substitution
Use the output of commands like a file.

## Examples

```bash
# diff (side by side) a local file with a remote file. (diff works with FILES, not STDOUT)
diff -y /etc/hosts <(ssh remotehost cat /etc/hosts)

# Read lines from the output of a command.
while read line; do
  command(s)...
done < <(sort file1.txt)

# Log ouput of script and display on screen
exec > >(tee logfile)               # STDOUT only
exec 2> >(tee logfile)              # STDERR only
exec > >(tee logfile): exec 2>&1    # BOTH
# Note: prompts for user input may appear strange.
```

## How it works
- create tempfile in `/dev/fd/`
- runs command(s), redirect STDOUT to tempfile
- The filename is then used as a substitution for the `<(...)` or `>( ... )` construct.
