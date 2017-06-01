# Experiment Manager

After having won the Open Call you are officially an Experimenter. First of all, you should have received a certificate that will be used for authentication of the [SoftFIRE VPN][openvpnconfig]. Once the SoftFIRE VPN is active you are able to reach this page, the [Experimenter Manager][ex-man-link]. This is the page you will see:

![Experimenter Manager Login page][ex-man-login-page]

## Login

Enter your username and password, the password is the same used in the SoftFIRE Web Portal and the username is your _name+surname_. The _Signup_ is currently disabled

!!! note
    The username must be the same that the one you used while registering.

If the login works correctly you will be redirected on the Experimenter page that looks like the following picture.

![Experimenter Manager Experimenter page][ex-man-experimenter-page]

## Resource discovery

By reloading the page, you are also refreshing the list of available resources. These resources have a detailed description and an id. The id will be used in the [definition of the experiment](experiment-definition.md), for pointing to the resources you want to reserve. For more details on how to define the experiment, please check the [next page](experiment-definition.md).

## Resource reservation

For reserving resources, you must define an experiment using the [TOSCA archive][tosca-csar]. Then you have to upload this archive file in the input box of _Reserve resources_ section. Once the experiment is reserved you will be able to see it under the _Defined experiment_ section

## Resource provisioning

Once you have uploaded your experiment CSAR file, you will have a list of chosen resources in the bottom table. The _**value**_ of the resources will be empty until deployed. By clicking to the "Deploy" button, you will trigger the deployment in the SoftFIRE middleware. The status will change to deployed and you will actually see the content of the deployed resource in the value column.

## Resource Termination

By clicking to the "Delete" button, you will trigger the removal of all the resources created. You will have to reserve the experiment again in case you want to redeploy
<!--
References
-->

[tosca-csar]:http://docs.oasis-open.org/tosca/TOSCA-Simple-Profile-YAML/v1.0/csd03/TOSCA-Simple-Profile-YAML-v1.0-csd03.html#_Toc419746172
[openvpnconfig]:openvpnconfig.md
[ex-man-link]:http://experimenter-manager.vpn.softfire.eu
[ex-man-login-page]:img/em-login.png
[ex-man-experimenter-page]:img/em-experimenter.png

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
