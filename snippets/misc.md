# Misc

## Concatenate multiple files include filename headers

    tail -n +1 file1.txt file2.txt file3.txt

## Sudo Keepalive

    # Might as well ask for password up-front, right?
    sudo -v

    # Keep-alive: update existing sudo time stamp if set, otherwise do nothing.
    while true; do sudo -n true; sleep 60; kill -0 "$$" || exit; done 2>/dev/null &

    # kill -0 "$$" just checks to see if the parent process is running. 
    # It returns true if process is still running.

Sudo options:

    -n, --non-interactive
                     Avoid prompting the user for input of any kind.  If a pass‐
                     word is required for the command to run, sudo will display an
                     error message and exit.
      
    -v, --validate
                     Update the user's cached credentials, authenticating the user
                     if necessary.  For the sudoers plugin, this extends the sudo
                     timeout for another 15 minutes by default, but does not run a
                     command.  Not all security policies support cached creden‐
                     tials.''
