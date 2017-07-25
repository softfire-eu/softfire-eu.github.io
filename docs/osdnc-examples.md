# SoftFIRE OpenSDNcore examples

Fraunhofer FOKUS provides two datacenter as part of its testbed for the SoftFIRE Project. The testbed identified as `fokus-dev` will provide SDN features based on OpenSDNcore.

## Setting up the Experiment

1. Define the experiment to use the `fokus-dev` testbed to launch virtual machines.
1. Include the resource `sdn-controller-opensdncore-fokus` to enable access to the SDN features.
1. after the successfill deployment of the experiment the sdn-manager resurnes the details needed to access the OpenSDNcore API.

```json
{
	"resource_id": "sdn-controller-opensdncore-fokus",
	"flow-table-range": [30, 31, 32],
	"token": "secret",
	"URI": "http://172.20.30.5:8001/api"
}
```
1. Copy the `token` value and navigate to the provided `URI` using a web browser. The website provides the needed information and an simple user interface to run JSON-RPC request against the OpenSDNcore [Northbound-API](opensdncore-nb-api). Use the provided token value to identify your experiment when doing API requests.

## Port Mirroring example
The following example will utilize a custom flow entry to duplicate all network traffic directed at a Virtual Machine and forward it to the network interface of another Virtual Machine.

1. after all the instances are bootet up correctly use the `ofc.list.channels` command to list all switches in the setup.
1. in our case there is only one switch with dpid "0x0000000000000001" present

```json
{
	"jsonrpc": "2.0",
	"result": [
		"0x0000000000000001"
	],
	"id": 1
}
```
1. find the port number to which the traffic should be mirrored to by using the `ofc.send.multipart.port_description` function and searching for the MAC address of the target instance.
1. use the discovered `port_no`, `dpid` and the provate IP-address of the instance which traffic should be duplicted to construct a openflow definition that will duplicate each network-packet to the target port of the monitoring instance.
1. add the new flow via the following json-rpc query to the switch into one of the flow tables that are assigned to your experiment (ex. 30,31,32).

```json
{
  "id":2342,
  "jsonrpc":"2.0",
  "method":"ofc.send.flow_mod",
  "params":{
     "dpid":"0x0000000000000001",			/* address of the target switch */
     "ofp_flow_mod":{
        "command":"add",
        "flags":[
           "reset_counts",
           "send_flow_rem"
        ],
        "idle_timeout":100,
        "ofp_instructions":{
           "apply_actions":{
              "output": {
                 "port": "0x05"				/* port number of the mirror port */
              }
           },
           "write_actions":{
              "output": {
                 "port": "0x01"				/* port number of the original destination instance */
              }
           }
        },
        "ofp_match":[
           {
             "match_class": "openflow_basic",
                "field": "ipv4_dst",
                "value": "192.168.66.22"	/* the private ip address of the target virtual machine */
           }
        ],
        "priority":400,
        "table_id":"0x1e" 					/* flow_table 30 in hex notation */
     }
  }
}
```


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
