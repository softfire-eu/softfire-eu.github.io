= Firewall API


[[_overview]]
== Overview
Control your UFW firewall via Rest APIs. 
Please consider that this is a Beta Version. Authentication will be probably added to the following APIs.


=== Version information
__Version__ : 0.1


=== URI scheme
__Host__ : your.fw.ip:5000
__Schemes__ : HTTP


=== Produces

* `text/json`




[[_paths]]
== Paths

[[_ufw_rules_post]]
=== Insert new rule
....
POST /ufw/rules
....


==== Description
Add a new rule to the firewall.

Rule text is sent in the body of the request. 

*RULE SYNTAX*

Users can specify rules using either a simple syntax or a full syntax. The simple syntax only specifies the port and optionally the protocol to be allowed or denied on the host.

Example rules using the simple syntax: `allow 53`

This rule will allow tcp and udp port 53 to any address on the firewall host. To specify a protocol, append '/protocol' to the port.

Users can also use a fuller syntax, specifying the source and destination addresses and ports. This syntax is loosely based on OpenBSD's PF syntax. For example:

`deny proto tcp to any port 80`

This will deny all traffic to tcp port 80.

`deny proto tcp from 10.0.0.0/8 to 192.168.0.1 port 25`

This will deny all traffic from the RFC1918 Class A network to tcp port 25 with the address 192.168.0.1.


==== Responses

[options="header", cols=".^2,.^14,.^4"]
|===
|HTTP Code|Description|Schema
|**200**|A JSON object containing the list of rules installed in the firewall through this APIs.|No Content
|**400**|Systax Error. The rule sent in the body of the request is not correct and therefore it's not included|No Content
|===


[[_ufw_rules_get]]
=== List of rules
....
GET /ufw/rules
....


==== Description
List the rules installed in the firewall, coupled with a unique id.


==== Responses

[options="header", cols=".^2,.^14,.^4"]
|===
|HTTP Code|Description|Schema
|**200**|A JSON object containing the list of rules installed in the firewall through this APIs.|No Content
|===


[[_ufw_rules_rule_id_post]]
=== Modify the existing rule
....
POST /ufw/rules/<rule_id>
....


==== Description
Modify the rule with a new one

The new rule text is sent in the body of the request. 

Rule syntax is the same as for new rules.


==== Responses

[options="header", cols=".^2,.^14,.^4"]
|===
|HTTP Code|Description|Schema
|**200**|A JSON object containing the list of rules installed in the firewall through this APIs.|No Content
|**400**|Systax Error. The rule sent in the body of the request is not correct and therefore it's not included|No Content
|**404**|Non existing rule. The id specified in the URL does not match the id of any rule in the firewall.|No Content
|===


[[_ufw_rules_rule_id_delete]]
=== Delete a rule
....
DELETE /ufw/rules/<rule_id>
....


==== Description
Delete an existing rule from the firewall


==== Responses

[options="header", cols=".^2,.^14,.^4"]
|===
|HTTP Code|Description|Schema
|**200**|A JSON object containing the list of rules installed in the firewall through this APIs.|No Content
|**404**|Non existing rule. The id specified in the URL does not match the id of any rule in the firewall.|No Content
|===


[[_ufw_status_get]]
=== GET /ufw/status

==== Description
Show status of firewall and ufw managed rules.
Equivalent to run `ufw status` from Terminal


==== Responses

[options="header", cols=".^2,.^14,.^4"]
|===
|HTTP Code|Description|Schema
|**200**|Shows if the firewall is in active or inactive status. If active it shows managed rules also.|No Content
|===


==== Produces

* `text`







