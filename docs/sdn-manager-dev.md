# SDN Manager Developer Details

## Overview

The SDN manager is in charge of managing access to the SDN resources provided by some testbeds.

```ascii
+-------------+   REST   +-----------+                    +--------------+
| SDN manager | +------+ | SDN proxy | +----------------> |  OpenSDNcore |
+-----------+-+          |   FOKUS   |    JSON-RPC        |   controller |
            |            +-----------+                    +--------------+
            |
            |            +-----------+                    +--------------+
            |     REST   | SDN proxy | +----------------> | OpenDayLight |
            +----------+ | Ericsson  |    RESTCONF        |   controller |
                         +-----------+                    +--------------+
```

The SDN manager keeps track of the API endpoints towards the SDN proxy services that are used to filter requests
from experimenters to enabe multi tenancy that is by default not provided by the used SDN controllers.

The communication between the SDN manager and the individual SDN proxy services is authorized by a secret that needs to be passed as a HTTP header field with every request.
The URL endpoint used for rest communication between manager and proxy is statically stored inside the configuration file of the SDN manager.

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

**Response (SM->EM)**:
TOSCA encoded object of type SDNResource for each supported SDN endpoint (e.g. for each testbed)

 * resource-id
 * testbed-id
 * description text

### Provision Resources

Involved: Experiment manager, SDN Manager, SDN Proxy

**Request (EM->SM)**:
TOSCA encoded object with a list SDNResource types for each requested SDN resource. (Each testbed has its own resource.)

 * resource-id(s)
 * UserInformation
   * username
   * password (can be used to create an account with the users password)
   * tenant-id for each used testbed
   * experiment-id (token used for this experiment)

**Request (SM->proxie(s))**:
REST request to resource /SDNroxySetup with JSON object in request-body:

 * token (experiment-id)
 * tenant-id (for the associated testbed)

```properties
method: POST
path: /SDNproxySetup
Header: Auth-Secret: <xx>
Body: JSON
```

 ```json
  {
      "experiment_id": "a5cfaf1e81f35fde41bef54e35772f2b",
      "jsonrpc": "2.0",
      "result": {
          "msg_id": "0x01020304"
      }
  }
 ```

**Response (proxie->SM)**:
REST response body JSON encoded:

```json
  {
  	"endpoint_url": "http:/foo.bar",
  	"user-flow-tables": [10,11,12,13,14,15]
  }
```

**Response (SM->EM)**:
List of resource objects

 * resource-id
 * URI
 * flow-table-range
 * token?


### Release Resources
Involved: Experiment manager, SDN Manager, SDN Proxy

**Request (EM->SM)**:
List or resource-id(s) to release

 * respurce-id
 * token

**Request (SM->proxie(s))**:
REST request to resouce /SDNproxyRemove witgh JSON body

```properties
method: DELETE
path: /SDNproxy/<token>
Header: Auth-Secret: <xx>

Response: HTTP 200
Body: none
```
