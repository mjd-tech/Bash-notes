# Conditionals
if, case, select

## if elif else fi
``` bash
if [[ $name = "Fred" ]] then;
    echo "Hello Flintstone"
elif [[ $name = "Barney" ]]; then
    echo "Hello Rubble"
else
    echo "I don't know you"
fi

# test the return status of a command
if grep -q 'foo' somefile; then
    echo "found it"
fi

# simulate a boolean
flag=false
if $flag; then echo "hello"; fi     # this does not echo anything

flag=true
if $flag; then echo "hello"; fi     # this does
```
## case
``` bash
case $var in
    foo*)       echo 'foo food fool etc' ;;
    foo|bar)    echo 'foo or bar' ;;
    *baz)       echo 'ends in baz'
                ...additional commands... ;;
    *)          echo 'everything else.' ;;
esac
```
var is expanded without word splitting, brace, or pathname expansion,
so you can leave unquoted without problems:
leading parentheses can be used with PATTERN, but you rarely see this.

**Command terminators**
```
;;      break out of case
;&      execute the next block, without testing its pattern
;;&     goto the next block, testing its pattern
```
You rarely see anything other than `;;`

## Select
Note: select loops infinitely by default, don't forget `break`

``` bash
PS3=$'\nSelect a color: '
colors="red green blue"
select choice in $colors; do
    case "$REPLY" in
        1)  echo "You chose red"; break ;;
        2)  echo "You chose green"; break ;;
        3)  echo "You chose blue"; break ;;
        *)  echo "$choice is Invalid" ;;
    esac
done
```

``` bash
# Use an array to store menu items
items=( Fred Barney Wilma "Mr. Slate" )
PS3=$'\nSelect an option: '
while : ; do
    clear
    select choice in "${items[@]}" "Quit"; do
        case "$REPLY" in
            1)  echo "Yabba Dabba Doo"; break ;;
            2)  echo "Let's go bowling"; break ;;
            3)  echo "Mink coat"; break ;;
            4)  echo "Flintstone, you're fired."; break ;;
            $(( ${#items[@]}+1 )) ) echo "Exiting..."
                break 2 ;;
            *)  echo "Invalid choice" ;;
        esac
    done
    sleep 3
done
```
