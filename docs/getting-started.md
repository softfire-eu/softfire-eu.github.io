## Registration to the Tools

1. Register at the Web Portal (and win the open call :stuck_out_tongue_winking_eye:)
1. Register to redmine [this page](https://redmine.softfire.eu/) and create an account
1. Install and configure [openvpn](#openvpn-setup)
1. Design your experiment
1. Execute the experiment
1. Retrieve the results
1. Terminate the experiment

We invite the experimenters to use the SoftFIRE [Slack channel]() the Issues to address specific problems or support requests. You can also visit the [Forums](https://redmine.softfire.eu/projects/softfire/boards) section to open a new discussion or to find useful information shared in the community

## OpenVPN setup

1. Install [OpenVPN][openvpn] for your platform
1. use the configuration file downloaded from the SoftFIRE Web Portal
1. start OpenVPN client with administrative rights (or sudo on linux) using the configuration file

!!! note
    if the configuration file download does not work, use [this guide](openvpnconfig) for creating the configuration file manually

## The Experiment Manager

Good, now you are in :smile: You should be able to reach now the [Experiment Manager Web page][ex-man-link]. Follow this tutorial on the [next page][ex-manager] for getting more knowledge on how to proceed with your experiment.

## Testbeds Information and usage

* SoftFIRE infrastructure monitoring [ZABBIX](https://zabbix.softfire.eu)

<!--
  References
-->

[openvpn]:https://openvpn.net/
[ex-man-link]:http://experiment.vpn.softfire.eu:5080/
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
