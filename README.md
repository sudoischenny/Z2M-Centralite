# Z2M-Centralite
Homebridge support for Centralite Pearl or any other dual set point thermostat

Pre-Resiquites:
------
* Homebridge
* n8n.io
* z2m
* homebridge-z2m (HB - Plugin)
* homebridge-web-thermostat

Why is this an issue
------
z2m does not nativley support dual set point thermostats like Leviton RC-2000WH, Centralite 3157100. homebridge-deconz / homebridge-hue only supports setting heat not AC/Cooling. Centralite Pearl is a very decent thermostat if you need a thermostat with out a C-wire in the US, it's a 24v thermostat which many systems support.

How this works
------
Homekit sends commands to Homebridge. Homebridge web thermostat then forwards those commands to a n8n webhook (alternatively you can use node-red). n8n then recieves the commands and forwards them to the correct MQTT service/device.

n8n modules (find n8n json templates in the n8n_templates folder)
------
1. MQTT Listen
2. Status API
3. Set HVAC State
4. Set Temperature

MQTT Listen
------
On MQTT event for zigbee2mqtt/(name of thermostat) format the MQTT JSON to web-thermostat. Then save to disk.
![image](https://user-images.githubusercontent.com/46699959/193655674-e3e270e1-9856-4e69-9528-334ff7378991.png)

Status API
------
When web-thermostat requests status respond with the JSON file store on disk.
![image](https://user-images.githubusercontent.com/46699959/193656670-2a5638c1-d545-4fb5-b070-d4453cd5683f.png)

Set HVAC State
------
When web-thermostat sends "targetHeatingCoolingState" to n8n, then n8n sends a message to the MQTT endpoint.
![image](https://user-images.githubusercontent.com/46699959/193657183-812b83f4-4fee-436b-bf3f-821940c90c8e.png)

Set Temperature
------
When web-thermostat sends "targetTemperature" to n8n, then n8n sends a message to the MQTT endpoint.
![image](https://user-images.githubusercontent.com/46699959/193657323-0926e2c6-f8d7-4469-bdb8-577937ff8051.png)
