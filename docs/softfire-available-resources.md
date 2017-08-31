# SoftFIRE available resources for the experimenters


## Manager resources

The SoftFIRE platform provides 5 different kind of resources:

* NfvResource
* SdnResource
* SecurityResource
* MonitoringResource
* PhysicalResource

Each resource is available to all the Experimenters. Some of them are preconfigured, other can be highly configured.

| Resource | Description |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| NfvResource | The NFV Resource represents a Network Service in the ETSI NFV terminology. It can be one of the provided by the SoftFIRE Middleware or you can define your own that will be deployed in the SoftFIRE platform. More details can be found in the [NFV Manager](nfv-manager.md) page. The limitation on the virtual resources applies on this kind of resource. |
| SdnResource | The SDN Resource represents a SDN endpoint api, it can be an ODL endpoint or an OpenSDNCore endpoint protected by password. There is no limitation on usage of this resource. More details can be found on the [SDN Manager](sdn-manager.md) page |
| SecurityResource | The Security Resource provides a firewall or a set of scripts for configuring a firewall. In the first case, The limitation on the virtual resources will be applied. |
| MonitoringResource | The Monitoring Resource provides a Zabbix Server to the deployed experiment and configure the Nfv Resource to register to this server. There can only be one monitoring server per deployment |
| PhysicalResource | The Physical Resources only provides reservation of a physical cell and can be only accessed in the location of the cell |


## Virtual Resource limits

For what concerns the virtual resources quota, each experiment environment cannot cross these limits:

| Resource | Limit |
|-----------|-------|
| RAM (MB) | 20480 |
| Instances | 10 |
| VCPUs | 10 |
| Networks | 10 |

We believe that this is sufficient for the majority of the most complex experiments. Of course, by request, we could increase the limits per specific experiment. But it requires a valid motivation :wink:.
