## Registration to the Tools

1. Register at the [Web Portal](https://portal.softfire.eu/login/) (and win the open call :stuck_out_tongue_winking_eye:). From your personal page you will be able to download the OpenVPN certificate configuration file that will allow you to enter the SoftFIRE VPN.
1. Register to redmine [this page](https://redmine.softfire.eu/) and create an account
1. Install and configure [openvpn](#openvpn-setup) using the certificate configuration file previously downloaded
1. Design your experiment
1. Execute the experiment
1. Retrieve the results
1. Terminate the experiment

We invite the experimenters to use the SoftFIRE [Slack channel](https://softfire.slack.com/messages) (if you did not register yet please do so [here](https://softfire-slacking.herokuapp.com/)) the Issues to address specific problems or support requests. You can also visit the [Forums](https://redmine.softfire.eu/projects/softfire/boards) section to open a new discussion or to find useful information shared in the community

## OpenVPN setup

1. Install [OpenVPN][openvpn] for your platform
1. use the configuration file downloaded from the SoftFIRE Web Portal
1. start OpenVPN client with administrative rights (or sudo on linux) using the configuration file

!!! note
    if the configuration file download does not work, use [this guide](openvpnconfig) for creating the configuration file manually

## The Experiment Manager

Good, now you are in :smile: You should be able to reach now the [Experiment Manager Web page][ex-man-link]. Anyway we immagine that your experiment could be very complex, involving many components. The most critical part is the definition of the NFV resources, that involves many automated scripts execution. For this reason we suggest to start with a local installation of [Open Baton](http://openbaton.github.io/documentation/) using a local OpenStack instance (this could be also a very small installation for testing purposes). This method will help you and us to interface with the SoftFIRE middleware, having for sure a working NFV resource definition.

After this step you can continue proceeding locally (if you wish) with a small deployment of the SoftFIRE Middleware, for this we suggest to build your own docker container solution using a docker compose file we provide. The tutorial can be fund [here][docker-compose]. Follow this tutorial on the [next page][ex-manager] for getting more knowledge on how to proceed with your experiment.


## Testbeds Information and usage

* SoftFIRE infrastructure monitoring [ZABBIX](https://zabbix.softfire.eu)

<!--
  References
-->

[openvpn]:https://openvpn.net/
[ex-man-link]:http://experiment.vpn.softfire.eu:5080/
[ex-manager]:experiment-manager.md
[docker-compose]:install-softfire-middleware-docker.md

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
