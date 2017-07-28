# Nfv tutorial Custom

This tutorial will show you how to create a SoftFIRE experiment with a custom NFV resource.  
This means that the experiment does not contain a Network Service provided by SoftFIRE, like it is the case in the [Nfv tutorial Iperf][nfv-tutorial-iperf], but that you will write your own custom Network Service.  
The Network Service that we write will be very simple and contain of two Virtual Network Functions (VNF), one acting as a server and one acting as a client.

In the first part we will create the experiment structure and in the second part we will add the csar archive containing the custom Network Service Descriptor (NSD) to it.


!!! Note
    This tutorial assumes that you use a Unix system.


## Create the experiment structure
First of all we need to create a folder which will contain all the necessary files.

```sh
mkdir custom-experiment
cd custom-experiment
````

then we have to add the sub folders

```sh
mkdir Files
mkdir TOSCA-Metadata
mkdir Definitions
```

and create files describing the experiment.

```sh
touch TOSCA-Metadata/TOSCA.meta
touch TOSCA-Metadata/Metadata.yaml
touch Definitions/experiment.yaml
```

The *TOSCA-Metadata/TOSCA.meta* file contains TOSCA specific configurations.

```sh
vim TOSCA-Metadata/TOSCA.meta
```

Here is the content of the file. You can change the *Created-By* value as you like.

```sh
TOSCA-Meta-File-Version: 1.0
CSAR-Version: 1.1
Created-By: SoftFIRE
Entry-Definitions: Definitions/experiment.yaml
```

The next file is the *TOSCA-Metadata/Metadata.yaml* file which contains information about the experiment.

```sh
vim TOSCA-Metadata/Metadata.yaml
```

The content has to look like this:

```sh
name: ExperimentIperf
start-date: "2017-7-2"
end-date: "2017-7-15"
```

Of course you can change the experiment\'s name, start and end date to your needs, but make sure you stick with the time format.

In the last file you specify the real definition of the experiment.

```sh
vim Definition/experiment.yaml
```

Here is an example for the file's content:

```yaml
description: "Template for SoftFIRE yaml resource request definition"
imports:
  - softfire_node_types: "http://docs.softfire.eu/etc/softfire_node_types.yaml"
topology_template:
  node_templates:
    customnsd:
      properties:
        resource_id: ubuntu_clean
        testbeds:
          ANY: fokus
        nsd_name: "ubuntu"
        file_name: Files/clean-ubuntu.csar
      type: NfvResource
tosca_definitions_version: tosca_simple_yaml_1_0
```

As you can see, the *node_templates* section contains a key named *customnsd*.
This is the resource which will contain our custom Network Service.
If you compare this experiment.yaml file with the one from the [Iperf tutorial][nfv-tutorial-iperf], you will recognize, that there are differences in the used fields.
The *resource_id* field changed from *iperf* to *ubuntu_clean*.
Since *iperf* is one of the provided Nfv resources by softfire, you could specify it in the experiment.yaml file and your experiment would use it.  
But now we want to use our own Nfv resource and therefore we change the resource_id to another value.
In this case we decided to name our resource_id *ubuntu_clean*.  
The next difference is the new *file_name* key. This points to the NSD which we want to use in our experiment and which we did not add yet.

!!! Note
    All the fields of this file are also explained in the [Nfv Manager page][nfv-manager-page].

## Defining the Network Service

Let's continue where we stopped in the previous section: the NSD file.  
We already specified in the experiment.yaml file where our custom NSD file resides, but we did not add it to our experiment yet.  
Since the file needs to be a csar archive (which is basically a zip archive with the .csar file ending) we will first create the folder structure and files and then compress it and put in its designated location.

!!! Note
    In-depth information about how to write a NSD with TOSCA and build a CSAR file with it can be found [here][openbaton-csar-tutorial].


Change into the *Files* directory

```sh
cd Files
```

and create the folder structure and the necessary files:

```sh
mkdir TOSCA-Metadata
mkdir Scripts
mkdir Definitions
touch TOSCA-Metadata/TOSCA.meta
touch TOSCA-Metadata/Metadata.yaml
```

In the *TOSCA-Metadata/TOSCA.meta* file reside TOSCA related meta information.

```sh
TOSCA-Meta-File-Version: 1.0
CSAR-Version: 1.1
Created-By: TUB
Entry-Definitions: Definitions/ubuntu.yaml
```

You can modify the *Created-By* property and the *Entry-Definition*, which points to the TOSCA file describing the NSD.

The *TOSCA-Metadata/Metadata.yaml* file contains information about the NSD.

```yaml
name: Ubuntu Example
description: Ubuntu Example.
provider: TUB
nfvo_version: 3.2.0
image:
    upload: false
    names:
            - ubuntu_14.04
