# SoftFIRE

SoftFIRE is building a federated experimental platform aimed at the construction and experimentation of services and functionalities built on top of NFV and SDN technologies. The platform is a loose federation of already existing testbed owned and operated by distinct organizations for purposes of research and development.


SoftFIRE has three main objectives: supporting interoperability, programming and security of the federated testbed. Supporting the programmability of the platform is then a major goal and it is the focus of the SoftFIRE’s Second Open Call.

## The SoftFIRE testbeds

SoftFIRE federates different European testbeds owned by the partners of the project. Currently the federated Testbed are

* **RMED** Cloud Lab from Ericsson, located in Rome;
* **FUSECO Playground** from FOKUS Fraunhofer/TUB, located in Berlin;
* **5GIC** from University of Surrey, located in Guildford, Surrey;

New testbeds are now in the integration phase and may soon join the Federation, e.g.,

* **Deutsche Telekom**;
* **Assembly Data System** (ADS).

In the past other Testbed were integrate (and currently not available):

* **JoLNet** from TIM, spread over several Italian cities;


## The SoftFIRE architecture

Experimenters can access the available resources through a single access-point, i.e., the SoftFIRE Experiment Manager, Figure 1. This tool will be under development during the entire lifecycle of the project in order to progressively manage and orchestrate the allocation of several resources (Virtualization, SDN, 5G Resources, Security, and Monitoring). The Experiment Manager provides primitives to authenticate users and to discover, reserve, acquire, monitor and finally release a set of arbitrary resources of the infrastructure. Once a user has been given the authorization to access the system, he can perform experiments on top of the architecture for a certain amount of time. The SoftFIRE Experiment Manager (SEM) will ensure interoperability with other technologies by implementing the standard TOSCA interface.


![SoftFIRE architecture][softfire-architecture]

## Get in contact


* Our website: http://softfire.eu
* Sending us an email to [our mailing list](mailto:info@softfire.eu)



[softfire-architecture]:img/general-softfire-arch.png

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
