# Zigbee2MQTT various tips and tricks

## Low-level Zigbee attribute access
### Reading

MQTT topic:
> zigbee2mqtt/&lt;FriendlyDeviceName&gt;/set/read

Payload: 
> {"cluster":&lt;ID or name&gt;, "attributes":[&lt;list of attribute names|list of attribute IDs&gt;]}

Payload example: 
> {"cluster":"genPowerCfg", "attributes":["batteryPercentageRemaining"]}

***Note: it works only for 'known' clusters and attributes (for those that have converters)***

List of supported clusters with attributes, commands and type information can be found [here](https://github.com/Koenkk/zigbee-herdsman/blob/master/src/zcl/definition/cluster.ts)
##### Relevant place in code [somewhere here](https://github.com/Koenkk/zigbee-herdsman/blob/master/src/controller/model/endpoint.ts#L443) (Endpoint.read)

### Writing

MQTT topic:
> zigbee2mqtt/&lt;FriendlyDeviceName&gt;/set/write

Payload: 
> {"cluster":&lt;ID or name&gt;, "payload":{&lt;attribute name-value pairs&gt;}}

Payload example: 
> {"cluster":"hvacThermostat", "payload":{"systemMode": 0}}

##### Note: taken from [here](https://github.com/Koenkk/zigbee2mqtt/discussions/16282#discussioncomment-4946732)

##### Relevant place in code [somewhere here](https://github.com/Koenkk/zigbee-herdsman/blob/master/src/controller/model/endpoint.ts#L360) (Endpoint.write)

## Calling arbitrary known commands for known Zigbee clusters

MQTT topic:
> zigbee2mqtt/&lt;FriendlyDeviceName&gt;/set

Payload: 
> {"command":{"cluster":8,"command":5,"payload":{"movemode":1,"rate":60}}}

or, alternatively, names can be used
> {"command":{"cluster":"genLevelCtrl","command":"moveWithOnOff","payload":{"movemode":0,"rate":60}}}

List of supported clusters with attributes, commands and type information can be found [here](https://github.com/Koenkk/zigbee-herdsman/blob/master/src/zcl/definition/cluster.ts)

***Note:***

meaning and possible values of payload fields is not described there. For that ZCL (Zigbee Cluster Library) description [document](https://zigbeealliance.org/wp-content/uploads/2021/10/07-5123-08-Zigbee-Cluster-Library.pdf) should be checked.
