# Security Manager
 
This is a Security Manager page

## Security Resource
```yaml
SecurityResource:
    derived_from: eu.softfire.BaseResource
    description: Defines a Security agent to be deployed. More details on <docu_URL>
    properties:
        resource_id:
            type: string
            required: true
            default: firewall
        testbed:
            type: string
            required: false
        want_agent:
            type: boolean
            required: true
            default: true
        logging:
            type: boolean
            required: true
            default: true
        allowed_ips:
            type: list
            entry_schema:
                type: string
            required: false
        denied_ips:
            type: list
            entry_schema:
            type: string
            required: false
        default_rule:
            type: string
            required: true
 ```
 
 This node type has different properties:

* **resource_id**: Defines the type of the Security Resource. To date only 'firewall' is accepted
* **testbed**: Defines where to deploy the Security Resource selected. It is ignored if want_agent is True
* **want_agent**: Defines if the Experimenter wants the security resource to be an agent directly installed on the VM that she wants to monitor
* **logging**: Defines if the Experimenter wants the security resource to send its log messages to a collector and she wants to see them on a dashboard
* **allowed_ips**: List of IPs (or CIDR  masks) allowed by the firewall. [allow from \<IP\>]
* **denied_ips**: List of IPs (or CIDR masks) denied by the firewall [deny from \<IP\>]
* **default_rule**: Default rule applied by the firewall (allow/deny)

 <!--
 References
 -->
 
 [node_types]:etc/softfire_node_types.yaml

