# DMVPN config using OSPF and BGP

Example Hub and Spoke DMVPN between Cisco Routers using OSPF and BGP

## Project Scope

The scope of this project is to create a Central Office (CO) with 4 Remote Offices (RO).  The ROs should be able to reach the CO LAN through redundant paths.  The DMVPN should be on a separate OSPF area and receive a summary route of the CO LAN. The CO and all ROs should use BGP for public routes.

Communication between LANs should be routed through an encrypted tunnel.

## Base Config

Using one HUB and one Spoke using Tunnel0 will yield a working DMVN config, the second HUB is strictly for redundancy.

### No Lab?  No problem!

You can fully configure the Hubs, Spokes, and ISPs using a simulator like GNS3 or VIRL.  Installing and configuring those tools is out of the scope of this project.

### Network Diagram

The physical network diagram has 2 Internet Routers at the CO, each using their own ISP for vendor redundancy.  ROs each have one Internet Router with connections to 2 ISPs for redundancy.

![alt networkDiagram](https://github.com/lileddie/dmvpn/blob/tschmid-complete-DC/images/DMVN-diagram.jpeg)

## Routing Tables

The OC routers should have a route to each remote office and the LAN at each site.



The RO routers should have a route back to the OC, and to each RO.



### Spoke to Spoke Connectivity

Utilizing BGP and DMVN's Next Hop feature, spokes should be able to communicate directly to one another.

```
RO1#! ### Verify Ping connectivity to RO2
RO1#ping 172.16.0.3                      

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.0.3, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 28/39/56 ms
RO1#! ### Verify Traceroute doesn't traverse Hub
RO1#traceroute 172.16.0.3

Type escape sequence to abort.
Tracing the route to 172.16.0.3

  1 172.16.0.3 44 msec 56 msec 16 msec
RO1#
```

### Verify Traffic is Encrypted

Utilizing the same test as above, capture the traffic to ensure all traffic on link is encrypted.

![alt packetCapture](https://github.com/lileddie/dmvpn/blob/tschmid-complete-DC/images/wireshark.png)

## Authors

* **Troy Schmid**

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details
