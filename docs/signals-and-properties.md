# Currently Supported Signals and Properties

This is a manually generated doc with examples. It is not canonical. See [https://bitbucket.org/smc-dev/smc-validators](https://bitbucket.org/smc-dev/smc-validators) for actual schemas.

*I intend to auto-generate docs for schemas fairly soon (Al)*

*Many of these are quite 'organic' and very much up for improvement given further domain knowledge.*

## Power Generator

Defines a generic power generator contract

### Property

```js
{
  "version": "2.0.0", // Optional. Defers to containing document version
  "units": "kW", // MW, kW or W. Default is kW
}
```

### Signal

```js
{
  "version": "2.0.0", // Optional. Defers to containing document version
  "power": 300 // Int or float
}
```

## Power Meter

Defines a generic power meter contract

### Property

```js
{
  "version": "2.0.0", // Optional. Defers to containing document version
  "units": "kW" // MW, kW or W. Default is kW
  "positiveIsProduction": true // Butoolean. Default is true
}
```

### Signal

```js
{
  "version": "2.0.0", // Optional. Defers to containing document version
  "activePower": 300, // Int or float. Units as per property
  "reactivePower": 300, // Int or float. Optional
  "apparentPower": 300, // Int or float. Optional
  "voltageAverage": 240, // Int of float. Optional
}
```

## Windspeed Meter

Defines a generic windspeed meter contract

### Property

None

### Signal

```js
{
  "version": "2.0.0", // Optional. Defers to containing document version
  "windspeed": 4 // Int or float. In m/s
}
```

## Setlevel

Defines the setlevel contract for a controllable power generator

*NB: This is currently a signal **from** the platform to the agent. This may be better expressed as a control message and be placed elsewhere*

### Property

```js
{
  "version": "2.0.0", // Optional. Defers to containing document version
  "min": 300, // Int. In kW
  "max": 500 // Int. Must be > min if min is set. In kW
}
```

### Signal

```js
{
  "version": "2.0.0", // Optional. Defers to containing document version
  "setlevel": 300 // Int. In kW
}
```

## Export Channel

Defines the contract for power export from an electricity network (e.g. a grid connection) 

### Property

```js
{
  "version": "2.0.0", // Optional. Defers to containing document version
  "limit": 300, // Int. In kW. Must be >= 0. 0 implies no limit
}
```

### Signal

None


## Network

Defines the contract for specifying software network details for a device

### Property

```js
{
  "version": "2.0.0", // Optional. Defers to containing document version
  "ip": "1.2.3.4", // Must be an ip. Required if "dns" is missing
  "dns": "hostname.smc" // Must be a dns resolvable hostname or entry. Required if "ip" is missing
}
```

### Signal

None

