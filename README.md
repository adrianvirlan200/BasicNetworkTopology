# BasicNetworkTopology
 ![img](https://github.com/adrianvirlan200/BasicNetworkTopology/assets/74298808/4031cad4-73b5-4a5f-83bf-e1a4bea4a507)
 
 A project in cisco packet tracer that contains the basic elements of a network like: 19 hosts, 9 switches, 4 routers, vlans, routers on a stick, hdcp servers.

 # 1.The network under Router0
![image](https://github.com/adrianvirlan200/BasicNetworkTopology/assets/74298808/cdded73e-168f-4323-b469-1dd575885855)

- this network contains 3 VLANs
- the hosts from VLAN1 (purple) are assigned ip addresses from 192.168.254.0/24 network
- the hosts from VLAN2 (cyan) are assigned ip addressed from 192.168.255.0/24 network
- the Management host has an ip address from 192.168.255.0/24 network, and it is used to establish a remote connection to switch8 and switch5. This host belongs to vlan100 (yellow) that it was not added to router1(this router plays the role of a router on a stick). In this way, the Management host can communicate only with the mentioned switches; it is the only host in the entire project that is isolated from all other hosts. In this way, we increase the security of the network. In order for the Management host to communicate with both switches, every switch has been assigned an ip address form 192.168.255.0/24 domain.
- the links between switch8-switch5 and switch5-router0 are truck mode access. In this way, the vlans can be transferred from both switches to the router.
- router0 plays the role of a router on a stick, in order to ensure the inter-vlan routing. The interface connected to switch5 has 2 subinterfaces, every one has an ip address form the vlan1 and vlan2 domain. Those ip address have the role of default gateway for the their hosts.

  # 2. The network under Router1
  ![image](https://github.com/adrianvirlan200/BasicNetworkTopology/assets/74298808/5b27ab0f-644a-460f-bdf3-0a54d8a2ab4e)

  - this network contains one vlan and only one network - 192.168.24.0/24
  - the links of switch are access mode
  - the hosts have been assigned a static ip address from the mentioned domain, and their default gateway is router1
 
  # 3. The network under Router3
  ![image](https://github.com/adrianvirlan200/BasicNetworkTopology/assets/74298808/1f549435-5ed8-49d5-b4af-09d4d6eb6849)

- this network contains only one vlan, and all switches are set in access mode
- the main difference between the other networks and this one consist of the DHCP server. By using this server, the host can automatically receive ip addresses (from 192.168.200.0/24 domain), network masks(/24), DNS servers and the default gateway address from the server itself, instead of manually setting them.

#4. The network under Router2
![image](https://github.com/adrianvirlan200/BasicNetworkTopology/assets/74298808/f41a91f6-0334-4a9d-a1d5-968d8e9bb999)

- this network contains 3 vlans: 10, 20 and 30. Every vlan contains hosts that have been configurated IP addresses from 3 subnetworks
- the 3 mentioned subnetworks were created using equal subnet length method, starting from 192.168.0.0/24 domain. To create 2 subnets, it was needed 2 extra bits. Thus, the new mask of the subnets have 24+2 = 26 bits. Next, we assigned values to these 2 bits as following:
  R1: 192.168.0.00000000/26 =>192.168.0.0/26
  R2: 192.168.0.01000000/26 =>192.168.0.64/26
  R3: 192.168.0.10000000/26 =>192.168.0.128/26
- every subnetwork can have 2^6-2 = 62 hosts
- on the hosts belonging to vlan10, it has been assigned ip addresses from R1, on the hosts belonging to vlan20, it has been assigned ip addresses from R2 and on the hosts belonging to vlan30, it has been assigned ip addresses from R3
- the links between switch0 and router2 are in trunk mode, the other ones are in access mode
- in order to achieve inter-vlan routing, 3 subinterfaces were defined on the interface (fa0/0) that connects Switch0 to Router2. On each subinterface, an IP address from the network corresponding to the vlan was defined: an IP address from R1 was assigned on Fa0/0.10, an address from R2 was assigned on Fa0/0.20, and an address from R3 was placed on Fa0/0.30 . These IP addresses play the role of default gateway for the hosts. So, on the stations in vlan10/R1, the default gateway is set to the IP address of the Fa0/0.10 interface of Router2, etc. (analogous for vlan20/R2 => addr ip of Fa0/0.20 and vlan30/R3 => addr ip of Fa0/0.30).

# 5. The networks between Routers
![image](https://github.com/adrianvirlan200/BasicNetworkTopology/assets/74298808/eba3d024-a30c-409e-a1e0-33a262f6139f)

- a network was created between each 2 routers. The IP addresses assigned to each router interface belong to the networks visible in the image above.
- in order for routers to ensure packet routing, the routing table was updated as follows:
For Router0:

| Network  | Next Hop |
| ------------- | ------------- |
| 192.168.24.0/24  | Router1*  |
| 192.168.200.0/24 | Router3*  |
| 192.168.0.0/26 |Router2*| 
| 192.168.0.64/26 |Router2* | 
| 192.168.0.128/26 |Router2* | 
| 10.0.0.16/30 | Router2* | 
| 10.0.0.24/30 | Router1* |
| 10.0.0.40/30 | Router1* |
*It is actually the IP address of the interface with which the specified Router is connected to Router0.
There is no need to specify the next hop for networks 192.168.254.0/24, 192.168.255.0/24, 10.0.0.0/30, 10.0.0.8/30, 10.0.0.32/30, because these networks are directly connected to Router0.

The same procedure was followed for Router1, Router2 and Router3.
