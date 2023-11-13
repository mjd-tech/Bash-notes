# Metacharacters
characters with "special meaning"
The Bash manual defines metacharacters as: a character
that, when unquoted, separates words. A metacharacter is a blank (space, tab)
or one of the following characters: `| & ; ( ) < >`

## Metacharacters per the Bash Manual

| Metacharacter  | Description                                                                                          |
| -------------- | ---------------------------------------------------------------------------------------------------- |
| `[whitespace]` | Whitespace (spaces, tabs and newlines). Bash uses whitespace to determine where words begin and end. |
| `\|`           | Pipe the output of one command to the input of another command.                                      |
| `&`            | 1. Run command in the background, and continue to the next. 2. used in redirection.                  |
| `;`            | Command separator. Used for multiple commands on one line.                                           |
| `( command )`  | Subshell Execution.                                                                                  |
| `> or <`       | Redirection characters. redirect the input and/or output of a command.                               |

## Other characters with special meaning

| Character                 | Description                                                                                                                  |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `=`                       | 1. Variable Assignment. 2. "is equal to"                                                                                     |
| `$`                       | Substitution (variables, command substitution).                                                                              |
| `$variable`               | Parameter expansion, short form.                                                                                             |
| `${variable}`             | Parameter expansion, long form. No spaces around braces, else Bash thinks you want command grouping.                         |
| `'text'`                  | Single quotes protect the text inside from any kind of expansion, or word-splitting.                                         |
| `"text"`                  | Double quotes protect the text inside from word-splitting, but they permit substitutions to occur.                           |
| `\`                       | Escape character. prevents the next character from having special meaning. Does not work within single quotes.               |
| `\`                       | Line continuation, if last character on line, escapes newline.                                                               |
| `#`                       | Comment character. Any word beginning with # begins a comment that extends to the next newline.                              |
| `&&`                      | AND. Run the command to the right ONLY IF the command on the left succeeded.                                                 |
| `\|\|`                    | OR. Run the command to the right ONLY IF the command on the left failed.                                                     |
| `;; ;& ;;&`               | terminates a list of commands in a case statement.                                                                           |
| `~`                       | A shortcut for home directory.                                                                                               |
| `>>`                      | Redirection: Append output to a file. If the file does not exist, it will be created.                                        |
| `<<`                      | Redirection. "Here Doc"                                                                                                      |
| `<<<`                     | Redirection. "Here String"                                                                                                   |
| `[abc]`                   | Match nne of the characters within the square brackets.                                                                      |
| `[ expr ]`                | Same as the "test" command. Brackets must be surrounded by whitespace, operators such as < and > escaped.                    |
| `[[ expression ]]`        | Test keyword. This evaluates the conditional expression as a logical statement, to determine whether it's "true" or "false". |
| `{ commands; }`           | Command grouping.                                                                                                            |
| ``command``, `$(command)` | Command substitution (The latter form is highly preferred.)                                                                  |
| `((expression))`          | Arithmetic Command. Equivalent to the "let" builtin command.                                                                 |
| `$((expression))`         | Arithmetic Substitution.                                                                                                     |

