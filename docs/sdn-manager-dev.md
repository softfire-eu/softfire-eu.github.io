# SDN Manager Developer Details

The SDN manager is in charge of managing access to the SDN resources provided by some testbeds.

The SDN manager keeps track of the API endpoints towards the SDN proxy services that are used to filter requests
from experimenters to enabe multi tenancy that is by default not provided by the used SDN controllers.

The SDN manager uses the following Experiment LifeCycles:

 * List
 * Provision
 * Release

## Message contents

There are three parties involved into the communication:

 * Experimet Manager (EM)
 * SDN Manager (SM)
 * SDN Proxie(s)

### List Resources

Involved: Experiment manager, SDN Manager

Response (SM->EM):
TOSCA encoded object of type SDNResource for each supported SDN endpoint (e.g. for each testbed)

 * resource-id
 * testbed-id
 * description text

### Provision Resources

Involved: Experiment manager, SDN Manager, SDN Proxy

Request (EM->SM):
TOSCA encoded object with a list SDNResource types for each requested SDN resource. (Each testbed has its own resource.)

 * resource-id(s)
 * UserInformation
   * username
   * password (can be used to create an account with the users password)
   * tenant-id for each used testbed
   * experiment-id (token used for this experiment)

Request (SM->proxie(s)):
REST request to resource /SDNroxySetup with JSON object in request-body:

 * token (experiment-id)
 * tenant-id (for the associated testbed)

Response (proxie->SM):
REST response body JSON encoded:

 * endpoint URL
 * flow-table-range (list of decimals)

Response (SM->EM):
List of resource objects

 * resource-id
 * URI
 * flow-table-range
 * token?


### Release Resources
Involved: Experiment manager, SDN Manager, SDN Proxy

Request (EM->SM):
List or resource-id(s) to release

 * respurce-id
 * token

Request (SM->proxie(s)):
REST request to resouce /SDNproxyRemove witgh JSON body

 * token
