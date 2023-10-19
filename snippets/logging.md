# Quick log for bash scripts, with line limit

```bash
script_dir="$(dirname "$(readlink -f "${BASH_SOURCE[0]}")")"
log_file="$script_dir/backup.log"

# Log function
LogIt () { echo "$@" | tee -a "$log_file"; }

# Truncate existing log file to 200 lines.
tmp=$(tail -n 200 "$log_file" 2>/dev/null) && echo "$tmp" > "$log_file"

# Log some text to both the log file and terminal
LogIt "this is logged"

# run a command, log stdout and stderr to both log file and terminal
somecmd some_args |& tee -a "$log_file"
```

## from the internet
forgot the source.

Logging can be a complex beast to tame, for logs require rotation and
deletion, otherwise the beast will eat up all your disk space. Setting
up *logrotate* for a single shell script is a pain.

This is my quick recipe for a basic bash logging solution with
line-based truncation: the logfile will never exceed RETAIN_NUM_LINES
lines (actually, it will never exceed such size when the script is
launched, so the number of lines when the script has run is
RETAIN_NUM_LINES + number of lines logged by the script).

Whenever you use the log function you'll get output on stdout as well,
complete with a timestamp.

    #!/bin/bash

    # set LOGFILE to the full path of your desired logfile; make sure
    # you have write permissions there. set RETAIN_NUM_LINES to the
    # maximum number of lines that should be retained at the beginning
    # of your program execution.
    # execute 'logsetup' once at the beginning of your script, then
    # use 'log' how many times you like.

    LOGFILE=quicklog.sh.log
    RETAIN_NUM_LINES=10

    function logsetup {
        TMP=$(tail -n $RETAIN_NUM_LINES $LOGFILE 2>/dev/null) && echo "${TMP}" > $LOGFILE
        exec > >(tee -a $LOGFILE)
        exec 2>&1
    }

    function log {
        echo "[$(date --rfc-3339=seconds)]: $*"
    }

    logsetup

    log hello this is a log