vim_types:
    - openstack
```

You can see the NSD\'s name and description and the NFVO version with which this NSD is compliant.
Furthermore the used image and the VIM types are defined. You can change those values as you need.

After this we can start to write the NSD.

```sh
touch Definitions/ubuntu.yaml
vim Definitions/ubuntu.yaml
```

In this file we will put all the information about the Network Service that we want to be represented by the resource_id *ubuntu_clean*.

```yaml
description: "NS for deploying a clean ubuntu machine"
metadata:
  ID: Ubuntu
  vendor: TUB
  version: "3.2.0"
relationships_template:
  rel1:
    parameters:
      - key_1
      - key_2
      - softfire-internal
    source: ubuntuclient
    target: ubuntuserver
    type: tosca.nodes.relationships.ConnectsTo
  rel2:
    parameters:
      - key_1
      - key_2
      - softfire-internal
      - softfire-internal_floatingIp
    source: ubuntuserver
    target: ubuntuclient
    type: tosca.nodes.relationships.ConnectsTo
topology_template:
  node_templates:
    CP1:
      properties:
        floatingIP: random
      requirements:
        - virtualBinding: VDU1
        - virtualLink: softfire-internal
      type: tosca.nodes.nfv.CP
    CP2:
      properties:
        floatingIP: random
      requirements:
        - virtualBinding: VDU2
        - virtualLink: softfire-internal
      type: tosca.nodes.nfv.CP
    VDU1:
      properties:
        scale_in_out: 10
      requirements:
        - virtual_link: CP1
      type: tosca.nodes.nfv.VDU
    VDU2:
      properties:
        scale_in_out: 10
      requirements:
        - virtual_link: CP2
      type: tosca.nodes.nfv.VDU
    ubuntuclient:
      interfaces:
        lifecycle:
          INSTANTIATE:
            - install.sh
          CONFIGURE:
            - ubuntuserver_relation.sh
          START:
            - start.sh
      properties:
        configurations:
          configurationParameters:
            - key_1: value_1
            - key_2: value_2
            - key_3: value_3
          name: ubuntuclient-configuration
        deploymentFlavour:
          - flavour_key: m1.small
        endpoint: generic
        type: ubuntuclient
        vendor: TUB
        version: 16.04
      requirements:
        - virtualLink: softfire-internal
        - vdu: VDU1
      type: openbaton.type.VNF
    ubuntuserver:
      interfaces:
        lifecycle:
          INSTANTIATE:
            - install.sh
          CONFIGURE:
            - ubuntuclient_relation.sh
          START:
            - start.sh
      properties:
        configurations:
          configurationParameters:
            - key_1: value_1
            - key_2: value_2
            - key_3: value_3
          name: ubuntuserver-configuration
        deploymentFlavour:
          - flavour_key: m1.small
        endpoint: generic
        type: ubuntuserver
        vendor: TUB
        version: 16.04
      requirements:
        - virtualLink: softfire-internal
        - vdu: VDU2
      type: openbaton.type.VNF
tosca_definitions_version: tosca_clean_ubuntu
```

As already mentioned for more information about the format of this TOSCA NSD file visit [this][openbaton-csar-tutorial] site, as for now here is a shallow overview of the fields:

An NSD\'s main components are Virtual Network Function Descriptors (VNFD) which are connected and together form the NSD.
Each VNFD has its own functionality and role in a NSD.
The VNFDs in this example are specified in the yaml keys *ubuntuclient* and *ubuntuserver*.
In the end our Network Service will be built of a client and a server and the client will connect to the server.  
Each VNFD has lifecycles in which it executes lifecycle scripts. In our NSD you can see e.g. that the *ubuntuclient* VNFD runs a script called *install.sh* in its *INSTANTIATE* lifecycle.  
Furthermore VNFDs may have configuration parameters which are passed as key value pairs and are available in the scripts and an endpoint, which specifies the type of VNFM with which the VNFD shall be used.  
Each VNFD contains at least one Virtual Deployment Unit (VDU). With the help of VDUs it is possible to deploy a VNFD on different testbeds and to specify the maximum number of VNF instances which can be created by scaling out.
In our example the ubuntuclient only has one VDU, i.e. VDU1.  
Another important part of the NSD file are the relationships between VNFDs defined in the *relationships_template:* section.
Here you can specify dependencies which exist between VNFDs, e.g. if the client needs the IP address of the server.

Until now we only have the structure of our Network Service which consists of two VNFDs which themselves contain one VDU each.
But what about the functionality of the VNFDs? This is defined in the lifecycle scripts.
As we saw the VNFDs can have scripts which are executed in their different lifecycles.
The scripts are stored in the *Scripts* folder.
Let\'s create them.

```sh
mkdir -p Scripts/ubuntuserver
mkdir -p Scripts/ubuntuclient
touch Scripts/ubuntuserver/ubuntu_relation.sh
touch Scripts/ubuntuserver/start.sh
touch Scripts/ubuntuserver/install.sh
touch Scripts/ubuntuclient/ubuntuserver_relation.sh
touch Scripts/ubuntuclient/start.sh
touch Scripts/ubuntuclient/install.sh
```

The subfolder names correspond to the VNFD keys in the *Definitions/ubuntu.yaml* file.

Here are the contents of the script files:

###### Scripts/ubuntuserver/install.sh

```sh
#!/bin/bash

