# Nfv tutorial Monitoring-VNF

This tutorial will show you how to create a monitoring resources as a Virtual Network Funtion (VNF).
The Network Service that we write will be very simple and contain of three VNFs, one acting as an iperf-server, one acting as an iperf-client and one acting as a monitoring resource (Zabbix-server).

This tutorial focuses on creating the Network Service Descriptor (NSD) which you can use in your experiement. 


!!! Note
    This tutorial assumes that you use a Unix system.


## Defining the Network Service
  
We are assuming that you know how to create a custom NFV resource from the NFV cutom tutorial, so we will focus on the main differences that you should consider when defining the network service.

!!! Note
    In-depth information about how to write a NSD with TOSCA and build a CSAR file with it can be found [here][openbaton-csar-tutorial].


### Creating the nsd.yaml file

The structure of the zabbix server VNF descriptor in the nsd yaml file should be similar to the below example:

```yaml
    zabbixserver: #VNF1
        type: openbaton.type.VNF
        properties:
          ID: zabbix-server
          vendor: FOKUS
          version: 1.0
          endpoint: generic
          type: zabbix
          configurations:
            configurationParameters:
              - name: zabbix
            name: zabbixserver-configuration
          deploymentFlavour:
            - flavour_key: m1.small
        requirements:
          - virtualLink: softfire-internal
          - vdu: VDU3
        interfaces:
          lifecycle: # lifecycle
            INSTANTIATE:
              - install.sh
```

The most important parameters should be configured in the zabbix server VNF descriptor example are: type, ConfigurationParameters, and the lifecycle INSTANTIATE script (install.sh) to install and configure the zabbix server.

For all other VNFs who will need to be monitored, a lifecycle event CONFIGURE script (zabbix_configure_agent.sh) should be added to install and configure zabbix agent as shown in example below for the iperf-client:

```yaml
    iperf-client:
      type: openbaton.type.VNF
      properties:
        ID: client-vnf
        vendor: FOKUS
        version: 1.0
        type: client
        deploymentFlavour:
          - flavour_key: m1.small

        endpoint: generic
      requirements:
         - virtualLink: Server-Network
         - vdu: VDU1
      interfaces:
          lifecycle: # lifecycle
            INSTANTIATE:
              - install.sh
            CONFIGURE:
              - zabbix_configure_agent.sh
```

The same as above should be configured for iperf-server.

In the nsd yaml, the relation dependency should be provided to let zabbix server be ready first then configure the zabbix agent in the monitored VNFs as the example shown below:

```yaml
relationships_template:
  rel1: 
    type: tosca.nodes.relationships.ConnectsTo
    source: zabbixserver
    target: iperf-client
    parameters:
        - softfire-internal_floatingIp
        - name
```

For the relation dependency, the zabbixserver should be the source and other VNFs should be the target. Also adding the floating ip and the name paramaters is a must, as the zabbix_agent_configuration script will be based on them.

For this solution, Ubuntu-14.04 image should be used for the zabbix-server VNF. That can be configured in the nsd yaml in the VDU part by adding the artifacts paramter including the image file required as shown in the example below. This way will add more flexibility in using different images per the VNFs.

```yaml
    VDU2: # This VDU is used by other VNF
      type: tosca.nodes.nfv.VDU
      properties:
        scale_in_out: 2
      requirements:
        - virtual_link: CP2
      artifacts:
        VDU2Image:
          type: tosca.artifacts.Deployment.Image.VM
          file: Ubuntu-16.04
    VDU3: # This VDU is used by the zabbix server
      type: tosca.nodes.nfv.VDU
      properties:
        scale_in_out: 2
      requirements:
        - virtual_link: CP3
      artifacts:
        VDU3Image:
          type: tosca.artifacts.Deployment.Image.VM
          file: Ubuntu-14.04

```

### Configuring the TOSCA-Metadata

The configuration of the image name in the Metadata.yaml file should be removed as we already define the images files in the nsd.yaml file. An exmaple is shown below:

```yaml

name: Iperf Example
description: Iperf Example.
provider: FOKUS
nfvo_version: 5.0.1
image:
    upload: false

vim_types:
    - openstack
```

### The required Scripts files:

Here are the contents of the script files:

###### Scripts/client/install.sh 
###### Scripts/server/install.sh
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
echo "Installing IPERF"
sudo apt-get install -y iperf screen
echo " iperf is installed"
```
This is the first script executed on the iperf-server and iperf-client VNFs.

###### Scripts/client/zabbix_configure_agent.sh 
###### Scripts/server/zabbix_configure_agent.sh
```sh
#!/bin/bash

