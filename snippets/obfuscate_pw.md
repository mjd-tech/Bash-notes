# Obfuscate passwords in scripts

Problem:

- need to run sudo somecommand in a shell script.
- Don't want to enter password manually (especially if script runs
  "unattended" in a cron job)
- Don't want to put password in clear text in the script.

Solution:

- encode the password with base64, and store it in a file.
- For example: `$HOME/.config/foo`
- the script decodes this file with base64 -d


    cd ~/home/.config
    echo "yourpassword" | base64 > foo

    # in your script:
    base64 -d < $HOME/.config/foo | sudo -S true 2>dev/null
    sudo somecommand

    # this decodes the password and pipes it into sudo,
    # which runs the command "true", which does nothing.
    # It "activates" sudo for further use without a password.

Another method is good old ROT13 (or ROT47) ROT13 only handles ASCII
letters, so you could use ROT47 to handle some punctuation as well.

``` bash
ROT13=$(echo mypassword | tr 'A-Za-z' 'N-ZA-Mn-za-m')
ROT47=$(echo mypassword | tr '!-~' 'P-~!-O')
```