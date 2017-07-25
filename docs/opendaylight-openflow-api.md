# OpenDaylight API

## Introduction
The testbeds which integrate OpenDaylight controller provide to the experimenter the access to MD-SAL Restconf API for OpenFlow Plugin. [1]


## Usage
As described in [SDN Manager documentation](sdn-manager), after successful instantiation of the resources, the experimenter receives the URI of the proxied OpenDaylight controller, a token and the list of tables assigned to him.

When the experiment starts, for every OpenFlow node, all the flows of table 0 are redirected to the first table assigned to the user and then go to table 17.

![controller_color__1_](img/odl_experimenter_tables.png)


Then, the experimenter can make JSON Restconf requests to the proxied SDN controller. Every request must contain in the header the token received, and the experimenter can create/edit/delete flows only in the tables assigned to him.

The created flows can forward packets only to other experimenter owned tables or to the table 17.

As action, in the attribute **"output-node-connector"** are **allowed** only the values **"TABLE"** and **"IN_PORT"**, or a **port_id**
The values **"ALL"**, **"CONTROLLER"**, **"ANY"**, **"LOCAL"**, **"NORMAL"**, **"FLOOD"** are **not allowed**.

For further information about these values, you can read the [official Openflow documentation](https://www.opennetworking.org/images/stories/downloads/sdn-resources/onf-specifications/openflow/openflow-spec-v1.3.0.pdf)

All the requests that not satisfy the above requirements, will be rejected with an http status code = 403

!!! note
    You can call Restconf services as described in the [OpenDaylight official documentation](https://wiki.opendaylight.org/view/OpenDaylight_OpenFlow_Plugin:End_to_End_Flows#Push_your_flow), but you can't use xml format and have to add the header **'API-Token'**. No Basic Authentication header is needed.


Here below some examples of Restconf requests:


### REST get nodes example:
* Headers:
	* Content-type: application/json
	* Accept: application/json
	* API-Token: <experimenter-token>
* URL:
	http://[HSOTNAME]:[PORT]/restconf/config/opendaylight-inventory:nodes/
* Method: GET
* Request body: EMPTY

* Response body:
```json
	{
		"nodes": {
			"node": [
				{
					"id": "openflow:257478269489732",
					"node-connector": [
						{
							"id": "openflow:257478269489732:4",
							...
							"flow-node-inventory:name": "tapfa76a4ad-58",
							"flow-node-inventory:configuration": "PORT-UP",
							"address-tracker:addresses": [
								{
									"id": 0,
									"ip": "10.0.0.10",
									"mac": "fa:16:3e:a9:9e:39",
									"first-seen": 1497436298506,
									"last-seen": 1497436298506
								}
							]
						},
						{
							"id": "openflow:257478269489732:2",
							"flow-node-inventory:name": "tap40c62f4c-f7",
							"flow-node-inventory:configuration": "PORT-DOWN",
							"address-tracker:addresses": [
								{
									"id": 2,
									"ip": "10.0.0.2",
									"mac": "fa:16:3e:0d:c6:40",
									"first-seen": 1497436298793,
									"last-seen": 1497448932221
								}
							]
						},
						{
							"id": "openflow:257478269489732:1",
							...
							"flow-node-inventory:name": "tapd0bff1a4-2d",
							"flow-node-inventory:configuration": "PORT-DOWN",
							"address-tracker:addresses": [
								{
									"id": 1,
									"ip": "10.0.0.2",
									"mac": "fa:16:3e:60:fd:aa",
									"first-seen": 1497436298646,
									"last-seen": 1497448932220
								}
							]
						},
						{
							"id": "openflow:257478269489732:7",
							...
							"flow-node-inventory:name": "tap62ce8677-d8",
							"address-tracker:addresses": [
								{
									"id": 3,
									"ip": "10.0.0.3",
									"mac": "fa:16:3e:93:ff:b6",
									"first-seen": 1497448932261,
									"last-seen": 1497448932261
								}
							]
						}
						...
					]
				}
			]
		}
	}
```


### REST put flow example - drop all packets with destination ip equals to 10.0.10.2/24:

* Headers:
	* Content-type: application/json
	* Accept: application/json
	* API-Token: <experimenter-token>
* URL:
	http://[HSOTNAME]:[PORT]/restconf/config/opendaylight-inventory:nodes/node/openflow:72664714402125/table/2/flow/1
* Method: PUT
* Request body:

```json
	{
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
    }
```




## Examples
The official OpenDaylight OpenFlow Plugin documentation can be found [here](https://wiki.opendaylight.org/view/OpenDaylight_OpenFlow_Plugin:End_to_End_Flows)

Examples for XML for various flow matches, instructions & actions can be found [here](https://wiki.opendaylight.org/view/Editing_OpenDaylight_OpenFlow_Plugin:End_to_End_Flows:Example_Flows), but remember, the SoftFIRE OpenDaylight controller accepts only JSON requests

## References
[1]: https://wiki.opendaylight.org/view/OpenDaylight_Controller:MD-SAL:Restconf OpenDaylight MD-SAL Documentation



<!---
 Script for open external links in a new tab
-->
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.js"></script>
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
