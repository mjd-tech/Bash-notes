# is_set - Test variables for null or unset

The main thing to remember here is there is a difference between *not
set* and *set to null*. Thus:

    [ -z "$var" ] && echo "var is unset or null, I can't tell the difference."

To get around that problem, use parameter expansion:

    if [[ ${var:+isset} ]]; then
        echo "var is set and not null."
    else 
        echo "var is either not set or it is null."
    fi

    if [[ ${var+isset} ]]; then
        echo "var is set but might be null."
    else 
        echo "var is not set."
    fi

In PHP, the isset() function returns true if a variable is set and not
null. This can be simulated in Bash with:

    isset () {
      [[ ${!1:+isset} ]]
    }

    usage:

    isset foo && echo "foo is set"

This function uses an indirect reference, as indicated by the leading
bang !  
Indirect references are a Bash feature and not a POSIX feature. A more
portable way (works in Dash):

    isset () {
        eval [ ${1:+isset} ]
    }

The problem with these methods is that if \$1 is something like "Some
Text", you will get an error.

    isset () { 
      [[ ${!1:+isset} ]]
    }
    isset foo || echo "foo not set"
    foo not set  
    # OK so far...

    isset "Some Text" || echo "Some Text not set"
    bash: Some Text: bad substitution

Bash 4.2 provides a -v option to test if a variable is set.

    [[ -v foo ]] || echo "foo not set"
    foo not set

This works even with this:

    [[ -v "Some Text" ]] || echo "Some Text not set"
    Some Text not set

Here is an example function that shows how this can be useful:

    # Foreground (text) colors
    BLK=$(tput setaf 0) RED=$(tput setaf 1) GRN=$(tput setaf 2) YEL=$(tput setaf 3)
    BLU=$(tput setaf 4) MAG=$(tput setaf 5) CYN=$(tput setaf 6) WHT=$(tput setaf 7)

    # Background colors
    BLKB=$(tput setab 0) REDB=$(tput setab 1) GRNB=$(tput setab 2) YELB=$(tput setab 3)
    BLUB=$(tput setab 4) MAGB=$(tput setab 5) CYNB=$(tput setab 6) WHTB=$(tput setab 7)

    # Character attibutes Bold, Italic, Reverse, Underline, Reset
    BLD=$(tput bold)    ITA=$(tput sitm)    REV=$(tput rev) UNL=$(tput smul)
    NOR=$(tput sgr 0)   # reset all attributes, including colors

    Cecho () {
      # Echo text using tput attributes
      # Usage: Cecho [ATTRIBUTE ATTRIBUTE ...] "text"
      local attributes=
      while [[ -v ${1} ]]; do     # parameter 1 is a variable
          attributes+=${!1}       # build list of tput attributes
          shift
      done
      echo ${attributes}"${1}"${NOR}
    }

    Cecho BLD YEL BLUB "Bold Yellow Text on Blue Background"

Here we use the \[\[ -v \]\] test and shift the command line parameters.

This way you can have any number of Tput attributes on the command line,
with a simple test in the function. The test won't choke on "Bold Yellow
Text on Blue Background"

By using some indirect referencing, you avoid having to use \${BLD}
\${YEL}, etc. Just use the bare variable name.

Otherwise you would need to use getopts, or resort to some complicated
if then else tests.
