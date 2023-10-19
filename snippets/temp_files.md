# Temporary Files and Directories

In general, use **mktemp** to safely create temporary files and
directories, using optional TEMPLATE  
Use an exit trap to remove the temp file/directory when script exits.

## Temp File

```bash
# creates /tmp/tmp.D54WuSSZBp (10 random characters after the dot)
tmpfile=$(mktemp)

# error checking, protect tmpfile variable
tmpfile=$(mktemp) && readonly tmpfile || exit 1

# Meaningful name - uses TEMPLATE - creates /tmp/script_name.c5W (3 random characters)
tmpfile=$(mktemp -p '' "${0##*/}".XXX
-or-
tmpfile=$(mktemp --tmpdir "${0##*/}".XXX

# Create tempfile in RAM (if your /tmp is not mounted as tmpfs)
tmpfile=$(mktemp -p /dev/shm)
tmpfile=$(mktemp -p /dev/shm/ "${0##*/}".XXX) && readonly tmpfile || exit 1

# Remove tempfile when script exits
trap 'rm -f "$tmpfile"' EXIT
```

## Temp Directory

If you need more than one temp file, you can create a temp directory,
and put your temp files in there.  
When the script exits, remove the entire temp directory.

```bash
# creates directory /tmp/tmp.eb65RXGtAu (10 random characters)
tmpdir=$(mktemp -d)

# with error checking, protect variable
tmpdir=$(mktemp -d) && readonly tmpdir || exit 1

# More meaningful name - creates /tmp/script_name.s5k (3 random characters)
tmpdir=$(mktemp --tmpdir -d "${0##*/}".XXX) && readonly tmpdir || exit 1
-or-
tmpdir=$(mktemp -p '' -d "${0##*/}".XXX) && readonly tmpdir || exit 1

# Create in RAM (if your /tmp is not mounted as tmpfs)
tmpdir=$(mktemp -d /dev/shm/"${0##*/}".XXX) && readonly tmpdir || exit 1

# Remove temp dir and all its contents when script ends
trap '[[ -d $tmpdir ]] && rm -rf "$tmpdir"' EXIT
```

You can give your temp files meaningful file names within the temp directory.

```bash
# Example, create temp file "mydirs" within $tmpdir
find $HOME -maxdepth 1 -type d > "$tmpdir/mydirs"
```

### How to use TEMPLATE

- TEMPLATE must contain at least 3 consecutive 'X's in last component.
- If TEMPLATE is not specified, use tmp.XXXXXXXXXX, and --tmpdir is implied.


```bash
# Create foo.3fg (3 random characters) in **current working directory**. Probably **not** what you want.
mktemp foo.XXX   

# Create in /tmp directory, explicitly
mktemp /tmp/foo.XXX

# Create in $TMPDIR, if set, else /tmp
mktemp --tmpdir foo.XXX
mktemp -p '' foo.XXX

# but NOT this
mktemp -p foo.XXX  # fails
```

## mktemp Notes

Usage: `mktemp [OPTION]... [TEMPLATE]`

| -d, --directory           | create a directory, not a file                         |
|-------------------------- |--------------------------------------------------------|
| -u, --dry-run             | do not create anything; merely print a name (unsafe)   |
| -q, --quiet               | suppress diagnostics about file/dir-creation failure   |
| --suffix=SUFF             | append SUFF to TEMPLATE; SUFF must not contain a slash |
| -p DIR, --tmpdir `[=DIR]` | interpret TEMPLATE relative to DIR                     |
| --help                    | display help and exit                                  |
| --version                 | output version information and exit                    |

- Files are created with u+rw permissions, and directories u+rwx
- mktemp prints the full path name to stdout
- By default, mktemp creates files/directories in `$TMPDIR`
- If `$TMPDIR` environment variable is not set (it usually isn't), use /tmp
- Override this by using full path in TEMPLATE
- or using the -p or --tmpdir options

Trap:

- In BASH, the EXIT signal alone is adequate to run the trap when the script exits for any reason.
- In other shells, like dash or zsh, it may be necessary to specify HUP INT TERM EXIT signals

RAMdisk - /dev/shm

- When using /dev/shm, **Don't make huge temp files**
- Don't set \$TMPDIR to /dev/shm
- Many Linux systems mount /tmp as tempfs, which is a RAMdisk

### Paranoid Version - Using TMPDIR environment variable
The concern is that an attacker could anticipate the name of your
tempfile, and create symlinks in /tmp that point to critical system
files  
and your script would possibly overwrite one of these files.

- Besides the temp files you create yourself, your Bash script may
  invoke other commands that use temp files as well.
- By default TMPDIR is unset, and temp files are created in /tmp
- Setting and exporting the TMPDIR environment variable ensures any temp
  files created by your script will reside in the TMPDIR

```bash
export TMPDIR=/home/someuser/tmp/  # this directory must exist, create it if necessary
tmpfile=$(mktemp)
trap 'rm -f "$tmpfile"' EXIT
```

### Another way

- This example uses *git show* to display a previously committed file in
  the "pluma" GUI text editor (to see syntax highlighting)
- It creates a temp file, launches pluma in the background, sleeps 1
  second then deletes the temp file
- temp file is set read-only, pluma displays read-only in the title bar,
  to remind you this is just to look at.
- sleep is needed because pluma needs time to initialize itself before
  it loads the temp file

``` bash
git log --oneline
# copy the commit ID, for example d275b89
git show d275b89:myfile > d275b89.myfile  # prepend ID so you don't clobber the current myfile
chmod a-w d275b89.myfile    # make it read-only
# need to sleep before removing tempfile,
pluma d275b89.myfile & sleep 1; unlink d275b89.myfile  # unlink is safer than rm
```
