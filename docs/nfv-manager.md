# NFV Manager

The NFV Manager is the manager interfacing with Open Baton. In case the Experimenter wants to deploy a different NS from the preconfigured ones, it is required to follow [this tutorial][openbaton-tosca]

## NFV Resource

The NfvResource node type is defined as follows, as per [node types page][node_types]:

```yaml
NfvResource:
    derived_from: eu.softfire.BaseResource
    description: "Defines a NFV resource request in the SoftFIRE Middleware"
    properties:
      testbeds:
        type: map
        entry_schema:
          description: "mapping between vnf types and testbed. Or
                       'ANY':<testbed_name> for all in one"
          type: string
      nsd_name:
        type: string
        description: "Name of the NS to deploy"
      file_name:
        type: string
        description: "relative path to the Open Baton CSAR. It is always starting with Files/..."
        required: false
      ssh_pub_key:
        type: string
        required: false
        description: "the public ssh key that will be injected in the VM in order to give access to the experimenter"

```

This node type has different properties:

* **resource_id**: The resource id that can be found in the dashboard's resource tables.
* **testbeds**: A map defining on which testbed a VNF's Virtual Deployment Unit will be deployed. It's key value pairs look like this **VDU name**: **testbed name**
* **nsd_name**: The name of the Network Service (NS). Open Baton will use this name when storing the NSD.
* **file_name**: In case the preconfigured NS are not sufficient for your experiment you can upload your own NS in CSAR format and place it in the experiment's **Files** folder. This field contains the name of the file.
* **ssh_pub_key**: In order to access the VMs via SSH, you need to add your public key here so that you can access the deployed experiment's VMs if they have floating IPs.

##### Testbed Names

| Alias    | Testbed                          |
|----------|----------------------------------|
| fokus    | FOKUS testbed, Berlin            |
| fokus-dev| FOKUS testbed with SDN support, Berlin            |
| ericsson | ERICSSON testbed, Rome           |
| surrey   | SURREY testbed, Surrey           |
| ads      | ADS testbed, Rome                |
| dt       | Deutsche Telekom testbed, Berlin |
| any      | No difference                    |


## Technical details

In the following sequence diagram you can see the life cycle of the Nfv Manager.

![NfvManager sequence diagram](img/nfv-manager.svg)

<!--
References
-->

[node_types]:etc/softfire_node_types.yaml
[openbaton-tosca]:https://openbaton.github.io/documentation/tosca-CSAR-onboarding/


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
