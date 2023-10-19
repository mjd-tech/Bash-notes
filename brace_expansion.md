# Brace Expansion
generate mass-arguments for a command.

## Examples
``` bash
# Create multiple directories.
mkdir -p $HOME/my_new_project/{src,docs,build}

# Change file extension .txt to .md
mv myfile.{txt,md}

# Backup a file
cp /path/to/file{,.bak}   # expands to: cp /path/to/file /path/to/file.bak

# Restore
cp /path/to/file{.bak,}   # expands to: cp /path/to/file.bak /path/to/file
echo {Fred,Barney,"Mr. Slate}
Fred Barney Mr. Slate

# Install similarly named packages
sudo apt install php-{common,mbstring,mysql}

# Ranges
echo {50..60}
50 51 52 53 54 55 56 57 58 59 60

echo {a..e}
a b c d e

echo {01..05}
01 02 03 04 05

echo {A..C}{1..3}
A1 A2 A3 B1 B2 B3 C1 C2 C3

echo {3..-3}
3 2 1 0 -1 -2 -3

echo {10..50..5}
10 15 20 25 30 35 40 45 50

echo {a..z..2}
a c e g i k m o q s u w y

echo b{all,ell,oat,eef}
ball bell boat beef

echo A{1..5}
A1 A2 A3 A4 A5

echo 2.{0..9}
2.0 2.1 2.2 2.3 2.4 2.5 2.6 2.7 2.8 2.9

# generate the alphabet, using nested brace expansion
echo {{A..Z},{a..z}}
A B C etc. X Y X a b c etc. x y z
```
## Common Mistakes
Don't put spaces after the opening brace, or before closing brace.  
Otherwise bash will interpret as command grouping.

A common error is trying to use a variable in a **range**:

``` bash
start=1 end=9
echo {$start..$end}  # range expects a literal number or alpha character
{1..9}               # prints literal braces, but does expand variables

eval echo {$start..$end}    # workaround using eval
1 2 3 4 5 6 7 8 9
```

But you **can** use variables in a **list**

``` bash 
start=1 end=9
echo foo{${start},${end}}bar
foo1bar foo9bar
```

Brace expansion **cannot** be used in a variable assignment:

``` bash
foo={1..10}
echo $foo
{1..10}

# Workaround using printf
printf -v foo '%s ' {1..10}
echo $foo
1 2 3 4 5 6 7 8 9 10
```
