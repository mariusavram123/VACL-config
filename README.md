# Configure VACL

This lab has been build using GNS3:

The goal of this lab is to configure an VACL(VLAN access control list) to block traffic between R1 and R2 in VLAN 10

* Configure IP addresses on routers and place the ports on the switch as access ports in vlan 10.
* Configure a VACL on the switch to block traffic between the routers in VLAN 10

[x] Create an ACL to block ICMP traffic:
```
ip access-list extended NOICMP
permit icmp any any
```
[x] Create a VLAN access map
* ip address is going to be refered to the access list that we have defined
* by default all trafic will be dropped so we need to create another sequence to forward the rest of the traffic that is dont defined by the access-list defined
```
vlan access-map BLOCKICMP
match ip address NOICMP
action drop

vlan access-map BLOCKICMP 20
action forward
```
[x] Apply the VLAN access map to the correct VLAN to filter the traffic
```
vlan filter BLOCKICMP vlan-list 10
```
[x]Test the connectivity:
* Ping from R1 to R2- it should fail
* Enable the HTTP server on R2
```
ip http server
```
* You should be able to connect from R1 to R2 using telnet on the HTTP port(80).

## Documentation
https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst6500/ios/12-2SX/configuration/guide/book/vacl.html