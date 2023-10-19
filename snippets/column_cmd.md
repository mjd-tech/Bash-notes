# column
format output in a table.

IMPORTANT: see comments about -n

Use column to "pretty print" text in a "table" format. This example
groups the output of several commands using the bash { } grouping operator,
then uses `column` to format the entire block of text as a whole.

```bash
{
echo "Group Name:Group ID:Members"
echo "----------:--------:-------"
awk '
    BEGIN { FS=":";OFS=":" }
    { print $1,$3,$4}
' /etc/group
} | column -t -n -s ':'
```

Note: older versions of column on DEBIAN based systems, use -n for:

     -n      By default, the column command will merge multiple adjacent delimiters into a single delimiter when using the -t
             option; this option disables that behavior. This option is a Debian GNU/Linux extension.

Newer versions of column enable Debian's -n behavior by default and re-define -n as:

      -n, --table-name name
           Specify the table name used for JSON output. The default is "table".



Also, newer versions of column can specify column names.

```bash
awk '
    BEGIN { FS=":";OFS=":" }
    { print $1,$3,$4}
' /etc/group | column -t -s ':' --table-columns Group\ Name,Group\ ID,Members
```

Note: column names that have a space ie. Group Name, need to have the space ecaped from the shell

## Format Markdown tables in vim
These can get messy when editing.

- get into Normal mode, select table with `vip` (visual, inner paragraph)
- run column command on selected text: `:!column -t -s '|' -o '|'`

example for neovim "mappings.lua"
      ["<leader>xt"] = { "vip :!column -t -s '|' -o '|'<CR>", desc = "Fix markdown tables" },
