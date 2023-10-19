# program_exists - Check if executable is installed

you need to use some external program, and you want to make sure it is
installed. Commonly, you see something like:

    foo=command_to_find
    which "$foo" >/dev/null || { echo "Command "$foo" not found" >&2; exit 1; }

Don't use *which*. Many operating systems have a *which* that doesn't
even set an exit status, or they modify *which* to like change the
output or even hook into the package manager.

For Bash scripts, use the *hash* builtin. *hash* has the side-effect
that the command's location will be "hashed" (for faster lookup next
time you use it). This is usually what you want, you are checking if a
command exists because you intend to actually use it. *hash* will output
on standard error if command not found, so there's no need to echo
"command not found". Of course you can if you want to. If the command
*is* found, *hash* outputs nothing and returns 0 (true), so no need to
redirect standard out to /dev/null.

    foo=command_to_find
    hash "$foo" || { echo "Command "$foo" not found." >&2; exit 1; }

In Bash you can also search for built-in commands and keywords using
*type*. *type* also finds external commands.

    type foo >/dev/null 2>&1 || { echo "Command "$foo" not found." >&2; exit 1; }

For POSIX shell scripts, use:

    command -v foo >/dev/null 2>&1 || { echo "Command "$foo" not found." >&2; exit 1; }
