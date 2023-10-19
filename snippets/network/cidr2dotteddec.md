# CIDR and Subnet mask

CIDR address consists of IP address, a slash, and the number of bits in
the subnet mask

    192.168.1.22/24   (24 bits in subnet mask, 255.255.255.0)

## Convert CIDR to Dotted Decimal

Argument: the number of bits (the number after the slash)  
Result: the subnet mask in dotted decimal format. ex. 255.255.255.0

``` bash
cidr2mask() {
    local mask=""
    local -i i=0
    local full_octets=$(( $1/8 ))
    local partial_octet=$(( $1%8 ))

    for (( i=0; i<4; i+=1 )); do
        if (( i < $full_octets )); then
            mask+=255
        elif (( $i == $full_octets )); then
            mask+=$(( 256 - 2**(8 - $partial_octet) ))
        else
            mask+=0
        fi  
        (( i < 3 )) && mask+=.
    done

    echo $mask
}
```

## Convert Dotted Decimal to CIDR

Here we set a local IFS within the function.

- It does act as the Internal Field Separator.
- It does not affect the global IFS.

``` bash
mask2cidr() {
    local -i nbits dec
    local IFS
    nbits=0
    IFS=.
    for dec in $1 ; do
        case $dec in
            255) (( nbits+=8 )) ;;
            254) (( nbits+=7 )) ;;
            252) (( nbits+=6 )) ;;
            248) (( nbits+=5 )) ;;
            240) (( nbits+=4 )) ;;
            224) (( nbits+=3 )) ;;
            192) (( nbits+=2 )) ;;
            128) (( nbits+=1 )) ;;
            0)   ;;
            *)   echo "Error: $dec is not recognized" >&2; exit 1 ;;
        esac
    done
    echo "$nbits"
}

```
