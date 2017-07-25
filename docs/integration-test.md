# SoftFIRE Integration Test

This project can be used to test the functionality of the experiment-manager and related components.

## Requirements

Python 3.5 or higher.

## Installation

Git clone the [repository][integration-test-github] and change into the project’s directory. Execute ```pip install .``` for installing the integration test. Afterwards you can start it with the command ```softfire-integration-test```.
Alternatively you can start it by running the project’s *softfire-integration-test* file with ```python softfire-integration-test```.

## Preparation

Copy the *integration-test.ini* file, which you can find in the project’s etc folder, to */etc/softfire/integration-test.ini*. Edit it so that it fits for your testing environment.

## Test phases

This section describes the test phases which are executed by the integration test. Each phase, except for the **Create experimenter** phase, use the user credentials provided in the configuration file in the *experimenter* section for accessing the experiment-manager.

### Preparation phase

First of all the integration test checks if the experiment file exists and scans it for the resources it uses. If something goes wrong in this phase, the remaining phases will not be executed and the test stops.

### Create experimenter

This phase is optional and is only executed if the *create-user* property in the configuration file is set to *true*. It tries to create a new experimenter in the experiment-manager whose username and password are also defined in the configuration file in the *experimenter* section. This is the only phase in which the *admin-username* and *admin-password* properties from the configuration file are used.

### Upload experiment

In this phase the experiment is uploaded to the experiment-manager. The experiment that shall be uploaded can be specified in the *experiment-file* property in the configuration file.

### Deploy experiment

This phase deploys the previously uploaded experiment.

### Validate experiment

In this phase the integration test checks whether all the resources defined in the experiment are running correctly. The exact validation procedure depends on the resource type. At the moment only resources of type *NfvResource* can be validated. This is done by waiting until the NSRs are in active state and then checking if the floating IP addresses of the NSR are reachable.

### Delete experiment

The last phase removes the experiment from the experiment-manager.

## Adding validators for other NfvResource types

Not all of the different resource types can be validated currently. This section describes how you can add the validation functionality for a certain node type to the integration test.

For this you have to create a new module and a new class in the softfire-integration-test project. In particular:

1. create a sub-module in the *eu.softfire.integrationtest.validators* module **called as the node type you want to validate** (for instance _NfvResource_ for the Nfv Resource)
1. create a class in the sub-module you just created called _NodeTypeValidator_ (for instance NfvResourceValidator)
1. the class must extend the _eu.softfire.integrationtest.validators.AbstractValidator_ class and implement the abstrac method _validate_

The abstrac class is:

```python
class AbstractValidator(metaclass=ABCMeta):
    @abstractmethod
    def validate(self, resource, resource_id):
        pass
```


The Validator class has to inherit from the AbstractValidator class and implement the *validate* method which gets the resource and the resource ID of the resource to test as an argument. In this method you can implement the validation of the resource. If the validation is successful, the method should simply return, otherwise it should raise an **Exception** (class of subclass of _Exception_)with the reason of failure as the Exception argument.

With the *get_resource_from_id(resource_id)* method you can get the _new_ status of the deployed experiment.

If you need an example look at the already existing *NfvResourceValidator* class in the *validators.py* module.


[integration-test-github]: https://github.com/softfire-eu/softfire-integration-test

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
