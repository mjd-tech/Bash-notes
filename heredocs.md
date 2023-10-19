# Here Docs and Here Strings
Make strings into standard input

## Examples
Note: "EOF" is any string that doesn't appear in the "here document" The
closing EOF must be alone on a newline. No leading or trailing
whitespace. EOF must not appear anywhere else in the here document

``` bash
# Create a new file with cat and "here document"
cat > /tmp/file.txt <<EOF
Some text...
EOF

# Another way to do it
cat <<EOF>/tmp/file.txt
Some text...
EOF

# Append to existing file
cat >> /tmp/file.txt <<EOF
this will be added to bottom of the file.
EOF

# Suppress leading tabs, but not spaces
cat >>/tmp/file.txt <<-EOF
        line 1 has leading tab. It will be stripped from output.
        line 2 has leading tab. It will be stripped from output.
  line 3 has two leading spaces. They will be preserved.
EOF
# Unfortunately there is no "suppress leading spaces" option.

# Use variables. Parameter substitution turned on
NAME="Fred Flintstone"
cat <<EOF
Hello, there, $NAME.
Goodbye
EOF
# $NAME gets replaced with Fred Flintstone in the output

# Don't use variables. Parameter substitution turned off
# This is useful for generating scripts.
NAME="Fred Flintstone"
cat <<'EOF'
Hello, there, $NAME.
Goodbye
EOF
# $NAME is displayed literally as `$NAME`

# Alternate syntax:
cat <<\EOF
cat <<"EOF"

# Set a variable from the output of a here document
variable=$(cat <<EOF
This variable
runs over multiple lines.
EOF)
echo "$variable"

# Supply input to a function
GetPersonalData () {
    read firstname
    read lastname
}
# This certainly looks like an interactive function, but...
# Supply input to the above function.
GetPersonalData <<EOF
Fred
Flintstone
EOF
echo "$firstname $lastname"
Fred Flintstone

# Multi-line comments in scripts
# Here, we use the colon : as a dummy command.
: <<'COMMENT'
Multi-line comments go here
without having to put # in beginning of each line
Bash interpreter will skip everything in here
even commands
COMMENT
```

## Here Strings
Bash extends the heredoc syntax with here strings

    COMMAND <<< $WORD

where `$WORD` is expanded and fed to the stdin of COMMAND.


``` bash
$ echo 'Wrap this silly sentence.' | fmt -t -w 20
Wrap this silly
   sentence.
$ fmt -t -w 20 <<< 'Wrap this silly sentence.'
Wrap this silly
   sentence.

# Or, in combination with read:
String="This is a string of words."

read -r -a Words <<< "$String"
#  The -a option to "read"
# assigns the resulting values to successive members of an array.

echo "First word in String is:    ${Words[0]}"   # This
echo "Second word in String is:   ${Words[1]}"   # is
echo "Third word in String is:    ${Words[2]}"   # a
echo "Fourth word in String is:   ${Words[3]}"   # string
echo "Fifth word in String is:    ${Words[4]}"   # of
echo "Sixth word in String is:    ${Words[5]}"   # words.
echo "Seventh word in String is:  ${Words[6]}"   # (null) Past end of $String.
```
