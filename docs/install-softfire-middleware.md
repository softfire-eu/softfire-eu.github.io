# How to install the SoftFIRE middleware

The SoftFIRE Middleware already provides a set of bash functions that will help you in case you want to install your _private_ SoftFIRE Middleware. There are two options:

* _**codeinstall**_: install the code of all the managers and the python package of the SDK. This procedure is meant to be for development purposes
* _**install**_: install the code of all the managers and the python package of the SDK. This procedure is meant to be for production purposes

!!! note
      The SoftFIRE Middleware is OS independent, however the bootstrap procedure assumes that the underlying OS is Debian based.

In case you want just to play around with the Experiment Manger, you can use the docker installation

## Prerequisites

Both procedures needs to have git installed

```bash
$-> sudo apt install git
```

and to run:

```bash
$-> git clone https://github.com/softfire-eu/bootstrap.git
```

for instance in your home directory. After the clone, you should have a folder called bootstrap containing:

```sh
bootstrap
├── LICENSE
├── README.md
├── bootstrap.sh
└── generate_cork_files.py
```

Then cd into the directory and run the bootstrap commands:


```text
$-> cd bootstrap
$-> ./bootstrap.sh

./bootstrap.sh <action>

actions:    [install|update|clean|start|stop|codestart|codeupdate|codeinstall|purge]

install:      install the SoftFIRE Middleware python packages
update:       update the SoftFIRE Middleware python packages
clean:        clean the SoftFIRE Middleware
start:        start the SoftFIRE Middleware via python packages
stop:         stop the SoftFIRE Middleware
codeinstall:  install the SoftFIRE Middleware source code
codeupdate:   update the SoftFIRE Middleware source code
codestart:    start the SoftFIRE Middleware via source code
purge:        completely remove the SoftFIRE Middleware
```

## Install the source code

For installing the source code just run:

```sh
$-> ./bootstrap.sh codeinstall
```

## Install the python packages

For installing the python packages just run:

```sh
$-> ./bootstrap.sh install

```

## What's happening?

After running these commands the script will:

1. install the debian packages required
1. creating the configuration folders
1. downloading the source code or installing python packages of all the managers (depending on what installation procedure you chose)
1. downloading configuration files

## Configuration

Each manager has its own configuration. Some are very simple some are more complex. You can find each manager configuration under the `etc` folder. You can tune them as you like. Most of the configuration files can be updated without stopping the middleware.
First of all, change the `etc/openstack-credentials.json` file.

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
    please let key as _fokus_ since it is needed to be one of the SoftFIRE testbed names.

## Start the Middleware

If everything went well, you are able to start the SoftFIRE Middleware by running

```sh
$-> ./bootstrap.sh codestart
```

in case you installed via source code, or

```sh
$-> ./bootstrap.sh start
```

in case you installed via python packages.

In both cases, a tmux session will run in background and you can check the output by attaching to it:

```sh
tmux a
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
