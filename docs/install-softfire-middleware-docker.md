# How to install the SoftFIRE middleware using docker compose

## Prerequisites

You need to install:

* [docker](https://docs.docker.com/engine/installation/#cloud) that includes also docker-compose
* git

For having a real example (fully reproduce the SoftFIRE Middleware), you will also need:

* an OpenStack instance where executing deployment

## Deployment

This deployment will be composed by these containers:

* Experiment Manager
* Nfv Manager
* Sdn Manager
* Security Manager
* Monitoring Manager
* Physical Device Manager
* Open Baton Standalone

The **Nfv Manager and Monitoring containers must be able to reach the OpenStack endpoints**.

## Get the docker compose folder

Just clone the repository containing the docker compose file and the configurations:

```sh
git clone https://github.com/softfire-eu/docker-softfire.git
```

after this command you can go in the folder and check that everything is there:

```sh
cd docker-softfire
```
and you should have something like this:

```sh
.
├── LICENSE
├── docker-compose.yaml
├── ex-man
│   ├── Dockerfile
│   └── softfire
│       ├── experiment-manager.ini
│       ├── softfire-ca.p12
│       ├── softfire-key
│       ├── softfire-key.pem.pub
│       ├── template_openvpn.tpl
│       ├── users
│       └── views
├── mon-man
│   ├── Dockerfile
│   └── softfire
│       ├── monitoring-manager.ini
│       └── openstack-credentials.json
├── nfv-man
│   ├── Dockerfile
│   └── softfire
│       ├── available-nsds.json
│       ├── nfv-manager.ini
│       ├── openstack-credentials.json
│       ├── packages
│       ├── softfire-key
│       ├── softfire-key.pem.pub
│       └── start.sh
├── pd-man
│   ├── Dockerfile
│   └── softfire
│       ├── physical-device-manager.ini
│       └── physical-resources.json
├── sdn-man
│   ├── Dockerfile
│   └── softfire
│       ├── sdn-manager.ini
│       └── sdn-resources.json
└── sec-man
    ├── Dockerfile
    └── softfire
        ├── security-manager
        └── security-manager.ini

16 directories, 26 files
```

Before running docker compose we need to correctly configure the Middleware.

## Configuration

Each manager has its own configuration. Some are very simple some are more complex. we will have a look into all of them.

### Nfv Manager configuration

let's go in the _nfv-man_ folder

```sh
cd nfv-man
```

we need to configure the nfv manager itself by changing the nfv-manager.ini file inside the softfire dir.
```sh
vim softfire/nfv-manager.ini
```

here you have to do some modifications:

the Open Baton endpoint must be correctly configured

```ini
...
[nfvo]
ip = openbaton
username = admin
password = openbaton
port = 8080
https = False
...
```

Then we need to set the OpenStack endpoint

```sh
vim softfire/openstack-credentials.json
```
modify the file in order to match your openstack endpoint.

!!! Note
    At the moment only v3 is supported

```json
{
  "fokus": {
    "username": "admin",
    "password": "password",
    "auth_url": "http://openstack:5000/v3",
    "ext_net_name": "whatever",
    "admin_tenant_name": "admin",
    "allocate-fip": 0,
    "api_version": 3,
    "admin_project_id": "ea45bf4462864832a75ece4c4cc33c11",
    "user_domain_name": "default"
  }
}
```

!!! Note
    please let as key _fokus_ since it is needed to be one of the SoftFIRE testbed names.

And we can come back the root folder

```sh
cd ../
```

### Monitoring Manager configuration

As per the Nfv Manager, we need to configure the Monitoring Manager, so just copy the file of the nfv manager

```sh
cp nfv-man/softfire/openstack-credentials.json mon-man/softfire/openstack-credentials.json
```

Then you also have to configure some options in the ini file:

```sh
vim mon-man/softfire/monitoring-manager.ini
```

in particular the openstack related configurations:

```ini
[openstack-params]
image_name=Zabbix_Server_image
flavour=m1.small
security_group=default
instance_name=Zabbix_Server_Instance
```

### Security Manager

As anticipated before, also the Security Manager needs some modifications:

```sh
vim sec-man/softfire/security-manager.ini
```

and modify the Open Baton related properties:
```ini
[open-baton]
ip = openbaton
port = 8080
https = False
version = 1
username = admin
password = openbaton
```

!!! Note
    The Log collector will not work for now. We are working in order to provide a solution in order to complete the security tutorial.

### Sdn Manager

Coming soon!

## Deploy!

Now it is time to deploy:

```sh
docker-compose up --build -d --remove-orphans
```

The Experiment Manager is available at http://localhost:5180. You can access the admin portal by using admin/admin.

The next step is to create an experimenter. By creating a user, a long chain of calls will be performed. In particular the Nfv Manager will create a user in OpenStack and then upload the right vim to Open Baton.

If it goes well, then you are able to logout and then log in with the create username and password and you should be able to see all the available resources.

From now on you can continue with the tutorial in the tutorial section
