# pfSense SecurityResource

pfSense is an open source firewall/router computer software distribution based on FreeBSD.

It can be installed on either physical or virtual machines, and it offers an high-level configuration interface, 
by means of a web dashboard, as well as a low-level interface by means of SSH.
Furthermore, pfSense supports the installation of third-party packages, that can be included. Exploiting this mechanism, the resource made 
available for SoftFIRE experimenters provides a ReST interface through which it is possible to configure and modify every property of the pfSense. 
The packet used to provide the ReST interface is the FauxAPI extension (please refer to the official [repository](https://github.com/ndejong/pfsense_fauxapi/) 
for more details about). 

Moreover, the credentials of the Experimenter are pushed in the Virtual Machine, so that user only is able to access each of the available
interfaces. 

## Resource properties
* **testbed**: Defines where to deploy the Security Resource selected
* **wan_name**: Selects the network on which the first interface of the VM is attached. It is configured as WAN on pfSense. It must be a network connected to the SoftFIRE-public network 
* **lan_name**: Selects the network on which the second interface of the VM is attached. It is configured as LAN on pfSense

## List of features
This is a list of the main features offered by pfSense OS: 

* Firewall with stateful packet inspection
* IPv4 and IPv6 support
* Traffic Shaping
* NAT
* Load Balancing
* VPN - IPsec, OpenVPN, L2TP
* Dynamic DNS
* DHCP Server and Relay 

More detailed information about pfSense can be found on the official [website](https://www.pfsense.org/) 
and [documentation](https://doc.pfsense.org).

## VM specification

The pfSense VM is by default provided by the Security Manager with the following characteristics:

| | |
|:-----------|:-------:|
| vCPUs       | 1       |
| RAM         | 512 MB  |
| Disk        | 1 GB    |
| Interfaces  | 2       |

The two network interfaces are attached to *wan_name* and *lan_name* networks, and they are set as *WAN* and *LAN* interface respectively. 

[Legal disclaimer](https://www.pfsense.org/about-pfsense/)