echo "installing!!!"
echo "Avoid long outputs! so redirect the output to a file that you can later check, for instance:"
sudo apt-get update > /tmp/install.log
echo "apt update worked!"
sudo apt-get install -y cowsay >> /tmp/install.log

echo "check the env before running special commands"
env
echo "like this one..."
/usr/games/cowsay -f tux "install successfull"
```

This is the first script executed on the server. Here you can install software you need later.

###### Scripts/ubuntuserver/ubuntu_relation.sh

```sh
#!/bin/bash

echo "Better to redirect output..."

sudo apt-get install -y figlet > /tmp/configure.sh

echo "first foreign key"
figlet $ubuntuclient_key_1
echo "second foreign key"
figlet $ubuntuclient_key_2
echo "foreign private ip"
echo "$ubuntuclient_softfire_internal"
echo "foreign floating ip"
echo "$ubuntuclient_softfire_internal_floatingIp"
```

This script prints out some of the values passed by the dependency, e.g. the client\'s configuration parameters and floating IP address.
The *CONFIGURE* lifecycle is the only one which has access to the dependency parameters so use it to configure your VNF.

###### Scripts/ubuntuserver/start.sh
```sh
#!/bin/bash

echo "start!!!"
echo "Avoid long outputs! so redirect the output to a file that you can later check, for instance:"
echo "log description here" > /tmp/start.log
/usr/games/cowsay -f tux "start finished worked!"
```

The script executed in the *START* lifecycle of the server.

###### Scripts/ubuntuclient/install.sh
```sh
#!/bin/bash

echo "installing!!!"
echo "Avoid long outputs! so redirect the output to a file that you can later check, for instance:"
sudo apt-get update > /tmp/install.log
echo "apt update worked!"
sudo apt-get install -y cowsay >> /tmp/install.log
echo "Check the env before running special commands!"
env
echo "this one for instance..."
/usr/games/cowsay -f tux "install successfull"
```

###### Scripts/ubuntuclient/ubuntuserver_relation.sh
```sh
#!/bin/bash

sudo apt-get install -y figlet > /tmp/configure.sh

echo "first foreign key"
figlet $ubuntuserver_key_1
echo "second foreign key"
figlet $ubuntuserver_key_2
echo "foreign private ip"
echo "$ubuntuserver_softfire_internal"
echo "foreign floating ip"
echo "$ubuntuserver_softfire_internal_floatingIp"
```

###### Scripts/ubuntuclient/start.sh

```sh
#!/bin/bash

echo "start!!!"
echo "Avoid long outputs! so redirect the output to a file that you can later check, for instance:"
echo "log description here" > /tmp/start.log
/usr/games/cowsay -f tux "start finished worked!"
```

This is everything we need for our NSD. Now we can archive everything as a csar archive.

```sh
zip -r clean-ubuntu.csar . -x ".*" -x "*/.*"
```

Now that the archive is created you can delete the files and directories if you want to.

The last step is to compress also the complete experiment to a csar file.

```sh
cd ..
zip -r my-experiment.csar . -x ".*" -x "*/.*"
```

## Conclusion
In this tutorial we created an experiment with a custom Nfv resources.
For this we created an experiment as usual, but instead of referencing a resource_id which is already provided by SoftFIRE, we used our own.
Then we created a NSD written in TOSCA and built a csar archive for it.
We placed the archive in the experiments *Files* folder and compressed the whole experiment into a csar archive.  
For uploading and running the experiment you can refer to the [Iperf tutorial][nfv-tutorial-iperf-last-section] as the procedure is the same.


<!--
References:  
-->


[nfv-tutorial-iperf]:nfv-tutorial-iperf.md
[nfv-tutorial-iperf-last-section]:nfv-tutorial-iperf#putting-all-together
[nfv-manager-page]:nfv-manager.md
[openbaton-csar-tutorial]:http://openbaton.github.io/documentation/tosca-CSAR-onboarding

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
