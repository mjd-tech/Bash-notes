# Obfuscate passwords in scripts

Problem:

- need to run `sudo somecommand` in a shell script.
- Don't want to enter password manually (especially if script runs
  "unattended" in a cron job)
- Don't want to put password in clear text in the script. (especially if
  you publish your scripts on Github or other public places)

Solution:

- encode the password with base64, and store it in a file.
- For example: `$HOME/.config/foo`
- the script decodes this file with `base64 -d`

```bash
cd ~/home/.config
echo "yourpassword" | base64 > foo
# if you change your password, you need to re-encode it with base64

# in your script:
base64 -d < $HOME/.config/foo | sudo -S true 2>dev/null

# this decodes the password and pipes it into sudo,
# which runs the command "true", which does nothing.
# It "activates" sudo for further use without a password.

sudo somecommand
sudo anothercommand
```

> **NEVER** do this on hosts directly connected to the internet. \
> The proper thing to do is add the command(s) to the `/etc/sudoers` file
