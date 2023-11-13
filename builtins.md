# Built-in Commands

Bash treats built-ins just like external commands, and expects the same `cmd arg1 arg2 ...` syntax.

| Command     | Example                                      | Description                                                                                                                        |
| ----------- | -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `.`         | `. file [arguments]`                         | Read and execute lines in file. Equivalent to source                                                                               |
| `:`         | `:`                                          | Null command. Returns an exit status of 0 (true).                                                                                  |
| `[`         | `[ expression ]`                             | Evaluate expression and return true or false. Equivalent to test                                                                   |
| `!`         | `! command` or `! pipeline`                  | Negate the return status of a command or pipeline.                                                                                 |
| `alias`     | `alias [options] [name[='cmd']]`             | Assign a shorthand name as a synonym for cmd.                                                                                      |
| `bg`        | `bg [job-id]`                                | Put jobs in the background.                                                                                                        |
| `bind`      | `bind [options][command]`                    | Manage keybindings in the readline library.                                                                                        |
| `break`     | `break [n]`                                  | Exit from a for, while, select, or until loop (or break out of n loops).                                                           |
| `builtin`   | `builtin cmd [arguments]`                    | Run the shell builtin command, bypass any function that redefines a built-in command                                               |
| `caller`    | `caller [expression]`                        | Print the line number and source filename of the current function call or dot file.                                                |
| `cd`        | `cd [options] [dir]`                         | Change the working directory.                                                                                                      |
| `command`   | `command [options] cmd [arg …]`              | Run cmd, bypassing shell aliases or functions with same name as cmd.                                                               |
| `compgen`   | `compgen [options] [string]`                 | Generate possible completions for string according to the options.                                                                 |
| `complete`  | `complete [options] command ...`             | Specifies the way to complete arguments for each command.                                                                          |
| `compopt`   | `compopt [-o\|+o option] [-DE] [name ...]`   | Modify or display completion options.                                                                                              |
| `continue`  | `continue [n]`                               | Skip remaining commands in a for, while, select, or until loop, (or skipping n loops).                                             |
| `declare`   | `declare [options] var[=value]`              | Declare variables and manage their attributes.                                                                                     |
| `dirs`      | `dirs [options]`                             | Print the directory stack, which is managed with pushd and popd.                                                                   |
| `disown`    | `disown [options] [job...]`                  | Remove jobs from list of jobs managed by Bash.                                                                                     |
| `echo`      | `echo [options] [string…]`                   | Write string to standard output.                                                                                                   |
| `enable`    | `enable [options] [command]`                 | Enable or disable shell built-in commands.                                                                                         |
| `eval`      | `eval args`                                  | Read args, expanding all variables, then execute the results.                                                                      |
| `exec`      | `exec [command]`                             | 1. Replace shell with given program. 2. set redirections for current shell.                                                        |
| `exit`      | `exit [n]`                                   | Exit the shell with a status of n. If N is omitted, the exit status is that of the last command executed.                          |
| `export`    | `export [options] [variables]`               | Make variables visible to child processes and subshells.                                                                           |
| `false`     | `false`                                      | Always "fails" and returns exit status "1"                                                                                         |
| `fc`        | `fc [options] [first[last]]`                 | Display or edit commands in the history list.                                                                                      |
| `fg`        | `fg [jobIDs]`                                | Bring job(s) to the foreground.                                                                                                    |
| `getopts`   | `getopts string name [args]`                 | Process command-line arguments (or args, if specified) and check for legal options.                                                |
| `hash`      | `hash [options] [commands]`                  | Manage the list of commands found in the search path ($PATH)                                                                       |
| `help`      | `help [-s] [pattern]`                        | Print info on Bash command(s) matching pattern.                                                                                    |
| `history`   | `history [options]`                          | Print commands in the history list or manage the history file.                                                                     |
| `jobs`      | `jobs [options] [jobIDs]`                    | List running or stopped jobs.                                                                                                      |
| `kill`      | `kill [options]`                             | IDs Send signals to or terminate processes.                                                                                        |
| `let`       | `let expressions`                            | Perform arithmetic as specified by one or more expressions.                                                                        |
| `local`     | `local [options] [name[=value]]`             | Declares local variables for use inside functions.                                                                                 |
| `logout`    | `logout`                                     | Exit a login shell.                                                                                                                |
| `mapfile`   | `mapfile [options] [array]`                  | Read lines from standard input to indexed array variable.                                                                          |
| `popd`      | `popd [options]`                             | Pop top directory off directory stack, and cd.                                                                                     |
| `printf`    | `printf [-v var] format [val …]`             | Formatted printing to stdout or variable.                                                                                          |
| `pushd`     | `pushd [options] [directory]`                | Add directory to the directory stack.                                                                                              |
| `pwd`       | `pwd [options]`                              | Print the current working directory.                                                                                               |
| `read`      | `read [options] [variable1 [variable2 ...]]` | Read one line of standard input and assign each word to the corresponding variable.                                                |
| `readarray` | `readarray [options] [array]`                | Read lines from a file into an array variable.                                                                                     |
| `readonly`  | `readonly [-afp] [variable[=value]...]`      | Prevent the specified shell variables from being assigned new values.                                                              |
| `return`    | `return [n]`                                 | Exit a function with exit status n, or status of last command.                                                                     |
| `set`       | `set [options arg1 arg2 …]`                  | Print all variables known to the shell. Or, manage shell options.                                                                  |
| `shift`     | `shift [n]`                                  | Shift positional arguments (e.g., $2 becomes $1). If n is given, shift to the left n places.                                       |
| `shopt`     | `shopt [options]`                            | Manage shell options                                                                                                               |
| `source`    | `source textfile`                            | Read and execute textfile                                                                                                          |
| `suspend`   | `suspend [-f]`                               | Suspend the current shell. Often used to stop a su session.                                                                        |
| `test`      | `test condition`                             | Evaluate condition and return true or false exit status. See also `[`                                                              |
| `times`     | `times`                                      | Print accumulated process times for user and system.                                                                               |
| `trap`      | `trap [ [commands] signals]`                 | Execute commands if any signals are received.                                                                                      |
| `true`      | `true`                                       | Always "suceeds" and returns exit status "0". Equivalent to the : command.                                                         |
| `type`      | `type [options]`                             | Show whether each command name is an external command, a built-in command, an alias, a shell keyword, or a defined shell function. |
| `typeset`   | `typeset [options]`                          | [variable[=value …]] Identical to declare.                                                                                         |
| `ulimit`    | `ulimit [options] [n]`                       | Print the value of one or more resource limits, or, if n is specified, set a resource limit to n.                                  |
| `umask`     | `umask [options] [mask]`                     | Display file creation mask or set file creation mask.                                                                              |
| `unalias`   | `unalias names`                              | Remove names from the alias list.                                                                                                  |
| `unset`     | `unset [options]`                            | list Erase definitions of functions or variables in list.                                                                          |
| `wait`      | `wait [ID]`                                  | Pause until all background jobs complete, or pause until the specified process ID or job ID completes.                             |
