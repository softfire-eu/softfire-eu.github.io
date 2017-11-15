# Physical Device Manager

The Physical Device Manager provides reservation of physical resources. For the third wave, experimenters may reserve UE(s) at the Surrey test bed, and remotely control them using [TeamViewer](https://www.teamviewer.com).

The PhysicalResource node type is defined as follows, as per [node types page][node_types]:

```yaml
PhysicalResource:
    derived_from: eu.softfire.BaseResource
    description: "Defines a Physical resource request in the SoftFIRE Middleware"
```

As such, the only parameter that needs to be specified is the **resource_id** which is derived from the BaseResource type. 

## UE Physical Resource
### UE Reservation
In the case of a UE reservation, the resource_id is **surrey-ue**. Therefore, reserving a UE can be done by including the following under *node_templates* in the experiment.yaml file for each UE to reserve:

```yaml 
<resource-name>:
	properties:
		resource_id: surrey-ue
	type: PhysicalResource
```

where <***resource-name***> is a unique name to be assigned to the UE.

The following diagram shows the procedure for reserving and terminating UE resources:

<center>![UE reservation reservation and termination procedures](img/ue-reservation-engine.svg)</center>

### UE Control
After deploying an experiment with a UE physical resource, the TeamViewer login credentials will appear under the properties of your experiment, as shown in the following example:

[![UE Reservation - Experiment Manager](img/ue-reservation-em.png)](img/ue-reservation-em.png)

Using these credentials, the experimenter can log in to TeamViewer and view the list of reserved UEs that can be controlled, as shown in the example below. These will be displayed under the group name "SoftFIRE UE Group X"; (X=1...3).

[![UE Reservation - TeamViewer](img/ue-reservation-tv.png)](img/ue-reservation-tv.png)

To connect to a UE, click the "Connect" button. If TeamViewer is installed on your system, the connection to the UE will be displayed using the installed client. Otherwise, it will be displayed using a brower-based TeamViewer client. Once connected, the UE can be controlled by the mouse as if physically in front of the device.