This repository is a project overview of a lab I did while at Seneca College. It provides a comprehensive guide to configuring trunking via one Aruba 6300 and one Aruba 2530 Switch. The 6300 switch functions as a Layer 3 switch, whereas the 2500 switch operates as a Layer 2 switch. This project can be completed with two 6300s, although the configuration will need to be different. With this topology, we have two PCs. PC1 and PC2 must reside within their VLANs and receive an IP via DHCP pools. Successful implementation will allow the virtual machines to communicate via ICMP and enable secure SSH access using key pairs. Lastly, each member must install and configure an Apache webpage. This webpage must display your name and Seneca ID, and each member must be able to access the webpage via the IP received from the DHCP server.



Commands for complete functionality

Aruba 6300 Switch Configuration

 - 6300# conf t  
 - 6300(config)# vlan 25  
 - 6300(config-vlan-25)# no shut  
 - 6300(config-vlan-25)# exit  
 - 6300(config)# vlan 14  
 - 6300(config-vlan-14)# no shut  
 - 6300(config-vlan-14)# exit  
 - 6300(config)# int lag 1  
 - 6300(config-if-lag1)# no shut  
 - 6300(config-if-lag1)# no routing  
 - 6300(config-if-lag1)# vlan trunk native 1  
 - 6300(config-if-lag1)# vlan trunk allowed all  
 - 6300(config-if-lag1)# lacp mode active  
 - 6300(config-if-lag1)# exit  
 - 6300(config)# int 1/1/2  
 - 6300(config-if-1/1/2)# no shut  
 - 6300(config-if-1/1/2)# lag 1  
 - 6300(config-if-1/1/2)# exit  
 - 6300(config)# int 1/1/4  
 - 6300(config-if-1/1/4)# no shut  
 - 6300(config-if-1/1/4)# lag 1  
 - 6300(config-if-1/1/2)# exit  
 - 6300(config)# int 1/1/1  
 - 6300(config-if-1/1/1)# no shut  
 - 6300(config-if-1/1/1)# no routing  
 - 6300(config-if-1/1/1)# vlan access 25  
 - 6300(config-if-1/1/1)# exit  
 - 6300(config)# int vlan 25  
 - 6300(config-if-vlan25)# ip address 172.20.12.129 255.255.255.128  
 - 6300(config-if-vlan25)# no shut  
 - 6300(config-if-vlan25)# exit  
 - 6300(config)# int vlan 14  
 - 6300(config-if-vlan14)# ip address 172.20.7.1 255.255.255.128  
 - 6300(config-if-vlan14)# no shut  
 - 6300(config-if-vlan14)# exit  
 - 6300(config)# dhcp-server vrf default  
 - 6300(config-dhcp-server)# pool 25  
 - 6300(config-dhcp-pool-25)# range 172.20.12.130 172.20.12.254 prefix-len 25  
 - 6300(config-dhcp-pool-25)# default-router 172.20.12.129  
 - 6300(config-dhcp-pool-25)# exit  
 - 6300(config-dhcp-server)# pool 14  
 - 6300(config-dhcp-pool-14)# range 172.20.7.2 172.20.7.126 prefix-len 25  
 - 6300(config-dhcp-pool-14)# default-router 172.20.7.1  
 - 6300(config-dhcp-pool-14)# exit  
 - 6300(config-dhcp-server)# auto-confirm  
 - 6300(config-dhcp-server)# enable  
 - 6300(config-dhcp-server)# exit  
 - 6300(config)# spanning-tree  
 - 6300# exit  

Aruba 2530 Switch Configuration

The following configuration commands were executed on the Aruba 2530 switch to set up VLANs, link aggregation, and enable spanning tree for efficient network traffic management.

 - HP-2530-24-PoEP# conf t  
 - HP-2530-24-PoEP(config)# trunk 2-4 trk1 lacp  
 - HP-2530-24-PoEP(config)# vlan 25  
 - HP-2530-24-PoEP(config-vlan-25)# tagged trk1  
 - HP-2530-24-PoEP(config-vlan-25)# no ip address  
 - HP-2530-24-PoEP(config-vlan-25)# exit  
 - HP-2530-24-PoEP(config)# vlan 14  
 - HP-2530-24-PoEP(config-vlan-14)# untagged 1  
 - HP-2530-24-PoEP(config-vlan-14)# tagged trk1  
 - HP-2530-24-PoEP(config-vlan-14)# no ip address  
 - HP-2530-24-PoEP(config-vlan-14)# exit  
 - HP-2530-24-PoEP(config)# spanning-tree  
 - HP-2530-24-PoEP(config)# exit 
