# SecurityResource tutorial firewall

This tutorial will guide you through the definition of the firewall example and to upload it.

!!! Note
    This tutorial assumes that you use a Unix system.

Create all the folders needded

```
$ mkdir firewall-softfire
$ cd firewall-softfire
$ mkdir Files
$ mkdir TOSCA-Metadata
$ mkdir Definitions
```

now we need to create the necessary files.

```
touch TOSCA-Metadata/TOSCA.meta
touch TOSCA-Metadata/Metadata.yaml
touch Definitions/experiment.yaml
```

The TOSCA-Metadata/TOSCA.meta file contains TOSCA specific configurations:

```
vim TOSCA-Metadata/TOSCA.meta
```

Here you must write something like this:

```
TOSCA-Meta-File-Version: 1.0
CSAR-Version: 1.1
Created-By: SoftFIRE
Entry-Definitions: Definitions/experiment.yaml
```

You can change the Created-By property, if you wish.

then we do the same with TOSCA-Metadata/Metadata.yaml that contains Metadata info of the experiment

```
vim TOSCA-Metadata/Metadata.yaml
```

Here you should write something like this:

```yaml
name: ExperimentIperf
start-date: "2017-7-2"
end-date: "2017-7-15"
```

Change start-date and end-date fields according to your experiment.
now we can write the definition of the experiment:

```sh
vim Definitions/experiment.yaml
```

Here you should write something like this:

```yaml
description: "Template for SoftFIRE yaml resource request definition"
imports:
  - softfire_node_types: "http://docs.softfire.eu/etc/softfire_node_types.yaml"
topology_template:
  node_templates:
    f:
      type: SecurityResource
      properties:
        resource_id: firewall
        want_agent: false
        lan_name: private
        testbed: fokus
        default_rule: allow
        denied_ips: [172.20.10.138]
        logging: true
```

all the fields are explained in the [Security manager page](security-manager.md)

Create the CSAR file:

```sh
zip -r firewall.csar . -x ".*" -x "*/.*"
```

Now you have to uploaded this file to the Experiment Manager GUI like the following images.
Go to [experimenter page](http://experiment.vpn.softfire.eu:5080/experimenter) and click on "Reserve Resource"

![tutorial firewall 1](img/tutorial-security-manager-firewall1.png)

![tutorial firewall 2](img/tutorial-security-manager-firewall2.png)

![tutorial firewall 3](img/tutorial-security-manager-firewall3.png)

When you click on send, the Managers will validate the resources, in case you did some mistake they should be shown now. If no errors are shown, you are able to deploy your resources by clicking deploy. Please consider that it takes some seconds to deploy the resource dependening in the experiment definition.
Once deployed, it will take time until reaching the ACTIVE state of the resource. A button on the right enable the automatic refresh of the resources' values or disable it, in the second case you will need to refresh the page.
Finally the firewall resource reaches the ACTIVE state and also the floating ip are shown in the newly created resource section of the experimenter GUI. Note that the floating ip changes for every deployment.
You are now able to ssh into the client.

```sh
~ $ ssh -i /opt/softfire/key/softfire-key.pem ubuntu@<HERE THE FLOATING IP>
Warning: Permanently added '172.20.30.91' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.2 LTS (GNU/Linux 4.4.0-77-generic x86_64)

* Documentation:  https://help.ubuntu.com
* Management:     https://landscape.canonical.com
* Support:        https://ubuntu.com/advantage

Get cloud support with Ubuntu Advantage Cloud Guest:
http://www.ubuntu.com/business/services/cloud

87 packages can be updated.
45 updates are security updates.


Last login: Thu Jul 20 14:41:18 2017 from 172.20.10.131
ubuntu@server-9771458:~$
```

Then remember always to delete the resources by clicking in the delete button.
