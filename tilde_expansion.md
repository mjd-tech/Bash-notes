# Tilde expansion
Tilde expansion is used to expand to several specific pathnames:

- home directories
- current working directory
- previous working directory

```bash
~               # your home directory, same as $HOME
~username       # username's home directory
~+              # current working directory, same as $PWD
~-              # previous working directory, same as $OLDPWD
```

Tilde expansion is performed when the tilde construct is at the
beginning of a word, or a separate word. 

If there's nothing to expand, i.e. in case of a wrong username or any
other error condition, the tilde construct is not replaced, it stays what it is.

Tilde expansion is also performed when a variable is assigned.

```bash
DOCS=~/Documents
PATH=$PATH:~/bin    # Tilde can be used after a colon
```

**Spaces in the referenced path?** A construct like...

    ~/"my directory"

...is perfectly valid and works!

## Caveats
Be careful when using ~ to specify the home directory on **remote** hosts, i.e. ssh connections.
When in doubt, use the full path.
