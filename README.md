# network_final_project
network_final_project is a project for greduation from TechCareer thanks to Saim Ozkinar.
I would like to thank him for his hard work and teaching.

## Basic
```sh
This is a networking project where one backbone runs the IPv4 and IPv6 binary stacks, dynamic routing happens with OSPF,
both IPv4 and IPv6 backbones are serviced with ACLs from service ports only, and all other ports are closed.
```

## Phases
- [1] Giving IPv4 to routers, servers, laptop, desktop and FastEthernet and Serial ports of the routers.
- [2] Application of dynamic routing to networks with IPv4 by OSPF.
- [3] IPv4 backbone will only serve from service ports with ACL. All other ports will remain closed.
- [4] Giving IPv6 to routers, servers, laptop, desktop and FastEthernet and Serial ports of the routers.
- [5] Application of dynamic routing to networks with IPv6 by OSPF.
- [6] Both IPv4 and IPv6 backbones will only serve from service ports with ACLs. All other ports will remain closed.


### Phase 1 Steps
#### Router 0 
```sh
int fa0/0
no sh
ip add 192.168.1.1 255.255.255.0
no sh
```
- [x] Internal connection (192.168.1.0)
- [ ] External connection (11.0.0.0)
- [ ] External connection (13.0.0.0)
```sh
int s0/0/0
clock rate 2000000
ip add 11.0.0.1 255.255.255.252
no sh
```
- [x] Internal connection (192.168.1.0)
- [x] External connection (11.0.0.0)
- [ ] External connection (13.0.0.0)
```sh
int s0/0/1
clock rate 2000000
ip add 13.0.0.1 255.255.255.252
no sh
```
- [x] Internal connection (192.168.1.0)
- [x] External connection (11.0.0.0)
- [x] External connection (13.0.0.0)

#### Router 1 xxx
```sh
int fa0/0
no sh
ip add 192.168.2.1 255.255.255.0
no sh
```

- [x] Internal connection (192.168.2.0)
- [ ] External connection (11.0.0.0)

```sh
int s0/0/0
ip add 11.0.0.2 255.255.255.252
no sh
```
- [x] Internal connection (192.168.2.0)
- [x] External connection (11.0.0.0)

#### Router 2 xxx

```sh
int fa0/0
no sh
ip add 192.168.3.1 255.255.255.0
no sh
```
Let's not forget to do the ping tests.

### Phase 2 Steps
#### [Router 0] xxx
```sh
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
network 11.0.0.0 0.0.0.3 area 0
network 13.0.0.0 0.0.0.3 area 0
```

#### Router 1 xxx
```sh
router ospf 1
network 192.168.2.0 0.0.0.255 area 0
network 11.0.0.0 0.0.0.3 area 0
```

#### Router 2 xxx

```sh
router ospf 1
network 192.168.3.0 0.0.0.255 area 0
network 13.0.0.0 0.0.0.3 area 0
```
Let's not forget to do the ping tests.


### Phase 3 Steps
#### Router 0 xxx
```sh
ip access-list extended WEBACL
permit udp any host 192.168.1.2 eq 53
permit tcp any host 192.168.1.3 eq 80
int fa0/0
ip access-group WEBACL out
```
Let's not forget to do the ping tests.


### Phase 4 Steps
#### Router 0 xxx
```sh
ipv6 unicast-routing

int fa0/0
ipv6 add 1ef0:111:11:1::1/64
int s0/0/0
ipv6 add 1ef0:abc:bc:c::1/126
int s0/0/1
ipv6 add 1ef0:def:ef:f::1/126
```

#### Router 1 xxx
```sh
ipv6 unicast-routing

int fa0/0
ipv6 add 1ef0:222:22:2::1/64
int s0/0/0
1ef0:abc:bc:c::2/126
```

#### Router 2 xxx

```sh
ipv6 unicast-routing

16
int fa0/0
1ef0:333:33:3::1/64
int s0/0/0
1ef0:def:ef:f::2/126
```
Let's not forget to do the ping tests.


### Phase 5 Steps
#### Router 0 xxx
```sh
ipv6 router ospf 1
router-id 1.1.1.1
int fa0/0
ipv6 ospf 1 area 0
int s0/0/0
ipv6 ospf 1 area 0
int s0/0/1
ipv6 ospf 1 area 0
```

#### Router 1 xxx
```sh
ipv6 router ospf 1
router-id 2.2.2.2
int fa0/0
ipv6 ospf 1 area 0
int s0/0/0
ipv6 ospf 1 area 0
```

#### Router 2 xxx

```sh
ipv6 router ospf 1
router-id 3.3.3.3
int fa0/0
ipv6 ospf 1 area 0
int s0/0/0
ipv6 ospf 1 area 0
```
Let's not forget to do the ping tests.


### Phase 6 Steps
#### Router 0 xxx
```sh
ipv6 access-list WEBACL6
permit udp any host 1ed0:111:11:1::2 eq 53
permit tcp any host 1ed0:111:11:1::3 eq 80
int fa0/0
ipv6 traffic-filter WEBACL6 out
```

#### Router 1 xxx
```sh

```

#### Router 2 xxx

```sh

```
Let's not forget to do the ping tests.
