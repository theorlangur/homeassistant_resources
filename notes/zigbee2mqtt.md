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

## Moving Hue for Led Controller (TS0503B, _TZ3000_i8l0nqdu)

MQTT topic:
> zigbee2mqtt/&lt;FriendlyDeviceName&gt;/set

Payload (move Up): 
> {"command":{"cluster":"lightingColorCtrl","command":"moveHue","payload":{"movemode":1,"rate":10}}}

Payload (move Down): 
> {"command":{"cluster":"lightingColorCtrl","command":"moveHue","payload":{"movemode":3,"rate":10}}}

Payload (move Stop): 
> {"command":{"cluster":"lightingColorCtrl","command":"moveHue","payload":{"movemode":0,"rate":10}}}

***Note 1:***

when stopping for some reason 'rate' had to be non-zero (at least for this led controller). Movemove:0, rate:0 - no reaction

***Note 2:***

rate according to ZCL (5.2.2.3.5.3 ) description is a 'change in the device’s hue of one unit' whatever that means.
