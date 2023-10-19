# directory - get the directory where the script is installed

``` bash
### Recommended
script_dir="$(dirname "$(readlink -f "${BASH_SOURCE[0]}")")"
```

## Notes

- This technique is used when your application is a "bundle" of files in one directory.
- Your script needs the path to this directory

There is no answer that works in all circumstances.

A script can be invoked in various ways:

- user can cd into the script's directory and then run the script.
- from any directory, user can specify a relative path, or absolute path
- script is in the `$PATH`, user can type script name from any directory
- script is symlinked
- script is sourced, using any of the path options above

Ideally the user would cd into the bundle directory and run the script.
But that's not always the case.

This will work in most cases:

    script_dir="$(cd "$(dirname "$0")" && pwd)"

But it will fail if the script is invoked like this:

    source /path/to/script

So use this (bash specific):

    script_dir="$(cd "$(dirname "{BASH_SOURCE[0]}")" && pwd)"

However, either of these will fail if there is a symlink in the path to the script.

So, provided you have **readlink** installed on your system, this will
work correctly with or without symlinks:

    script_dir="$(dirname "$(readlink -f "${BASH_SOURCE[0]}")")"

No matter what you do, something like this will NOT work

    ssh remotehost bash < ./myscript
    # runs a local script on remotehost

Bash on the remote host cannot access any files in the bundle, because
the bundle is on the local host.
