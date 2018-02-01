# How to install the SoftFIRE middleware using docker compose

## Prerequisites

You need to install:

* [docker](https://docs.docker.com/engine/installation/#cloud) and also docker-compose
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
* Open Baton NFVO
* Open Baton GVNFM
* Open Baton OpenStack plugin

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
├── README.md
├── dbdata_nfvo
├── dbdata_rabbit
├── softfire-middleware.yaml
├── etc
│   ├── available-nsds.json
│   ├── experiment-manager.ini
│   ├── mapping-managers.json
│   ├── nfv-manager.ini
│   ├── openstack-credentials.json
│   ├── physical-device-manager.ini
│   ├── physical-resources.json
│   ├── sdn-manager.ini
│   ├── sdn-resources.json
│   ├── security-manager.ini
│   └── softfire_node_types.yaml
└── views
    ├── LICENSE
    ├── README.md
    ├── admin_page.tpl
    ├── calendar.tpl
    ├── experimenter.tpl
    ├── login_form.tpl
    ├── openvpn.tpl
    ├── password_change_form.tpl
    └── static
```

Before running docker compose we need to correctly configure the Middleware.

## Configuration

Each manager has its own configuration. Some are very simple some are more complex. You can find each manager configuration under the `etc` folder. You can tune them as you like. Most of the configuration files can be updated without stopping the middleware.
First of all change the `etc/openstack-credentials.json` file.

```sh
vim etc/openstack-credentials.json
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

### Sdn Manager

Coming soon!

## Deploy!

Now it is time to deploy:

```sh
HOST_IP=<youriphere> docker-compose -f softfire-middleware.yaml up -d
```

!!! Note
    <yourip> stands for the ip where the rabbitmq container runs, the VMs will contact this ip when registering to Open Baton.

The Experiment Manager is available at http://localhost:5080. You can access the admin portal by using admin/admin.

Please bear in mind that each container takes different time to be up and running, the Experiment Manager usually is up before the NFVO. Please wait until you can reach localhost:8080.

The next step is to create an experimenter. By creating a user, a long chain of calls will be performed. In particular the Nfv Manager will create a user in OpenStack and then upload the right vim to Open Baton.

If it goes well, then you are able to logout and then log in with the create username and password and you should be able to see all the available resources.
