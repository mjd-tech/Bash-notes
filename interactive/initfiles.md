# .profile vs .bashrc

These are configuration files for Bash. The set environment variables
such as PATH

In an nutshell:

- ~/.profile - login shells
- ~/.bashrc - non-login shells

Typically, the .profile sources .bashrc, and .bashrc determines if shell
is interactive.

## login vs non-login

Non-login Shell

- When you open a terminal emulator like gnome-terminal or
  mate-terminal. It doesn't ask for a login, you use it straight away.

Login shell:

- The Desktop GUI login that you see when you first power up.
- Using Ctrl+Alt+F1 (F2,F3,etc) to launch a virtual terminal.
- SSH connections.
- simulating an initial login shell with bash -l (or sh -l)
- sudo -i, or sudo -u username -i
- su

## interactive vs non-interactive

- If you can type commands and see output, it's interactive.
- Otherwise it's not.

For example, *cron* jobs are non-interactive shells.

- They don't read .bashrc or .profile.
- So they get a very bare-bones PATH variable.
- Use full paths for commands in non-interactive shells.
- Specify needed environment variable. Don't rely on the ones set in
  your .profile or .bashrc

## Bash startup files

Login shell - interactive:

- First: /etc/profile (which typically reads the files in
  /etc/profile.d)
- Next: look for ~/.bash_profile, ~/.bash_login, and ~/.profile, in that
  order, use the first one it finds.
- Upon exit: use ~/.bash_logout, if it exists.

Note: ~/.profile is the original Bourne shell config file. Still
commonly used. For example: a typical Ubuntu system does not have a
.bash_profile or .bash_login but use .profile

Non-login shell - interactive:

- use /etc/bash.bashrc and ~/.bashrc, if these files exist.

Non-interactive shells:

- inherit environment from parent process

In Ubuntu Desktop, the GUI login reads the settings in ~/.profile.
