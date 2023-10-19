# IP Address - how to get it

There are two basic ways of getting the necessary information in Linux
systems

1.  ip addr show method
2.  ifconfig method

## The ip addr show method

We will parse this output:

    $ ip addr show eth0
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
        link/ether 00:22:15:f6:55:e6 brd ff:ff:ff:ff:ff:ff
        inet 192.168.16.30/24 brd 192.168.16.255 scope global eth0
        inet6 fe80::222:15ff:fef6:55e6/64 scope link 
           valid_lft forever preferred_lft forever

NOTE: Try this...

    ip -4 -brief addr show eth0 

### IP Address

A common approach using grep, awk and cut:

    $ ip addr show eth0 | grep -w inet | awk '{ print $2}'| cut -d "/" -f 1
    192.168.16.30

We can use awk to do everything:

    $ ip addr show eth0 | awk '/inet / {sub(/\/.*$/,"",$2); print $2}'
    # Even better:
    $ ip -4 -brief addr show eth0 | awk '{sub(/\/.*$/,"",$3); print $3}'

### Netmask in CIDR Notation

Common:

    $ ip addr show eth0 | grep -w inet | awk '{ print $2}'| cut -d "/" -f 2
    24

Awk only:

    $ ip addr show eth0 | awk '/inet / {sub(/^.*\//,"",$2); print $2}'

### IP Address and Netmask in Dotted Decimal Notation

The ip addr command returns the ip address in CIDR notation, ie
192.168.1.100/24 rather than a separate ip address and subnet mask. Here
is a function that converts the "slash" number to a dotted decimal
subnet mask.

    Cidr2mask() {
      # Usage: Cidr2mask n
      # Where n is the 'slash' part of a cidr address
      # ex: 192.168.1.100/24  n=24  Output=255.255.255.0
      local mask=""
      local -i i=0  full_octets=$(( $1/8 )) partial_octet=$(( $1%8 ))

      for (( i=0; i<4; i+=1 )); do
          if (( i < full_octets )); then
              mask+=255
          elif (( i == full_octets )); then
              mask+=$(( 256 - 2**(8 - partial_octet) ))
          else
              mask+=0
          fi
          (( i < 3 )) && mask+=.
      done
      echo $mask
    }  

An example of how to use this function:

    # Get ip address in "slash notation" (CIDR). Something like 192.168.1.100/24
    ip_cidr=$(ip -br addr show eth0 | awk '{print $3}')

    # Strip the slash and everything to the right.
    ip_address=${ip_cidr%/*}

    # Strip the slash and everything to the left. Call Cidr2mask with the "slash number". 
    subnet_mask=$(Cidr2mask ${ip_cidr#*/})

    # Display results
    echo "IP address:  $ip_address"
    echo "Subnet mask: $subnet_mask"

### Broadcast IP

Common:

    $ ip addr show eth0 | grep -w inet | awk '{ print $4}'
    192.168.16.255

Awk only:

    $ ip addr show eth0 | awk '/inet / { print $4}'

### MAC Address

Common:

    $ ip addr show eth0 | grep -w ether | awk '{ print $2 }'
    00:22:15:f6:55:e6

Awk only:

    $ ip addr show eth0 | awk '/ether / { print $2 }'

## The ifconfig method

This time we’ll parse this output:

    $ ifconfig eth0
    eth0      Link encap:Ethernet  HWaddr 00:22:15:f6:55:e6  
              inet addr:192.168.16.30  Bcast:192.168.16.255  Mask:255.255.255.0
              inet6 addr: fe80::222:15ff:fef6:55e6/64 Scope:Link
              UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
              RX packets:626934 errors:0 dropped:0 overruns:0 frame:0
              TX packets:363506 errors:0 dropped:0 overruns:0 carrier:2
              collisions:0 txqueuelen:1000 
              RX bytes:284072710 (284.0 MB)  TX bytes:56413838 (56.4 MB)
              Interrupt:45 

### IP ADDRESS

Common:

    $ ifconfig eth0 | grep -w inet | awk '{print $2}' | cut -d ":" -f 2
    192.168.16.30

Another slightly different form:

    $ ifconfig eth0 | grep "inet addr" | cut -d: -f2 | awk '{print $1}'

And, of course we can do all parsing with awk:

    $ ifconfig eth0 | awk -F: '/inet / { sub(/ .*$/,"",$2); print $2 }'

### Broadcast IP

Common:

    $ ifconfig eth0 | grep -w inet | awk '{print $3}' | cut -d ":" -f 2
    192.168.16.255

Awk only:

    $ ifconfig eth0 | awk -F: '/inet / { sub(/ .*$/,"",$3); print $3 }'

### Netmask

Common:

    $ ifconfig eth0 | grep -w inet | awk '{print $4}' | cut -d ":" -f 2
    255.255.255.0

Awk only:

    $ ifconfig eth0 | awk -F: '/inet / { print $4 }'

### MAC Address

Common:

    $ ifconfig eth0 | grep HWaddr | awk '{ print $5 }'
    00:22:15:f6:55:e6

Awk only: (here it *really* shows how unnecessary it is to use grep)

    $ ifconfig eth0 | awk '/HWaddr/ { print $5 }'

## Calculating the subnet

Given the IP address and Subnet Mask, get the subnet.

    #!/bin/bash
    ip=192.168.10.115
    mask=255.255.255.248
    # Take the leftmost field of ip and mask, do bitwise AND
    subnet=$(( ${ip%%.*} & ${mask%%.*} ))       
    for i in . . . ; do    # loop 3 times
        ip=${ip#*.}        # strip off the leftmost field
        mask=${mask#*.}    # ditto
        # concatenate subnet with a dot and the bitwise AND
        subnet=${subnet}${i}$(( ${ip%%.*} & ${mask%%.*} ))
    done
    echo "Subnet = $subnet"

Result

    Subnet = 192.168.10.112