if [[ -z "$zabbix_softfire_internal_floatingIp" ]]; then
  echo "softfire_internal_floatingIp is required"
  exit 1
fi

export MONITORING_IP=$zabbix_softfire_internal_floatingIp

echo "Installing zabbix-agent for server at $MONITORING_IP"

sudo apt-get install -y zabbix-agent
sudo sed -i -e "s/ServerActive=127.0.0.1/ServerActive=$MONITORING_IP:10051/g" -e "s/Server=127.0.0.1/Server=$MONITORING_IP/g" -e "s/Hostname=Zabbix server/#Hostname=/g" /etc/zabbix/zabbix_agentd.conf
sudo service zabbix-agent restart
echo "finished installing zabbix-agent!"
```
This is the second script executed for configuring the zabbix-agent for the iperf-server and iperf-client VNFs.
This script uses the values passed by the dependency to configure the zabbix-agent, e.g. the zabbix-server configuration parameters and floating IP address.

The scripts needed to install zabbix-server are:

###### Scripts/zabbix/install.sh 
```sh
#!/usr/bin/env bash
# asterisk installation script and configuration for sipas

# sane defaults
SERVICE="zabbix"
LOGFILE="$HOME/install.log"
CONFIG_FOLDER="/etc/zabbix/"

# If there are default options load them 
if [ -f "$SCRIPTS_PATH/default_options" ]; then
	  source $SCRIPTS_PATH/default_options
fi

echo "$SERVICE : Installing zabbix server 3.0"
sudo su
wget http://repo.zabbix.com/zabbix/3.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_3.0-1+trusty_all.deb
dpkg -i zabbix-release_3.0-1+trusty_all.deb
rm zabbix-release_3.0-1+trusty_all.deb
echo "$SERVICE : Added zabbix repo"

export DEBIAN_FRONTEND=noninteractive
mysql_pass=""
apt-get update && apt-get -y install mysql-server zabbix-server-mysql zabbix-frontend-php
echo "$SERVICE : Finished installing packages"

sed -i 's/;date.timezone =/date.timezone = "Europe\/Berlin"/' /etc/php5/apache2/php.ini
sed -i 's/# DBPassword=/DBPassword=/' /etc/zabbix/zabbix_server.conf
cp $SCRIPTS_PATH/zabbix.conf.php /etc/zabbix/web/zabbix.conf.php 

mysql -uroot -e "create database zabbix character set utf8 collate utf8_bin;" --password=""
mysql -uroot -e "grant all privileges on zabbix.* to zabbix@localhost identified by '';" --password=""
zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | mysql -uroot zabbix

service apache2 reload
service zabbix-server restart
echo "$SERVICE : Changed configuration and restarted service"

wget https://raw.githubusercontent.com/openbaton/juju-charm/develop/hooks/zbx_helper.py

python zbx_helper.py -a
```

This script will install and configure zabbix server. It also makes use of the below files during the installation.

###### Scripts/zabbix/zabbix.conf.php
```sh
<?php
// Zabbix GUI configuration file.
global $DB;
$DB['TYPE']     = 'MYSQL';
$DB['SERVER']   = 'localhost';
$DB['PORT']     = '0';
$DB['DATABASE'] = 'zabbix';
$DB['USER']     = 'zabbix';
$DB['PASSWORD'] = '';
// Schema name. Used for IBM DB2 and PostgreSQL.
$DB['SCHEMA'] = '';
$ZBX_SERVER      = 'localhost';
$ZBX_SERVER_PORT = '10051';
$ZBX_SERVER_NAME = '';
$IMAGE_FORMAT_DEFAULT = IMAGE_FORMAT_PNG;
```

###### Scripts/zabbix/default_options
```sh
LOGFILE="/home/ubuntu/install.log"
SERVICE="zabbix"
CONFIG_FOLDER="/etc/zabbix/"

```


This is everything we need to aware about while configuring a monitoring-VNF in our NSD. Now we can archive everything as a csar archive.

## Conclusion
In this tutorial we created an NSD has a monitoring VNF.
For this we showed the main differences with the NFV custom tutorial that need to be considered.
We showed the needed scripts for installing zabbix-server and zabbix-agent in the other VNFs.

A Complete NSD TOSCA files are exist as a full example [here][monitoring-vnf].

<!--
References:  
-->


[monitoring-vnf]:https://github.com/softfire-eu/monitoring-vnf
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

