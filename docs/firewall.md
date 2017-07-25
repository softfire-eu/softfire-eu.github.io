# Firewall SecurityResource
A firewall is a network security system that monitors and controls the incoming and outgoing
network traffic based on predetermined rules.
The available firewall resource is built upon Ubuntu UFW (Uncomplicated FireWall), to which a
control system, based on a ReST interface, has been added.  
The firewall agent is available for Ubuntu OS only.  
The rules that can be defined on this type of firewall are stateless (they do not maintain
information about the context). It works as a packet filter, which looks at network addresses,
ports and protocols.  

Services specifically available for the firewall Resource are:

1. The Experimenter can statically define a list of allowed IP addresses (or CIDR masks)
2. The Experimenter can statically define a list of denied IP addresses (or CIDR masks)
3. The Experimenter can statically define the default behaviour of the firewall  
4. The Experimenter can get the status of the firewall
5. The Experimenter can get the rules installed on the firewall
6. The Experimenter can dynamically add a rule to the firewall
7. The Experimenter can dynamically update a rule on the firewall
8. The Experimenter can dynamically remove a rule from the firewall

[Here][fw-api] you can find the API definition to configure the firewall.

[fw-api]: etc/firewall.adoc

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
