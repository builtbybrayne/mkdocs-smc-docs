# Platform Terminology

These terms are used within the SMC Platform, and drive the design of the API for integration with onsite hardware.

## The Platform and Plugins

The SMC `Platform` provides the infrastructure and generic functionality that allows us to interact with both the onsite hardware and the operator who needs to view data and define policy.

It is a suite of software composed of microservices, databases, message routers, configuration, UIs etc. The aim is for the platform to be agnostic to the semantics of any given installation.

`Plugins` on the other hand provide the specific functionality associated with particular operations. They therefore require a semantic understanding of the domain in question - or at least the aspects of the domain pertinent to their functionality. E.g. operating a technical balance of energy producing devices requires a knowledge of power, units of power, the difference between power generators and consumers etc.

*NB: At the moment, the platform at Faccombe (FEL) is not actually built with a plugin architecture at a technical level, but this is a design goal.*


## Devices and Signals

`Devices` are physical hardware entities such as sensors, meters, actuators etc. They may have an inbuilt software platform that allows us to interact with them, or they may require us to build some sort of low-level hardware integration. 

`Devices` may constitute only a single physical device, such as a power meter. Or they may in reality be a collection of many physical devices. A wind turbine for example is actually a collection of multiple sensors as well as some control interfaces. 

The actual data created by devices is decomposed into a 2-tier hierarchy within the `platform`; `signals` and `attributes`. `Attributes` are the most primitive fields, usually numeric, boolean, strings etc. `Signals` are logical groupings of related `attributes` and these almost always - although not necessarily - correspond to the actual sensors and meters that make up the `device`. 

For example, a meter might emit messages at a lower frequency than its sampling rate, with numeric readings aggregated into multiple `attributes` including mean, std deviation, max and min. All these `attributes` are transmitted together and constitute a single `signal`. 

`Signals` also help us decompose complex `devices` into generic collections of data readings for more generalised processing within the `platform`. For example, both wind turbines and pv arrays have sensors which register their power output. A general contract for power meters can be registered with the `platform` in the form of a schema that defines the expected `attributes` of a power meter. The integrations to both devices can reuse this contract to format data as a known `signal`. Other components of the `platform` can also understand the power meter contract, and consume the data in the `signal`.

The API exposed to external integrations is in large part this set of schemas that define the known `signal` contracts. These schemas are registered with the `platform` against unique, predefined keys and form the semantic dictionary of the installation.

## Agents and Messages

`Agents` are the software clients which provide the edge integration with the `platform`. They interface with the software/hardware of the `device` to collect the `signals` and `attributes` and then send valid `messages` to the `platform`.

In almost all cases, and certainly all early cases, the `agent` will need to be an integration built by us for the `device` or type of `device`, with an embedded client library for SMC. In best case scenarios the `devices` will have standardised interfaces which allows us to create small reusable `agents`. In more complicated or unique setups a custom `agent` will have to be written just for that `device`. 

Where `devices` have internet accessible APIS, `agents` can be run on platform hardware. But where `devices` require a physical integration it is most likely that `agents` will be colocated with the `device` itself.

`Messages` are simply json documents which include a certain amount of meta-information (such as time sent) and a collection of `signals`.

The messaging transport approach has until now been simply http, but we will be using mqtt from here on in.

### Signal Messages vs Control Messages

At the moment control `messages` are sent by the `platform` using exactly the same terminology as `signals`. They could be considered as `signals` *from* the `platform` to `agents`. However, this may be a bad design and could do with some discussion.


## Configuration and Properties

Much like `messages` are composed of `signals` that adhere to predefined contracts, the configuration for `devices` and `agents` is similarly split into composable contracts called `properties`. 

Often, where a `device` or `agent` adheres to a `signal's` contract it can or must also then provide some additional configuration details. This is particularly relevant for `device` configuration, where a generic `agent` (e.g. a generic power meter integration) needs to know details about the readings generated by the device so that it compose a `message` with the correct `attributes` for each `signal`.  

In the example of the generic power meter `agent`, the `device` may emit just a single numeric reading without specifying units. The `agent` needs to know what units the power meter is measuring in and whether positive values imply production or consumption. 

When the `agent` and `device` are registered with the `platform` these configuration details can be provided as `properties` within the json configuration. These are seperate from the platform-specific configuration fields (such as id, heartbeat etc) which are also supplied on registration.

`Properties` are themselves registered with the `platform` as schemas against a unique predefined key, just like `signals`.

Where `properties` relate to specific `signals` the keys should be the same. 