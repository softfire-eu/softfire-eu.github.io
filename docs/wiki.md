## Registration to the Tools

1. register at the portal (alredy done)
1. register to redmine [this page](https://redmine.softfire.eu/) and create an account (Introduction to the redmine tool: [REDMINE](https://redmine.softfire.eu/documents/3))
1. install and configure [openvpn](#openvpn-setup)
1. configure [jfed](#jfed-setup)
1. [prepare your experiment](#design-your-experiment)
1. [execute the experiment](#execute-your-experiment)
1. retrieve the results

We invite the experimenters to use the Issues to address specific problems or support requests. You can also visit the [Forums](https://redmine.softfire.eu/projects/softfire/boards) section to open a new discussion or to find useful information shared in the community

## OpenVPN setup

1. Install openVPN for your platform
1. use the configuration file downloaded from the portal
1. start OpenVPN client with administrative rights (or sudo on linux) using the configuration file

!!! note
    if the download does not work create the configuration file manually ([OpenVpnConfig](openvpnconfig))

## jFed setup

Here is described the small procedure to follow in order to get *jFed* up and running.

### *Pre-requisites*

* Java 8 installed
* OpenVPN installed and connected
* Download the jFed.zip from [here](https://owncloud.tu-berlin.de/index.php/s/tB1eKv8yCRUheTR). Password for the download is *softfire*
* unzip the jFed.zip
* Certificate and private key for jFed in one file, called e.g. *_cert.pem_*

### *Steps*

* Get the following files:
  * [authorities.xml](etc/authorities.xml)
  * [config.xml](etc/config.xml)
* Put the authorities.xml to *<your_home_folder\>/.jfed/*

* Start jFed:  _java -jar experimenter-gui-5.6.3-jfx.jar --config=<path to your config.xml\>_
* Point to your certificate and enter the chosen password

## Design your experiment

[This document](etc/design_the_nfv_solution.pdf)  provides a detailed tutorial on how to implement your experiment

## Execute your experiment
| Option 1 using jFed                                                                               | Option 2 direct usage of OpenBaton                                                                  |
|---------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Start jFed: *java -jar experimenter-gui-5.6.3-jfx.jar --config=<path to your config.xml\>*        | access openbaton machine at [openbaton.vpn.softfire.eu:5081](http://openbaton.vpn.softfire.eu:5081) |
| Point to your certificate and enter the chosen password                                           | request credential to your mentor if you don't have it yet                                          |
| Go to the [Softfire Software Portal](http://172.20.30.52:5083) and log in                         | you have full rights in your project                                                                |
| Upload your package                                                                               | upload the VNFPackage in the correct page from the menu                                             |
| In jFed click *New*                                                                               | if the package is correct, it will result in a new VNF Descriptor                                   |
| Drag and drop a *Generic Node* to the topology canvas                                             | combine multiple VNF Descriptor in the NSD create page, creating then the NSD                       |
| Doubleclick or Rightclick and select *Configure Node*  the node (Properties window should appear) | launch the NSD. This will result in the NSR, your experiment                                        |
| Name the node as you like                                                                         | -                                                                                                   |
| Select testbed *fiteagle.vpn.softfire.eu* from the *Select Testbed* dropdown                      | -                                                                                                   |
| Select *Specific Node*                                                                            | -                                                                                                   |
| Select your package from the dropdown                                                             | -                                                                                                   |
| Click *Save*                                                                                      | -                                                                                                   |
| Click the green arrow *Run* button                                                                | -                                                                                                   |
| Name your experiment (experiment names should be unique!)                                         | -                                                                                                   |
| Remove the check from *Project*                                                                   | -                                                                                                   |
| Click *Start Experiment*                                                                          | -                                                                                                   |
| Wait for some seconds until your Node is coloured dark green                                      | -                                                                                                   |
| Your experiment should be ready and you can connect to the node via SSH                           | -                                                                                                   |

## Testbeds Information and usage

* SoftFIRE infrastructure monitoring [ZABBIX](https://zabbix.softfire.eu)
* Interconnection Benchmarks: [VPNBenachmark](vpnbenchmarklink)

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
