# Redirections

Created by Peteris Krumins (peter@catonmat.net) www.catonmat.net -- good
coders code, great coders reuse

| Redirection                                            | Description                                                                                                                                                            |
|:-------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `cmd > file`                                            | Redirect the standard output (stdout) of cmd to a file.                                                                                                                |
| `cmd 1> file`                                           | Same as above. 1 is the default file descriptor for stdout.                                                                                                            |
| `cmd 2> file `                                          | Redirect the standard error (stderr) of cmd to a file. 2 is the default file descriptor for stderr.                                                                    |
| `cmd >> file`                                          | Append stdout of cmd to a file.                                                                                                                                        |
| `cmd 2>> file`                                         | Append stderr of cmd to a file.                                                                                                                                        |
| `cmd &> file`                                           | Redirect stdout and stderr to a file.                                                                                                                                  |
| `cmd > file 2>&1`                                      | Another way to redirect both stdout and stderr of cmd to a file. This *is not* the same as cmd 2>&1 > file. Redirection order matters!                             |
| `cmd > /dev/null`                                       | Discard stdout of cmd.                                                                                                                                                 |
| `cmd 2> /dev/null`                                      | Discard stderr of cmd.                                                                                                                                                 |
| `cmd &> /dev/null`                                      | Discard stdout and stderr.                                                                                                                                             |
| `cmd < file`                                            | Redirect the contents of the file to the stdin of cmd.                                                                                                                 |
| `cmd << EOL ... EOL`                                   | Here-doc syntax - Redirect the text between the EOL markers to the stdin of cmd                                                                                        |
| `cmd <<- EOL ... EOL`                                  | Here-doc syntax - ignore leading tabs                                                                                                                                  |
| `cmd <<< "string"`                                    | Here-string syntax - same as here-doc but without EOL markers                                                                                                          |
| `exec 2> file`                                          | Redirect stderr of all commands to a file                                                                                                                              |
| `exec 3< file`                                          | Open a file for reading using a custom file descriptor.                                                                                                                |
| `exec 3> file`                                          | Open a file for writing using a custom file descriptor.                                                                                                                |
| `exec 3<> file`                                        | Open a file for reading and writing using a custom file descriptor.                                                                                                    |
| `exec 3>&-`                                             | Close a file descriptor.                                                                                                                                               |
| `exec 4>&3`                                             | Make file descriptor 4 to be a copy of file descriptor 3. (Copy fd 3 to 4.)                                                                                            |
| `exec 4>&3-`                                            | Copy file descriptor 3 to 4 and close fd 3.                                                                                                                            |
| `echo "foo" >&3`                                        | Write to a custom file descriptor.                                                                                                                                     |
| `cat <&3`                                               | Read from a custom file descriptor.                                                                                                                                    |
| `(cmd1; cmd2) > file`                                   | Redirect stdout from multiple commands to a file (using a sub-shell).                                                                                                  |
| `{ cmd1; cmd2; } > file`                                | Redirect stdout from multiple commands to a file (faster; not using a sub-shell).                                                                                      |
| `cmd <(cmd1)`                                           | Redirect output of cmd1 to an anonymous fifo, then pass the fifo to cmd as an argument. Useful when cmd doesn't read from stdin directly and expects a file.           |
| `cmd < <(cmd1)`                                        | Redirect stdout of cmd1 to an anonymous fifo, then redirect the fifo to stdin of cmd. Best example: diff <(find /path1 | sort) <(find /path2 | sort)               |
| `cmd <(cmd1) <(cmd2)`                                  | Redirect stdout of cmd1 cmd2 to two anonymous fifos, then pass both fifos as arguments to cmd                                                                          |
| `cmd1 >(cmd2)`                                          | Run cmd2 with its stdin connected to an anonymous fifo, and pass the filename of the pipe as an argument to cmd1.                                                      |
| `cmd1 | cmd2`                                           | Redirect stdout of cmd1 to stdin of cmd2. Pro-tip: This is the same as cmd1 > >(cmd2), same as cmd2 < <(cmd1), same as > >(cmd2) cmd1, same as < <(cmd1) cmd2. |
| `cmd1 |& cmd2`                                          | Redirect stdout and stderr of cmd1 to stdin of cmd2 (bash 4.0+ only). Use cmd1 2>&1 cmd2 for older bashes.                                                            |
| `cmd | tee file`                                        | Redirect stdout of cmd to a file and print it to screen.                                                                                                               |
| `exec {filew}> file`                                    | Open a file for writing using a **named file descriptor** called {filew} (bash 4.1+)                                                                                   |
| `cmd 3>&1 1>&2 2>&3`                                  | Swap stdout and stderr of cmd.                                                                                                                                         |
| `cmd > >(cmd1) 2> >(cmd2)`                           | Send stdout of cmd to cmd1 and stderr of cmd to cmd2.                                                                                                                  |
| `cmd1 | cmd2 | cmd3 | cmd4; echo ${PIPESTATUS[@]}` | Find out the exit codes of all piped cmds.                                                                                                                             |

## Detail

- A File Descriptor (FD) is a positive integer which refers to an open file.  
- Every process inherits three open FDs from its parent: 0 ("standard
input"), open for reading; 1 ("standard output") and 2 ("standard
error"), open for writing.  
- A process that is started without one or more of these may behave
unpredictably. (So never close stderr. Always redirect to /dev/null
instead.)  

Redirection involves changing the default File Descriptors to somewhere else:

A common example:

Redirect both stdout and stderr to a file...

    some_command 2>&1 > some_file  WRONG!
    some_command >some_file 2>&1   CORRECT!

Why is the first one wrong? Because order matters. It goes left to right.  
The first one reads: redirect stderr to where stdout is pointing right
now (the screen), then redirect stdout to a file.  
The second one reads: redirect stdout to a file, then redirect stderr to
where stdout is pointing right now (the file).
