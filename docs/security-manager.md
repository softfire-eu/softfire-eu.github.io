# Security Manager
The Security Manager inside the SoftFIRE Middleware makes available to the Experimenter a 
series of security related functionalities that she might decide to include and use within her 
activities on the SoftFIRE platform. 
Here is the list of the available features. 
1. The Experimenter can deploy a Security Resource 
2. The Experimenter can statically configure her Security Resource by means of its 
descriptor 
  a. The Experimenter can enable logs collection from her Resource 
  b. The Experimenter can statically configure some rules on her Resource 
3. The Experimenter can dynamically configure her Resource once it has been deployed 
4. The Experimenter can see her Resources logs in a web dashboard 
5. The Experimenter can perform searches among her Resources logs in a web dashboard 
6. The Experimenter can see statistics related to her Resources logs in a web dashboard  

## Security Resources 
A Security Resource is a commonly used security agent that the Experimenter can include in her 
experiment. She can access and configure it through a static initial configuration, included in the 
TOSCA description of the Resource, or, once deployed, through a REST interface that exposes its 
main services. 
The Experimenter can also ask the Security Resource to send its log messages to a remote log 
collector, which makes them available in a simple web page reserved to her. The Experimenter 
could easily access it through its web browser and check the behaviour of her all security agents, 
and to see some statistics. 
The Experimenter can get the Security Resource in two different formats: 

* As an agent directly installed in the VM that she wants to monitor. The system will 
provide her a script that the Experimenter has just to run inside the VM. It will be already 
configured as required in the TOSCA description of the resource. The output of the script 
will provide to the Experimenter information on how to access the deployed resource 
(URLs, etc.) 

* As a standalone VM the Security Resource will be deployed directly by the Security 
Manager in the testbed chosen by the Experimenter. The Security Manager will take 
care of the initial configuration of the resource. 
The Experimenter has to set up on her own the redirection of the network traffic that she wants 
to control through the Security Resource VM (by means of tunnelling or SDN capabilities).  


To date the only Security Resource available on the SoftFIRE environment is the [firewall][firewall].  


## Security Resource definition

```yaml
SecurityResource:
    derived_from: eu.softfire.BaseResource
    description: Defines a Security agent to be deployed. More details on [docu_url]
    properties:
        resource_id:
            type: string
            required: true
        testbed:
            type: string
            required: false
        want_agent:
            type: boolean
            required: true
        logging:
            type: boolean
            required: true
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

* **resource_id**: Defines the type of the Security Resource. To date only [firewall][firewall] is accepted
* **testbed**: Defines where to deploy the Security Resource selected. It is ignored if want_agent is True
* **want_agent**: Defines if the Experimenter wants the security resource to be an agent directly installed on the VM that she wants to monitor
* **logging**: Defines if the Experimenter wants the security resource to send its log messages to a collector and she wants to see them on a dashboard
* **allowed_ips**: List of IPs (or CIDR  masks) allowed by the firewall. [allow from *IP*]
* **denied_ips**: List of IPs (or CIDR masks) denied by the firewall [deny from *IP*]
* **default_rule**: Default rule applied by the firewall (allow/deny)


##### Testbed Names

| Alias    | Testbed                          |
|----------|----------------------------------|
| fokus    | FOKUS testbed, Berlin            |
| ericsson | ERICSSON testbed, Rome           |
| surrey   | SURREY testbed, Surrey           |
| ads      | ADS testbed, Rome                |
| dt       | Deutsche Telekom testbed, Berlin |
| any      | No difference                    |

## Technical details
This sequence diagram specifies the operations performed by the Security Manager based on the inputs received by the Experimenter.
![Security Manager sequence diagram][sequence]



<!--
 References
-->
 
[node_types]:etc/softfire_node_types.yaml
[firewall]:firewall.md
[docu_url]:http://docs.softfire.eu/security-manager/
[sequence]: img/security-manager.png
