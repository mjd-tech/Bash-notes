# Reserved Words

Reserved words have special meaning to the shell. They do not follow the
usual `command argument argument ...` format like external commands or
built-in commands. Most reserved words introduce shell flow control
constructs, such as "for" and "while".

| Command | Description |
| --- | --- |
| `!` | `! pipeline`  Negate the exit status of pipeline. |
| `[[`  `]]` | `[[ expression ]]`  Same as `test expression` or `[ expression ]`, except `[[ ]]` allows additional operators. |
| `(`  `)` | `( list )`  group a list of commands to be executed as a unit within a subshell. |
| `{`  `}` | `{ list; }`  group a list of commands to be executed as a unit, without launching a subshell. |
| `case` | `case value in  pattern1) cmds1;;  pattern2) cmds2;;  ...  esac`  execute commands if value matches pattern |
| `do` | `do`  precedes the command sequence in a for, while, until, or select statement. |
| `done` | `done`  ends a for, while, until, or select statement.|
| `elif` | `if condition1; then cmds; elif condition2; then cmds2; fi`  Shortcut for else if. |
| `else` | `if condition; then cmds; else cmds2; fi`  Execute cmds2 if condition not met. |
| `esac` | `case ... esac`  Ends a case statement. |
| `fi` | `if ... fi`  Ends an if statement. |
| `for` | `for var [in list]; do cmds; done `  Loop through list, each time setting var to value of list item. |
| `function` | `function name { cmds; }`  Defines //name// as a shell function. Same as `name () { cmds; }` |
| `if` | `if condition; then cmds; fi `  Execute cmds if condition is met. Condition usually involves the `test expr, [ expr ], or [[ expr ]]` statements. |
| `in` | `for var in list; do cmds; done `  Part of a "for" statement. |
| `select` | `select NAME in WORDS; do LIST; done `  User is prompted with a numbered list of the given words. |
| `then` | `if condition; then cmds; fi  `  Part of the "if" statement.|
| `time` | `time [-p] pipeline `  Report time consumed by pipeline's execution.|
| `until` | `until condition; do cmds; done `  Loop over commands until condition is true.|
| `while` | `while condition; do cmds; done `  Loop over commands while condition is true.|
