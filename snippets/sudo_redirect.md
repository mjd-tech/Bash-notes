# sudo with redirection

This contrived example doesn't work. Fails with "permission denied" error:

    sudo ls -alh /root/ > /root/test.out

Redirection is set up **before** any commands (including sudo) are
executed. So a non-root user is trying to write to the /root directory.

Option 1, run a separate shell as root and feed it the original command.

    sudo sh -c 'ls -alh /root/ > /root/test.out'

- Here, you just enclose the original command in single quotes.
- If your command contains quotes itself, you may have to do some "escaping gymnastics".
- Remember, the command is going to get parsed twice, once by the shell
  you are in, and again by the second shell launched with `sh -c`

Option 2, sudo tee (if you have to escape a lot when using the sh -c option):

    sudo ls -alh /root/ | sudo tee /root/test.out > /dev/null

- tee writes to both the screen and a file.
- The redirect to /dev/null is optional, it stops tee from outputting to the screen.
- To append instead of overwriting the output file, use tee -a or tee --append.
- This works because tee is a *command* executed under *sudo*

There are other ways to get the job done, these are the most commonly
used.
