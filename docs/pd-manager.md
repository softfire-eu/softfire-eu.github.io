# Physical Device Manager

The Physical Device Manager only provides (for the second wave) reservation of physical resources. For that reason the only thing to be specified is the **resource_id**, that can be checked in the resource discovery phase. Please read carefully the description for avoiding misunderstanding. Anyway a definition of the NodeType follows:


```yaml
PhysicalResource:
  derived_from: eu.softfire.BaseResource
  description: "Defines a Physical resource request in the SoftFIRE Middleware"
  properties:
    start-date: "2017-08-01"
    start-date: "2017-08-10"
```

!!! note
    Regarding the PhysicalResource, it is recommended to specify start and end date since it is not possible to reserve a physical resource for a long period and for that reason the whole experiment could be rejected


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
