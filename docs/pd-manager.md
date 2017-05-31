# Physical Device Manager

The Physical Device Manager only provides (for the second wave) reservation of physical resources. For that reason the only thing to be spacified is the **resource_id**, that can be checked in the resource discovery phase. Please read carefully the description for avoiding misunderstanding. Anyway a definition of the NodeType follows:


```yaml
PhysicalResource:
  derived_from: eu.softfire.BaseResource
  description: "Defines a Physical resource request in the SoftFIRE Middleware"
```
