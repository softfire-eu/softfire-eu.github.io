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

1. Install [OpenVPN][openvpn] for your platform
1. use the configuration file downloaded from the SoftFIRE Web Portal
1. start OpenVPN client with administrative rights (or sudo on linux) using the configuration file

!!! note
    if the configuration file download does not work, use [this guide](openvpnconfig) for creating the configuration file manually

## The Experiment Manager

Good, now you are in :smile: You should be able to reach now the [Experiment Manager Web page][ex-man-link]. Follow this tutorial on the [next page][ex-man-link] for getting more knowledge on how to proceed with your experiment.

## Testbeds Information and usage

* SoftFIRE infrastructure monitoring [ZABBIX](https://zabbix.softfire.eu)
* Interconnection Benchmarks: [VPNBenachmark](vpnbenchmarklink)

<!--
  References
-->

[openvpn]:https://openvpn.net/
[ex-man-link]:http://experiment-manager.vpn.softfire.eu
[ex-manager]:experiment-manager.md

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
