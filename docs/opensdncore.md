# Fraunhofer FOKUS OpenSDNCore

## Introduction

## API Documentation

The OpenSDNcore provides an JSON RPC based northbound API to control all aspects of the SDN implementation.
The details of the provided api function calls are described in the [Northbound API page][osdnc-api]

## Architecture

![controller_color__1_](img/osdnc-controller1.png)

More Details please refer to the [OpenSDNCore Controller documentation][osdnc-controller]

<!--
References
-->

[osdnc-api]:opensdncore-nb-api.md
[osdnc-controller]:osdnc-controller.md

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
