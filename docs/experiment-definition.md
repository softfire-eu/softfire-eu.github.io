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

## TOSCA-Metadata

The TOSCA-Metadata folder contains the TOSCA.meta file. This file must contain the reference to the template in this case `:::yaml Entry-Definitions: Definitions/experiment.yaml`.

```yaml
TOSCA-Meta-File-Version: 1.0
CSAR-Version: 1.1
Created-By: MyCompany
Entry-Definitions: Definitions/experiment.yaml
```

## Definitions

The experiment.yaml must follow a specific structure. An example is show in the following lines:

```yaml
tosca_definitions_version: tosca_simple_yaml_1_0

description: Template for SoftFIRE yaml resource request definition

imports:
  - softfire_node_types:
    file: http://docs.softfire.eu/etc/softfire_node_types.yaml

topology_template:
  node_templates:
    zabbix_server:
      type: MonitoringNode

    sdn_ericsson:
      type: ODLController
      properties:
        testbed: ericsson

    iperf:
      type: NfvResource
      properties:
        nsd_name: iperf
        testbed: ericsson
```

As shown in the example, the SoftFIRE experiment yaml file must contain the TOSCA definition version as `:::yaml tosca_definitions_version: tosca_simple_yaml_1_0`. This line is followed by a description of the experiment.

The imports section must be specified as in the example because the EM will only accept specific node types defined in [this document][node_types]

### Topology Template

The topology template describe the actual experiment. The required nodes are listed in this section. The types are defined in the [node types][node_types] definitions.

#### MonitoringNode

#### ODLController

#### NfvResource

#### OSDNController

#### SecurityResource

## Files

<!--
References
-->

[openbaton-tosca]:https://openbaton.github.io/documentation/tosca-CSAR-onboarding/
[tosca-csar]:http://docs.oasis-open.org/tosca/TOSCA-Simple-Profile-YAML/v1.0/csd03/TOSCA-Simple-Profile-YAML-v1.0-csd03.html#_Toc419746172
[tosca-simple]:http://docs.oasis-open.org/tosca/TOSCA-Simple-Profile-YAML/v1.0/csd03/TOSCA-Simple-Profile-YAML-v1.0-csd03.html
[tosca]:http://docs.oasis-open.org/tosca/TOSCA/v1.0/TOSCA-v1.0.html
[tosca-node-types]:http://docs.oasis-open.org/tosca/TOSCA-Simple-Profile-YAML/v1.0/csprd01/TOSCA-Simple-Profile-YAML-v1.0-csprd01.html#DEFN_TYPE_CAPABILITIES_NODE
[node_types]:etc/softfire_node_types.yaml
