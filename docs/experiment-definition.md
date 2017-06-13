# Experiment definition

The experiment is defined using [_Topology and Orchestration Specification for Cloud Applications_][tosca] (TOSCA). In particular, the Experimenter has to create a [CSAR][tosca-csar] zip file containing all the necessary files and definitions for letting the [ExperimentManager](experiment-manager.md) (EM) instantiate everything. The structure of the CSAR is defined as following:

```bash
├── Definitions
|   └── experiment.yaml
├── Files
|   └── nsd.csar
└── TOSCA-Metadata
    ├── Metadata.yaml
    └── TOSCA.meta
```
There are three two mandatory folders plus one optional. _Definitions_ and _TOSCA-Metadata_ are mandatory folder containing the metadata files and the experiment definition, as described in the following sections. the _Files_ folder contains some additional files needed in case some resources specifies extra requirements.

## TOSCA-Metadata

The TOSCA-Metadata folder contains the TOSCA.meta file and the Metadata.yaml file. The TOSCA.meta file must contain the reference to the template in this case `:::yaml Entry-Definitions: Definitions/experiment.yaml`. For example:

```yaml
TOSCA-Meta-File-Version: 1.0
CSAR-Version: 1.1
Created-By: MyCompany
Entry-Definitions: Definitions/experiment.yaml
```

The Metadata.yaml contains experiment meta information:

* the name
* the start date
* the end date

!!! note
    Please use ISO format dates (YYYY-MM-DD HH:MM).

!!! note
    These dates are general for the whole experiment, but you can specify specific dates per resource. The specific dates of the resources override these ones.

As follows:
```yaml
name: Experiment Name
start-date: "2017-07-28 10:00"
end-date: "2017-07-30 10:00"
```

Where:

* **name**: simple name of your experiment
* **start-date**: start date of your experiment.
* **end-date**: end date of your experiment.


## Definitions

The experiment.yaml must follow a specific structure. An example is show in the following lines:

```yaml
tosca_definitions_version: tosca_simple_yaml_1_0

description: Template for SoftFIRE yaml resource request definition

imports:
  - softfire_node_types: http://docs.softfire.eu/etc/softfire_node_types.yaml

topology_template:
  node_templates:
    zabbix_server:
      type: MonitoringNode
      properties:
        resource_id: monitoring
        testbed: ericsson

    sdn_ericsson:
      type: SdnResource
      properties:
        resource_id: sdn_ericsson
        testbed: ericsson

    iperf:
      type: NfvResource
      properties:
        resource_id: iperf
        nsd_name: The Iperf NSD
        testbeds: { ALL: ericsson }
```

As shown in the example, the SoftFIRE experiment yaml file must contain the TOSCA definition version as `:::yaml tosca_definitions_version: tosca_simple_yaml_1_0`. This line is followed by a description of the experiment.

The imports section must be specified as in the example because the EM will only accept specific node types defined in [this document][node_types]

Each node type specifies a `:::text resource_id` that must be chosen from the available resources. The node name is arbitrary. Each node type can have some additional properties and they can be different from each others. Check the [node type specification][node_types] to understand all the node types. However, each node type is specified in the specific manager page:

* **SDNResource**: [SDN Manager](sdn-manager.md)
* **NFVResource**: [NFV Manager](nfv-manager.md)
* **PhysicalResource**: [Physical Device Manager](pd-manager.md)
* **MonitoringResource**: [Monitoring Manager](monitoring-manager.md)
* **SecurityResource**: [Security Manager](security-manager.md)

### Topology Template

The topology template describe the actual experiment. The required nodes are listed in this section. The types are defined in the [node types][node_types] definitions, please refer to the specific manager page for the description of the type.

## Files

This folder contains an inner CSAR of a NS. This is only used in case the NFV Resource you want to deploy is not one of the preconfigured one. In this case, the how to build this CSAR is explained in the [Open Baton CSAR tutorial][openbaton-tosca]. And the NfvResource _file_name_ field must point to this file. An example of two Ubuntu 16.04 VNFs can be found [here][nfv-example]

## Example

A full example using iperf can be found [here](etc/example.csar). A full example uploading two simple Ubuntu VNFs can be found [here][nfv-example]
<!--
References
-->

[openbaton-tosca]:https://openbaton.github.io/documentation/tosca-CSAR-onboarding/
[tosca-csar]:http://docs.oasis-open.org/tosca/TOSCA-Simple-Profile-YAML/v1.0/csd03/TOSCA-Simple-Profile-YAML-v1.0-csd03.html#_Toc419746172
[tosca-simple]:http://docs.oasis-open.org/tosca/TOSCA-Simple-Profile-YAML/v1.0/csd03/TOSCA-Simple-Profile-YAML-v1.0-csd03.html
[tosca]:http://docs.oasis-open.org/tosca/TOSCA/v1.0/TOSCA-v1.0.html
[tosca-node-types]:http://docs.oasis-open.org/tosca/TOSCA-Simple-Profile-YAML/v1.0/csprd01/TOSCA-Simple-Profile-YAML-v1.0-csprd01.html#DEFN_TYPE_CAPABILITIES_NODE
[node_types]:etc/softfire_node_types.yaml
[nfv-example]:etc/nfv-resource-example.csar

<!---
 Script for open external links in a new tab
-->
<script type="text/javascript" charset="utf-8">
      // Creating custom :external selector
      $.expr[':'].external = function(obj){
          return !obj.href.match(/^mailto\:/)
                  && (obj.hostname != location.hostname);
      };
      $(function(){
        $('a:external').addClass('external');
        $(".external").attr('target','_blank');
      })
</script>
