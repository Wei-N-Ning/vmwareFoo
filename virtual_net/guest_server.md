

#### create virtual network

(I can also use the "bridge" type, meaning that the vm will connect
directly to the host's physical network. See the note at the bottom)

vmware ui -> edit -> virtual network editor

add a new `NAT` type virtual network entry, e.g. vmnet8

nat settings:

gateway ip: 192.168.220.2

allow active FTP

allow any unique identifier

UDP timeout (seconds): 60

config port: 33445

turn on: use local DHCP service; connect a host virtual adapter 

subnet ip: 192.168.220.0

subnet mask: 255.255.255.0

#### config VM 

(see the notes below about "bridge" type)

vm -> settings

hardware / network adapter

connect at power on

custom: specific virtual network

choose the vm entry just created

leave advanced alone 

NOTE: the guest vm will still have access to the internet after this

#### testing:

on guest vm: 

```
ifconfig
# make note of the vm's ip address, in this case 192.168.220.128
cd ~
python2 -m SimpleHTTPServer

or 

# source: https://stackoverflow.com/questions/7943751/what-is-the-python-3-equivalent-of-python-m-simplehttpserver
python3 -m http.server

# by default the port number is 8000
```

on host

```
# make sure the virtual network is accessible
ping -c 1 192.168.220.128
echo $?
# access to the server
curl http://192.168.220.128:8000
```

#### side note:

in my physical network if other devices (e.g. macbook) are connected 
to the same public router I can use them to host server too:

on macbook
```
ifconfig
# make note of its ip address, in this case 192.168.1.86
python2 -m SimpleHTTPServer
```

other device does not have access to the vm on host (gunship) 

I can therefore setup the server-client hierarchy like this:

```
gunship (physical server)
 - vm U18 (virtual client)
 - macbook (physical client)
```

#### attach vm directly to host's physical network

it can be useful for doing quick testing but it exposes the guest 
vm to the outside hostile environment

in L17 vm's setting page, choose bridge (the default choice) instead of NAT

test accessiblity 
```
ifconfig
# make note of its ip address (192.168.1.134) 
# now that this vm connects to the host's network, its address should 
# be configured following the host's subnet ip

python2 -m SimpleHTTPServer

# port number is by default 8000
```

on host and other devices:
```
ping -c 1 192.168.1.134
curl http://192.168.1.134:8000
```

lastly if the `NAT` vm client on gunship visits the `bridge` vm server,
the server thinks the visitor is gunship (because of the encapsulation)

updating the server-client hierarchy:
```
gunship
 - vm U18 (NAT)
 - macbook
 - vm L17 (bridge)

vm L17 (bridge)
 - gunship
 - macbook
 - vm U18 (NAT) -- the visitor is considered gunship

vm U18 (NAT)
 - gunship
```

