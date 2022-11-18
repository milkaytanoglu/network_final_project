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

Here is a general view of the my network.
![dark-theme](https://github.com/milkaytanoglu/network_final_project/blob/main/media/0001_general.PNG)

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

![dark-theme](https://github.com/milkaytanoglu/network_final_project/blob/main/media/0002.PNG)

```sh
int s0/0/0
clock rate 2000000
ip add 11.0.0.1 255.255.255.252
no sh
```
- [x] Internal connection (192.168.1.0)
- [x] External connection (11.0.0.0)
- [ ] External connection (13.0.0.0)

![dark-theme](https://github.com/milkaytanoglu/network_final_project/blob/main/media/0003.PNG)


```sh
int s0/0/1
clock rate 2000000
ip add 13.0.0.1 255.255.255.252
no sh
```

- [x] Internal connection (192.168.1.0)
- [x] External connection (11.0.0.0)
- [x] External connection (13.0.0.0)


![dark-theme](https://github.com/milkaytanoglu/network_final_project/blob/main/media/0004.PNG)

#### Router 1 
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

#### Router 2 

```sh
int fa0/0
no sh
ip add 192.168.3.1 255.255.255.0
no sh
```

- [x] Internal connection (192.168.3.0)
- [ ] External connection (13.0.0.0)

```sh
int s0/0/0
ip add 13.0.0.2 255.255.255.252
no sh
```

- [x] Internal connection (192.168.3.0)
- [x] External connection (13.0.0.0)

Let's not forget to do the ping tests.



### Phase 2 Steps
#### Router 0 
```sh
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
network 11.0.0.0 0.0.0.3 area 0
network 13.0.0.0 0.0.0.3 area 0
```

#### Router 1 
```sh
router ospf 1
network 192.168.2.0 0.0.0.255 area 0
network 11.0.0.0 0.0.0.3 area 0
```

![dark-theme](https://github.com/milkaytanoglu/network_final_project/blob/main/media/0005_top_side_router_1.PNG)

#### Router 2 

```sh
router ospf 1
network 192.168.3.0 0.0.0.255 area 0
network 13.0.0.0 0.0.0.3 area 0
```


![dark-theme](https://github.com/milkaytanoglu/network_final_project/blob/main/media/0006_top_side_router_2.PNG)

Let's not forget to do the ping tests.

![dark-theme](https://github.com/milkaytanoglu/network_final_project/blob/main/media/0007_OSPF_ping_1.PNG)

![dark-theme](https://github.com/milkaytanoglu/network_final_project/blob/main/media/0007_OSPF_ping_2.PNG)





### Phase 3 Steps
#### Router 0 
```sh
ip access-list extended WEBACL
permit udp any host 192.168.1.2 eq 53
permit tcp any host 192.168.1.3 eq 80
int fa0/0
ip access-group WEBACL out
```

![dark-theme](https://github.com/milkaytanoglu/network_final_project/blob/main/media/0008_open_DNS.PNG)

![dark-theme](https://github.com/milkaytanoglu/network_final_project/blob/main/media/0009_web_browser.PNG)

### Phase 4 Steps
#### Router 0 
```sh
ipv6 unicast-routing

int fa0/0
ipv6 add 1ef0:111:11:1::1/64
```

- [x] Internal connection (1ef0:111:11:1::0)
- [ ] External connection (1ef0:a bc:bc:c::0)
- [ ] External connection (1ef0:def:ef:f::0)

```sh
int s0/0/0
ipv6 add 1ef0:abc:bc:c::1/126
```

- [x] Internal connection (1ef0:111:11:1::0)
- [x] External connection (1ef0:a bc:bc:c::0)
- [ ] External connection (1ef0:def:ef:f::0)

```sh
int s0/0/1
ipv6 add 1ef0:def:ef:f::1/126
```

- [x] Internal connection (1ef0:111:11:1::0)
- [x] External connection (1ef0:a bc:bc:c::0)
- [x] External connection (1ef0:def:ef:f::0)


#### Router 1 
```sh
ipv6 unicast-routing

int fa0/0
ipv6 add 1ef0:222:22:2::1/64
```
- [x] Internal connection (1ef0:222:22:2::0)
- [ ] External connection (1ef0:a bc:bc:c::0)

```sh
int s0/0/0
ipv6 add 1ef0:abc:bc:c::2/126
```

- [x] Internal connection (1ef0:222:22:2::0)
- [x] External connection (1ef0:a bc:bc:c::0)


#### Router 2 

```sh
ipv6 unicast-routing

int fa0/0
ipv6 add 1ef0:333:33:3::1/64
```
- [x] Internal connection (1ef0:333:33:3::0)
- [ ] External connection (1ef0:def:ef:f::0)

```sh
int s0/0/0
ipv6 add 1ef0:def:ef:f::2/126
```

- [x] Internal connection (1ef0:333:33:3::0)
- [x] External connection (1ef0:def:ef:f::0)


Let's not forget to do the ping tests.

![dark-theme](https://github.com/milkaytanoglu/network_final_project/blob/main/media/0011_ipv6_ping_test.PNG)


### Phase 5 Steps
#### Router 0 
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

#### Router 1
```sh
ipv6 router ospf 1
router-id 2.2.2.2
int fa0/0
ipv6 ospf 1 area 0
int s0/0/0
ipv6 ospf 1 area 0
```

![dark-theme](https://github.com/milkaytanoglu/network_final_project/blob/main/media/0012_ipv6_ospf_n1.PNG)

#### Router 2 

```sh
ipv6 router ospf 1
router-id 3.3.3.3
int fa0/0
ipv6 ospf 1 area 0
int s0/0/0
ipv6 ospf 1 area 0
```

![dark-theme](https://github.com/milkaytanoglu/network_final_project/blob/main/media/0013_ipv6_ospf_n2.PNG)

Let's not forget to do the ping tests.


### Phase 6 Steps
#### Router 0 
```sh
ipv6 access-list WEBACL6
permit udp any host 1ed0:111:11:1::2 eq 53
permit tcp any host 1ed0:111:11:1::3 eq 80
int fa0/0
ipv6 traffic-filter WEBACL6 out
```

![dark-theme](https://github.com/milkaytanoglu/network_final_project/blob/main/media/0013_ipv6_dns_record.PNG)

![dark-theme](https://github.com/milkaytanoglu/network_final_project/blob/main/media/0014_ipv6_final.PNG)

Let's not forget to do the ping tests.
