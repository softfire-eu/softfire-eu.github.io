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

class Manager(AbstractManager):
  def refresh_resources(self, user_info):
        """
            List all available images for this tenant

            :param tenant_name: the tenant name
             :type tenant_name: str
            :return: the list of ResourceMetadata
             :rtype list
            """
        result = []

        result.append(messages_pb2.ResourceMetadata(resource_id=resource_id,
                                                        description='',
                                                        cardinality=-1,
                                                        node_type='NodeType',
                                                        testbed=messages_pb2.ANY)
        return result

    def provide_resources(self, user_info, payload=None):
        """
            Deploy the selected resources

            :param payload: the resources to be deployed
             :type payload: dict
            :param user_info: the user info requesting
            :return: the nsr deployed
             :rtype: ProvideResourceResponse
            """
        return messages_pb2.ProvideResourceResponse(resources="content")

    def create_user(self, username, password):
        """
            Create project in Open Stack and upload the new vim to Open Baton

            :param name: the username of the user, used here also as tenant name
             :type name: string
            :param password: the password of the user
             :type password: string
            :return: the new user info updated
             :rtype: UserInfo

            """

        user_info = messages_pb2.UserInfo(
            name=username,
            password=password,
            ob_project_id='id',
            testbed_tenants={}
        )

        return user_info

    def list_resources(self, user_info=None, payload=None):
        """
            list all available resources

            :param payload: Not used
            :param user_info: the user info requesting, if None only the shared resources will be returned
            :return: list of ResourceMetadata
            """
        result = []

        result.append(messages_pb2.ResourceMetadata(resource_id=resource_id,
                                                        description=description,
                                                        cardinality=cardinality,
                                                        node_type=node_type,
                                                        testbed=messages_pb2.ANY)

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
```

## Start the manager:

For starting the manager use the utility method start_manager()

```python

from sdk.softfire.main import start_manager


def start():

    start_manager(Manager(), '/etc/softfire/my-manager.ini')


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
