# Arrays
Bash has 3 kinds of arrays

1. Positional Parameters - arguments passed into a script or function
2. Indexed Arrays - indexed by number, zero based
3. Associative Arrays - indexed by string,

## Positional Parameters

`$@` - all  
`$*` - all  
`$0` - name of script  
`$1` - 1st argument  
`${10}` - 10th argument  

Note: `$@` and `$*` work differently when used within double quotation marks.  
See "Within double quotes" below.

## Indexed arrays
Mostly used when you have a list of items, especially if items have spaces in them.  
Arrays can be "sparse", ie non-contiguous indexes, but you almost never want or need that.

POSIX shell does not have indexed arrays.  

### Create/Assign Values
    arr=()                              # create new empty array or clear existing array
    arr=(Fred Barney "Mr. Slate")       # create and assign
    declare -a arr                      # same.
    local -a arr                        # declare array, scope is limited to current code block.
    local -a arr=(foo bar baz)          # declare and assign
    readonly -a arr                     # an existing array can be marked read only,
    readonly -a arr=(foo bar baz)       # or declare and assign in one step.
    
    arr=( [0]=foo [1]=bar [2]=baz )     # another way to assign values
    arr[0]=foo                          # Or, like this...
    arr+=(bar baz)                      # add one or more elements to end of existing array.
    arr=("${arr[@]}" bar baz)           # same thing, gets rid of sparse elements
    arr=(bar baz "${arr[@]}")           # add to beginning of array
    
    read -a arr                         # read a line from standard input, split words based on IFS,
                                        # load words into array starting at index 0
    IFS=. read -a arr <<< "127.0.0.1"   # read IP address into array using "here string"
                                        # arr[0] contains 127  arr[3] contains 1
    arr=(/path/to/*.mp3)                # correct way to load array with filenames
                                        # filenames can contain spaces
    while read -r -d $'\0'; do          # another way to load array with filenames
        files+=("$REPLY")               # uses process substitution with find command using null delimiters
    done < <(find /foo -print0)         # and builtin REPLY variable set by read

### Get values of Array Elements

    # Example: arr=( Fred Barney "Mr. Slate" )
    declare -p arr      # prints the array definition with all indexes and values
    echo $arr           # Fred ; same as arr[0]
    echo ${arr[0]}      # Fred - arrays are zero-based
    echo ${arr[1]}      # Barney
    echo ${arr[@]}      # Fred Barney Mr. Slate
    echo ${arr[*]}      # Fred Barney Mr. Slate
    echo ${arr[@]:1:2}  # At index 1 (second array element), print value of 2 elements...Barney Mr.Slate
                        # This method has strange behavior in sparse arrays.
    # loop through array, do something with each element, sparse array ok
    for i in "${arr[@]}"; do echo "$i"; done
    for i in ${!arr[@]}; do echo "${arr[i]}"; done
    
    # loop through array, this method doesn't work with sparce array
    for ((i=0; i<${#arr[@]}; i++)); do echo "${arr[i]}"; done

Note: The subscripts `*` and `@` both extract the entire array, but
work differently when used within double quotation marks.

### Within double quotes

`@` returns all elements as **individual words**, separated by space character.  
This is what you usually want.

`*` returns all elements as a **single word**, separated by 1st character of IFS (space by default).  
Use this form to "serialize" an array into a scalar value.  
(change IFS to the desired separator character. ex. `IFS=$'\t'` )

### Examples using @, *, and double quotes.

We are trying to print each element on a new line. Only the last example
gives the results we want.

``` bash
arr=( Fred Barney "Mr. Slate" )

# example 1: wrong! splits 3rd element
for i in ${arr[*]}; do echo "$i"; done
Fred
Barney
Mr.
Slate

# example 2: wrong! splits 3rd element same as above
for i in ${arr[@]}; do echo "$i"; done

# example 3: wrong! combines all elements into one string
for i in "${arr[*]}"; do echo "$i"; done
Fred Barney Mr. Slate

# example 4: correct!
for i in "${arr[@]}"; do echo "$i"; done
Fred
Barney
Mr. Slate
```

### Serialize an array

This is one of the few times you use `"${array[*]}"`.
``` bash
array1=("Fred" "Barney" "Mr. Slate")
# serialize array1 into tab-delimited scalar, var1
IFS=$'\t' var1="${array1[*]}"   # Note: ${array1[@]} doesn't work, whether you quote it or not.
# optional
# export var1

# "De-serialize" the string back into an array.
IFS=$'\t' read -r -a array2 <<< "$myvar"
# use array2 as normal.
for name in "${array2[@]}"; do
    echo "$1 $name"
done
```

## Show information about arrays

    arr=( Fred Barney "Mr. Slate" )

    declare -a          # shows the values of all arrays
    declare -p arr      # shows the values of arr
    echo ${#arr[*]}     # 3  the number of elements
    echo ${#arr[@]}     # 3  same
    echo ${#arr[2]}     # 9  the length of the 3rd element
    echo ${!arr[*]}     # 0 1 2  the indexes of the array
    echo ${!arr[@]}     # 0 1 2  same

## Remove elements

    arr=(fred barney "Mr. Slate")

    unset arr           # removes entire array
    unset arr[1]        # removes 2nd element (index 1) from array. 
                        # does not re-index array, resulting in sparse array.

    # to make array non-sparse after unsetting an element, copy it to itself
    arr=("${arr[@]}")

    # Remove the 2nd element of a non-sparse array, resulting in a non-sparse array, in one step
    arr=(${arr[@]:0:1} ${arr[@]:2))})

    # Another way. set "pos" to the (zero-based) index you want to remove.
    pos=1  # remove 2nd element
    arr=(${arr[@]:0:$pos} ${arr[@]:$(($pos + 1))})


## Push Pop Shift Unshift

These code snippets are for non-sparse arrays, and will maintain the
array as non-sparse.

    # push - add new element to end of array
    arr=("${arr[@]}" "$new_element")
    arr+=("$new_element")

    # pop - remove last element from array
    arr=(${arr[@]:0:$((${#arr[@]}-1))})

    # shift - remove first element from array
    arr=(${arr[@]:1})

    # unshift - add new element to beginning of array
    arr=($new_element "${arr[@]}")


## Associative Arrays

- use strings as keys, rather than integers.
- you can't count on them coming out in the same order you put them in.

### Declare and Populate Associative Arrays

    # You must declare an associative array first.
    declare -A aa                                         # just declare, populate later
    declare -A aa=([a]=fred [b]=barney [c]="Mr. Slate")   # declare and populate in one step

    # The following assume the array has already been declared
    aa=([a]=fred               # Can span multiple lines
        [b]=barney             # with embedded comments
        [c]="Mr. Slate"
    )

    aa[a]=fred                 # Or, one element at a time
    aa[b]=barney
    aa[c]="Mr. Slate"

    aa+=([d]=Wilma [e]=Betty)  # add one or more elements to existing array.

### Getting the values of Array Elements

    declare -A aa=([a]=fred [b]=barney [c]="Mr. Slate")
    
    declare -p aa    # prints the array definition with all indexes and values
    echo ${aa[a]}    # Fred
    echo ${aa[b]}    # Barney
    echo ${aa[@]}    # Fred Barney Mr. Slate
    echo ${aa[*]}    # Fred Barney Mr. Slate
    
    # The next line does not work reliably with Associative Arrays:
    echo ${aa[@]:1:2}
    # But it does work if you are dealing with an individual element:
    echo ${aa[a]:1:2} # re  strips the F and d from Fred
    
    # loop through array, do something with each value.
    # there is no guarantee the elements will come out in the same order as they went in.
    for i in "${aa[@]}"; do echo "$i"; done
    
    # loop through array, do something with each index.
    for i in ${!aa[@]}; do echo "${asarr[$i]}"; done

Note: The subscripts `*` and `@` both extract the entire array, but
work differently when used within double quotation marks.

`@` returns all elements individually quoted `*` returns all elements as a
single string

These work the same way as indexed arrays

## Show information about Associative Arrays

    declare -A aa=([a]=fred [b]=barney [c]="Mr. Slate")

    declare -a          # shows the values of all arrays
    declare -p aa       # shows the values of aa
    echo ${#aa[*]}      # 3  the number of elements
    echo ${#aa[@]}      # 3  same
    echo ${#aa[c]}      # 9  the length of element indexed by "c"
    echo ${!aa[*]}      # a b c  the indexes of the array
    echo ${!aa[@]}      # a b c  same

## Remove elements

Unlike Indexed Arrays, Associative Arrays cannot be "sparse"

    declare -A aa=([a]=fred [b]=barney [c]="Mr. Slate")

    unset aa            # removes entire array
    unset aa[b]         # removes element with index b (barney) from array.
