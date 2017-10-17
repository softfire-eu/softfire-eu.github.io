# Security Manager
The Security Manager inside the SoftFIRE Middleware makes available to the Experimenters a
set of security related functionalities that they might decide to include and use within their
activities on the SoftFIRE platform.

Here is the list of the available features for every type of Resource.

1. The Experimenters can deploy a Security Resource
2. The Experimenters can statically configure the Security Resource by means of its descriptor
1. The Experimenter can statically configure some features on her Resource
3. The Experimenters can dynamically configure the Resource once it has been deployed

Features **not** available for Resource _pfsense_

1. The Experimenters can enable logs collection from their Resource
4. The Experimenters can see Resources logs in a web dashboard
5. The Experimenters can perform searches among the Resources logs in a web dashboard
6. The Experimenters can see statistics related to the Resources logs in a web dashboard  

## Security Resources
A Security Resource is a commonly used security agent that the Experimenters can include in their
experiment. They can access and configure it through a static initial configuration, included in the
TOSCA description of the Experiment, or, once deployed, through the interfaces that expose its
main services. These interfaces can include SSH, a dashboard, or ReST APIs.
Depending on the type of Resource, Experimenters can also ask the Security Resource to send its log messages to a remote log
collector, which makes them available in a simple web page reserved to them. The Experimenters
could easily access it through its web browser and check the behaviour of all their security agents,
and to see some related statistics.

The Experimenters can get the Security Resource in two different formats:

* As an agent directly installed in the VM that they want to monitor. The system will
provide them a script that the Experimenters have just to run inside the VM. It will be already
configured as required in the TOSCA description of the resource. The output of the script
will provide to the Experimenters information on how to access the deployed resource
(URLs, etc.)

* As a standalone VM. The Security Resource will be deployed directly by the Security
Manager in the testbed chosen by the Experimenter. The Security Manager will take
care of the initial configuration of the resource.
The Experimenters have to set up on their own the redirection of the network traffic that they want
to control through the Security Resource VM (by means of OS configuration, or SDN capabilities provided by the SoftFIRE platform).  

The Security Manager provides three types of resources:

* [firewall][firewall]
* [suricata][suricata]
* [pfsense][pfsense]


## Security Resource definition
In this section the attributes that can be defined for a TOSCA node of type _SecurityResource_ are listed. Please note that
the meaning of the specific property depends on the type of resource (specified in the *resource_id* field). 
For more details, refer to the specific sections of the documentation. 

```yaml
SecurityResource:
    derived_from: eu.softfire.BaseResource
    description: "Defines a Security agent to be deployed. More details on [docu_url]"
    properties:

        resource_id:
            type: string
            required: true

        testbed:
            type: string
            required: false

        lan_name:
            type: string
            required: false

        wan_name: 
            type: string
            required: false
            description: valid for pfsense

        ssh_key:
            type: string
            required: false
            description: valid for firewall and suricata

        want_agent:
            type: boolean
            required: false
            description: valid for firewall and suricata
       
        logging:
            type: boolean
            required: false
            description: valid for firewall and suricata
        
        allowed_ips:
            type: list
            entry_schema:
                type: stringThis node type
            required: false
            description: valid for firewall
        
        denied_ips:
            type: list
            entry_schema:
                type: string
            required: false
            description: valid for firewall
        
        default_rule:
            type: string
            required: false
            description: valid for firewall
        
        rules: 
            type: 
            required: false
            description: valid for suricata
```

Every node has different properties. Here they are listed for each type of resource:

**resource_id = [firewall][firewall]**

* **testbed**: Defines where to deploy the Security Resource selected. It is ignored if want_agent is True
* **want_agent**: Defines if the Experimenter wants the security resource to be an agent directly installed on the VM that he wants to monitor
* **ssh_key**: Defines the SSH public key to be pushed on the VM in order to be able to log into it
* **lan_name**: Select the network on which the VM is deployed (if __want_agent__ is False). If no value is entered, __softfire-internal__ is chosen
* **logging**: Defines if the Experimenter wants the security resource to send its log messages to a collector and he wants to see them on a dashboard
* **allowed_ips**: List of IPs (or CIDR  masks) allowed by the firewall. [allow from *IP*]
* **denied_ips**: List of IPs (or CIDR masks) denied by the firewall [deny from *IP*]
* **default_rule**: Default rule applied by the firewall (allow/deny)

**resource_id = [suricata][suricata]**

* **testbed**: Defines where to deploy the Security Resource selected. It is ignored if want_agent is True
* **want_agent**: Defines if the Experimenter wants the security resource to be an agent directly installed on the VM that he wants to monitor
* **ssh_key**: Defines the SSH public key to be pushed on the VM in order to be able to log into it
* **lan_name**: Select the network on which the VM is deployed (if __want_agent__ is False). If no value is entered, __softfire-internal__ is chosen
* **logging**: Defines if the Experimenter wants the security resource to send its log messages to a collector and he wants to see them on a dashboard
* **rules**: Defines the list of rules to be configured in Suricata VM. These rules follow the syntax 

**resource_id = [pfsense][pfsense]**

* **testbed**: Defines where to deploy the Security Resource selected
* **wan_name**: Selects the network on which the first interface of the VM is attached. It is configured as WAN on pfSense. It must be a network connected to the SoftFIRE-public network 
* **lan_name**: Selects the network on which the second interface of the VM is attached. It is configured as LAN on pfSense


##### Testbed Names

| Alias    | Testbed                          |
|----------|----------------------------------|
| fokus    | FOKUS testbed, Berlin            |
| ericsson | ERICSSON testbed, Rome           |
| surrey   | SURREY testbed, Surrey           |
| ads      | ADS testbed, Rome                |

## Technical details
This sequence diagram specifies the operations performed by the Security Manager based on the inputs received by the Experimenter.
![Security Manager sequence diagram][sequence]



<!--
 References
-->

[node_types]:etc/softfire_node_types.yaml
[firewall]:firewall.md
[suricata]:suricata.md
[pfsense]:pfsense.md
[docu_url]:http://docs.softfire.eu/security-manager/
[sequence]:security-manager.png



