# Tutorial Iperf + ODL-SDN

This tutorial will guide you through the definition of the iperf and odlsdn example.

Take your _experiment.yaml_ file, and write something like this :
```yaml

---
sdn_ads:
      properties:
        resource_id: sdn-controller-odl-ads
      type: SdnResource
```

The new _experiment.yaml_ file will be something like this
```yaml

--- 
description: "Template for SoftFIRE yaml resource request definition"
imports: 
  - softfire_node_types: "http://docs.softfire.eu/etc/softfire_node_types.yaml"
  
topology_template: 
  node_templates: 


    iperf:
      type: NfvResource
      properties:
        resource_id: iperf
        nsd_name: The Iperf NSD
        testbeds: { ANY: ads }
        ssh_pub_key: "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDNo3y5JdeIzeIBbpQEEtjn/BgBjTzyAo7HeSPAy9tZfXpOt0P/rGaflRSiAOTk+P+kHs9GMFQrA3nfk6z9Ass18BTUmtNvovyQphqcEAAAADAQABAAABAQDNDtULVF+n3/znwENEga+5Fl6qvVzWWMepb02q41VvaZy/NoMHnw9+NwNiM1BY9AAy2+Z6AIg8CJ1EvIZTPlD7a7RveSjLHZLuVeBGZdOky1EQf+m8VpPvM2axrdtluch/bJXPKVJQhF7Wc4HSFxdAhxhGPSeyNMPqQ/EcevZQMic0qJ82GKsWFm5M+Fy4x1wsOG5aJ918Za29aiKMrUv8Borod7b2YCBb"


    sdn_ads:
      properties:
        resource_id: sdn-controller-odl-ads
      type: SdnResource


tosca_definitions_version: tosca_simple_yaml_1_0
```

after that create the CSAR file, upload to the Experiment Manager GUI. 
If there are no mistakes, you are able to deploy your resources by clicking deploy.
Once deployed, in the details of the experiment a section is displayed for sdn-controller-odl-ads

!!! Note
    To create, upload a CSAR file and deploy your resources view a [tutorial](http://docs.softfire.eu/nfv-tutorial-iperf/).

The value contains somethings like this :
```json

{
    "resource_id": "sdn-controller-odl-ads",
    "token": "x1x1x1x1x1x1x1x1x1x1x1x1x",
    "flow-table-range": [
        2,
        3,
        4
    ],
    "URI": "http://172.20.70.130:8001/"
}
```
- "token" is needed to call the rest API 
- "flow-table-range" are the tables that odl assigned to the experimenter
- "URI" is the ODL endpoint used for RESTCONF API requests

![Provide resources](img/tutorial-iperf+odl-sdn.png)

### REST put flow example - drop all packets with destination ip equals to 10.0.10.2/24:

```sh
curl -X PUT \
  'http://<URI>/restconf/config/opendaylight-inventory:nodes/node/openflow:72664714402125/table/2/flow/1' \
  -H 'accept: application/json' \
  -H 'api-token: <token>' \
  -H 'cache-control: no-cache' \
  -d '{
      "flow-node-inventory:flow": [
        {
          "id": "1",
          "flow-name": "Foo",
          "match": {
            "ipv4-destination": "10.0.10.2/24",
            "ethernet-match": {
              "ethernet-type": {
                "type": 2048
              }
            }
          },
          "priority": 2,
          "table_id": 2,
          "instructions": {
            "instruction": [
              {
                "order": 0,
                "apply-actions": {
                  "action": [
                    {
                      "order": 0,
                      "drop-action": {}
                    }
                  ]
                }
              }
            ]
          }
        }
      ]
    }'
```

Then remember always to delete the resources by clicking in the delete button.
!!! Note
    When you delete the experiment, OLD will de-associate and clean the tables that were assigned.