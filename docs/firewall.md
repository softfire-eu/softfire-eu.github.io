# Firewall SecurityResource
A firewall is a network security system that monitors and controls the incoming and outgoing 
network traffic based on predetermined rules. 
The available firewall resource is built upon Ubuntu UFW (Uncomplicated FireWall), to which a 
control system, based on a ReST interface, has been added.  
The firewall agent is available for Ubuntu OS only.  
The rules that can be defined on this type of firewall are stateless (they do not maintain 
information about the context). It works as a packet filter, which looks at network addresses, 
ports and protocols.  

Services specifically available for the firewall Resource are:  
1. The Experimenter can statically define a list of allowed IP addresses (or CIDR masks) 
2. The Experimenter can statically define a list of denied IP addresses (or CIDR masks) 
3. The Experimenter can statically define the default behaviour of the firewall  
4. The Experimenter can get the status of the firewall 
5. The Experimenter can get the rules installed on the firewall 
6. The Experimenter can dynamically add a rule to the firewall 
7. The Experimenter can dynamically update a rule on the firewall 
8. The Experimenter can dynamically remove a rule from the firewall 

## Resource properties
* **testbed**: Defines where to deploy the Security Resource selected. It is ignored if want_agent is True
* **want_agent**: Defines if the Experimenter wants the security resource to be an agent directly installed on the VM that he wants to monitor
* **ssh_key**: Defines the SSH public key to be pushed on the VM in order to be able to log into it
* **lan_name**: Select the network on which the VM is deployed (if __want_agent__ is False). If no value is entered, __softfire-internal__ is chosen
* **logging**: Defines if the Experimenter wants the security resource to send its log messages to a collector and he wants to see them on a dashboard
* **allowed_ips**: List of IPs (or CIDR  masks) allowed by the firewall. [allow from *IP*]
* **denied_ips**: List of IPs (or CIDR masks) denied by the firewall [deny from *IP*]
* **default_rule**: Default rule applied by the firewall (allow/deny)

API documentation
=================

Control your UFW firewall via Rest APIs. Please consider that this is a
Beta Version. Authentication will be probably added to the following
APIs.

Version information
-------------------

*Version* : 0.1

URI scheme
----------

*Host* : your.fw.ip:5000 *Schemes* : HTTP

Produces
--------

-   `text/json`

Paths
=====

Insert new rule
---------------

    POST /ufw/rules

### Description

Add a new rule to the firewall.

Rule text is sent in the body of the request.

**RULE SYNTAX**

Users can specify rules using either a simple syntax or a full syntax.
The simple syntax only specifies the port and optionally the protocol to
be allowed or denied on the host.

Example rules using the simple syntax: `allow 53`

This rule will allow tcp and udp port 53 to any address on the firewall
host. To specify a protocol, append */protocol* to the port.

Users can also use a fuller syntax, specifying the source and
destination addresses and ports. This syntax is loosely based on
OpenBSD’s PF syntax. For example:

`deny proto tcp to any port 80`

This will deny all traffic to tcp port 80.

`deny proto tcp from 10.0.0.0/8 to 192.168.0.1 port 25`

This will deny all traffic from the RFC1918 Class A network to tcp port
25 with the address 192.168.0.1.

### Responses

<table>
<colgroup>
<col width="10%" />
<col width="70%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">HTTP Code</th>
<th align="left">Description</th>
<th align="left">Schema</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>200</strong></p></td>
<td align="left"><p>A JSON object containing the list of rules installed in the firewall through this APIs.</p></td>
<td align="left"><p>No Content</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>400</strong></p></td>
<td align="left"><p>Systax Error. The rule sent in the body of the request is not correct and therefore it’s not included</p></td>
<td align="left"><p>No Content</p></td>
</tr>
</tbody>
</table>

List of rules
-------------

    GET /ufw/rules

### Description

List the rules installed in the firewall, coupled with a unique id.

### Responses

<table>
<colgroup>
<col width="10%" />
<col width="70%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">HTTP Code</th>
<th align="left">Description</th>
<th align="left">Schema</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>200</strong></p></td>
<td align="left"><p>A JSON object containing the list of rules installed in the firewall through this APIs.</p></td>
<td align="left"><p>No Content</p></td>
</tr>
</tbody>
</table>

Modify the existing rule
------------------------

    POST /ufw/rules/<rule_id>

### Description

Modify the rule with a new one

The new rule text is sent in the body of the request.

Rule syntax is the same as for new rules.

### Responses

<table>
<colgroup>
<col width="10%" />
<col width="70%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">HTTP Code</th>
<th align="left">Description</th>
<th align="left">Schema</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>200</strong></p></td>
<td align="left"><p>A JSON object containing the list of rules installed in the firewall through this APIs.</p></td>
<td align="left"><p>No Content</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>400</strong></p></td>
<td align="left"><p>Systax Error. The rule sent in the body of the request is not correct and therefore it’s not included</p></td>
<td align="left"><p>No Content</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>404</strong></p></td>
<td align="left"><p>Non existing rule. The id specified in the URL does not match the id of any rule in the firewall.</p></td>
<td align="left"><p>No Content</p></td>
</tr>
</tbody>
</table>

Delete a rule
-------------

    DELETE /ufw/rules/<rule_id>

### Description

Delete an existing rule from the firewall

### Responses

<table>
<colgroup>
<col width="10%" />
<col width="70%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">HTTP Code</th>
<th align="left">Description</th>
<th align="left">Schema</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>200</strong></p></td>
<td align="left"><p>A JSON object containing the list of rules installed in the firewall through this APIs.</p></td>
<td align="left"><p>No Content</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>404</strong></p></td>
<td align="left"><p>Non existing rule. The id specified in the URL does not match the id of any rule in the firewall.</p></td>
<td align="left"><p>No Content</p></td>
</tr>
</tbody>
</table>

GET /ufw/status
---------------

### Description

Show status of firewall and ufw managed rules. Equivalent to run
`ufw status` from Terminal

### Responses

<table>
<colgroup>
<col width="10%" />
<col width="70%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">HTTP Code</th>
<th align="left">Description</th>
<th align="left">Schema</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>200</strong></p></td>
<td align="left"><p>Shows if the firewall is in active or inactive status. If active it shows managed rules also.</p></td>
<td align="left"><p>No Content</p></td>
</tr>
</tbody>
</table>

### Produces

-   `text`


[fw-api]: firewall.adoc
