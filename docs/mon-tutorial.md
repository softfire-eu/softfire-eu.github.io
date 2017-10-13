# Monitoring Tutorial -- Preliminary Version

This tutorial will guide you through the definition of Monitoring example and to upload it.

!!! Note
    This tutorial assumes that you use a Unix system.

Create all the folders needded

```
$ mkdir monitor-softfire
$ cd monitor-softfire
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

Here you must write something like the following example:

```
TOSCA-Meta-File-Version: 1.0
CSAR-Version: 1.1
Created-By: SoftFIRE
Entry-Definitions: Definitions/experiment.yaml
```

If you wish, you can change the Created-By property.

then we do the same with TOSCA-Metadata/Metadata.yaml that contains Metadata info of the experiment

```
vim TOSCA-Metadata/Metadata.yaml
```

Here you should write something like the following example:

```yaml
name: Experiment Name
start-date: "2017-08-10"
end-date: "2017-08-11"
```

Change start-date and end-date fields according to your experiment.
now we can write the definition of the experiment:

```sh
vim Definitions/experiment.yaml
```

Here you should write something like the following example:

```yaml
description: "Template for SoftFIRE yaml resource request definition"
imports:
  - softfire_node_types: "http://docs.softfire.eu/etc/softfire_node_types.yaml"
topology_template:
  node_templates:
    m:
      type: MonitoringResource
      properties:
        testbed: ericsson
        lan_name: Zabbix_Lan
        resource_id: monitor

```

all the fields are explained in the [Monitoring manager page](monitoring-manager.md)

Create the CSAR file:

```sh
zip -r monitor.csar . -x ".*" -x "*/.*"
```

Now you have to uploaded this file to the Experiment Manager GUI like the following images.
Go to [experimenter page](http://experiment.vpn.softfire.eu:5080/experimenter) and click on "Reserve Resource"

![tutorial Monitor 1](img/Monitor_Tutorial-01.PNG)

![tutorial Monitor 2](img/Monitor_Tutorial-02.PNG)

![tutorial Monitor 3](img/Monitor_Tutorial-03.PNG)

![tutorial Monitor 4](img/Monitor_Tutorial-04.PNG)

![tutorial Monitor 5](img/Monitor_Tutorial-05.PNG)

![tutorial Monitor 6](img/Monitor_Tutorial-06.PNG)

xxxxxx
xxxxxx
xxxxxx
xxxxxx




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
