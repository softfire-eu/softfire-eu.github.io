# Experiment Manager

After having won the Open Call you are officially an Experimenter. First of all, you should have received a certificate that will be used for authentication of the [SoftFIRE VPN][openvpnconfig]. Once the SoftFIRE VPN is active you are able to reach this page, the [Experimenter Manager][ex-man-link]. This is the page you will see:

![Experimenter Manager Login page][ex-man-login-page]

## Registration

If it is the first time you access you must register first. Enter your username and a chosen password plus your email.

!!! note
    The username must be the same that the one you used while registering.

If the signup works correctly you will be redirected on the Experimenter page that looks like the following picture.

![Experimenter Manager Experimenter page][ex-man-experimenter-page]


<!--
References
-->

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
