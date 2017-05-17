# Monitoring Manager
The Monitoring Manager provides proper resources to experimenters requiring monitoring service.
For experimenters requiring monitoring services, the Monitoring Manager provides, via OpenBaton, the
installation of an additional Virtual Machine pre-configured with Zabbix
Server.
All Virtual Machines requested by the experimenter will be installed with Zabbix Agent, per-configured to
communicate with experimenter’s Zabbix Server.
The experimenters receive full administrations rights of Zabbix Server, in order to be able to configure the
server according the specific needs of the experimenter.
## Monitoring resource


The MonitoringResource node type is defined as follows, as per node types page: node types page



```yaml

MonitoringResource:
  
derived_from: eu.softfire.BaseResource

    description: Defines the Zabbix monitoring resource requested

    properties:

      testbed:

        type: string
       
        required: false
       
        description: Location where to deploy the monitoring server



```


 
<!--
MonitoringResource:   
-->
This node type has a single property:
***testbeds**: in case the experimenter requires deployment of VMs on more than one
     testbed is it possible to define on which testbed the Zabbix Server VM
     will be deployed. 
 
[node_types]:etc/softfire_node_types.yaml
