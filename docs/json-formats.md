# Configuration and Message Formats

This is a manually generated doc with examples. It is not canonical. See [https://bitbucket.org/smc-dev/smc-validators]() for actual schemas.

*I intend to auto-generate docs for schemas fairly soon*


## Device Config

```js
{
  "type": "deviceConfig", // must be "deviceConfig',
  "version": "2.0.0", // Must be 2.0.0
  "deviceId": "<DEVICE ID>", // some unique string. TBD
  "ns": "<NAMESPACE>", // "fel" for faccombe. Other sites TBD
  "name": "<DEVICE NAME>", // human-friendly name for the device
  "description": "<DESCRIPTION>", // additional details describing the device
  "heartbeat": {
    "frequency": 0, // in ms. 0 implies no heartbeat. Default is 0
    "timeout": 0 // in ms. 0 implies no timeout. Default is 0
  },
  "properties": {
    // optional properties dictionary
  }
}
``` 

## Agent Config

```js
{
  "type": "agentConfig", // must be "agentConfig',
  "version": "2.0.0", // Must be 2.0.0
  "agentId": "<AGENT ID>", // some unique string. TBD
  "enabled": "yes", // "yes" or "no. Default is "yes". (Messages from disabled agents are ignored)
  "description": "<DESCRIPTION>", // additional details describing the agent
  "deviceId": "<DEVICE ID>", // the device this agent is an integration for
  "ns": "<NAMESPACE>", // "fel" for faccombe. Other sites TBD
  "heartbeat": {
    "frequency": 0, // in ms. 0 implies no heartbeat. Default is 0
    "timeout": 0, // in ms. 0 implies no timeout. Default is 0
  },
  "properties": {
    // optional properties dictionary
  }
}
```

## Messages

```js
{
  "type": "message", // must be message
  "version": "2.0.0", // must be 2.0.0
  "time": "<ISOFORMAT DATE STRING>", // e.g. 2017-05-18T14:08:49.291Z
  "agentId": "<AGENT ID>", // e.g. manor_sensor
  "agentStatus": "on", // 'on' or 'off'
  "deviceStatus": "on", // 'on', 'off' or 'unknown'
  "ns": "<NAMESPACE>", // "fel" for Faccombe. Other sites tbd
  "description": "<HUMAN FRIENDLY MESSAGE>",
  "signals": {
    // Optional signals from device
  }
}
```

