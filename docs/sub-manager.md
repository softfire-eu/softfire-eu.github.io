# Write your own manager

This guide is intended for internal developer of SoftFIRE middleware. The easiest way to implement a new sub manager is to do as follows

1. install virtualenv
2. create a virtual environment: `:::bash virtualenv --python=python3 venv`
3. activate it: `:::bash source venv/bin/activate`
4. proceed installing the softfire-sdk: `:::bash pip install softfire-sdk`
5. create the python manager

## The python manager

```python
from sdk.softfire.manager import AbstractManager
from sdk.softfire.grpc import messages_pb2
from sdk.softfire.utils import TESTBED_MAPPING

class NfvManager(AbstractManager):
    def __init__(self, config_file_path):
        super().__init__(config_file_path)

    def validate_resources(self, user_info=None, payload=None) -> None:
        """
        Validate the resources

        :param user_info:
        :param payload:
        :raise any exception for error
        """
        pass

    def refresh_resources(self, user_info):
        """
            List all available images for this tenant

            :param user_info:
            :return: the list of ResourceMetadata
             :rtype list
            """
        result = []
        ob_client = OBClient(user_info.name)
        for image in available_resources():
            testbed = image.get('testbed')
            resource_id = image.get('name')
            result.append(messages_pb2.ResourceMetadata(resource_id=resource_id,
                                                        description='',
                                                        cardinality=-1,
                                                        node_type='NodeType',
                                                        testbed=TESTBED_MAPPING.get(testbed)))
        return result

    def provide_resources(self, user_info, payload=None):
        """
            Deploy the selected resources. Payload looks like:
            {
                'properties': {
                    'nsd_name': 'my_nsd',
                    'resource_id': 'open5gcore',
                    'testbeds': {
                        'ANY':
                        'fokus'
                    }
                },
                'type': 'NfvResource'
            }

            :param payload: the resources to be deployed
             :type payload: dict
            :param user_info: the user info requesting
            :return: the nsr deployed
             :rtype: ProvideResourceResponse
            """
        resource='{}'
        return [resource]

    def create_user(self, user_info):
        """
            Create project in Open Stack and upload the new vim to Open Baton

            :param user_info:
            :return: the new user info updated
             :rtype: UserInfo

            """

        return user_info

    def list_resources(self, user_info=None, payload=None):
        """
            list all available resources

            :param payload: Not used
            :param user_info: the user info requesting, if None only the shared
              resources will be returned
            :return: list of ResourceMetadata
            """

        for k, v in get_resources().items():
            testbed = v.get('testbed')
            node_type = v.get('node_type')
            cardinality = int(v.get('cardinality'))
            description = v.get('description')
            resource_id = k
            result.append(messages_pb2.ResourceMetadata(resource_id=resource_id,
                                                        description=description,
                                                        cardinality=cardinality,
                                                        node_type=node_type,
                                                        testbed=TESTBED_MAPPING.get(testbed)))

        return result

    def release_resources(self, user_info, payload=None):
        """
           Delete the NSR from openbaton based on user_info and the nsr
           :param payload: the NSR itself
           :type payload: dict
           :param user_info:
            :type user_info: UserInfo
           :return: None
           """

    def _update_status(self) -> dict:
      """
      update the status of the experiments in case of value change

      :return: dict
      key is "username" and value is a list of str representing the resources of this user
      {
          'username1':[],
          'username2':[]
      }
      """
      return dict()

```

## Start the manager:

For starting the manager use the utility method start_manager()

```python

from sdk.softfire.main import start_manager


def start():

    start_manager(Manager('/etc/softfire/my-manager.ini'))


if __name__ == '__main__':
    start()
```

## Configuration file example

The configuration ini file can be similar to this example:

```ini
####################################
###########  Messaging #############
####################################

[messaging]
bind_port = 50053

####################################
############  system ###############
####################################

[system]
server_threads = 3
experiment_manager_ip = localhost
experiment_manager_port = 50051
name = my-manager
description = my manager
ip = localhost

####################################
############  Logging ##############
####################################

[loggers]
keys = root,main

[handlers]
keys = consoleHandler,logfile

[formatters]
keys = simpleFormatter,logfileformatter

[logger_main]
level = DEBUG
qualname = eu.softfire
handlers = consoleHandler,logfile
propagate = 0

[logger_root]
level = DEBUG
handlers = consoleHandler,logfile

[handler_consoleHandler]
class = StreamHandler
level = DEBUG
formatter = simpleFormatter
args = (sys.stdout,)

[formatter_logfileformatter]
#format=%(asctime)s %(name)-12s: %(levelname)s %(message)s
format = %(levelname)s: %(name)s:%(lineno)-20d:  %(message)s

[handler_logfile]
class = handlers.RotatingFileHandler
level = DEBUG
args = ('/var/log/softfire/experiment-manager.log', 'a', 2000, 100)
formatter = logfileformatter

[formatter_simpleFormatter]
format = %(levelname)s: %(name)s:%(lineno)-20d:  %(message)s
```


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
