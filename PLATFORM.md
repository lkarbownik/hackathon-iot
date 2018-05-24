# Hackaton IoT 2018 - Platform

## Requirements:
- Linux / OSX
- Mosquitto-clients

## Installation:
### Mosquitto-clients:
[mosquitto](https://mosquitto.org)  
Linux:

```bash
sudo apt-get install mosquitto-clients
```
OSX:

```bash
brew install mosquitto
```

## Guidelines
- platform must implement the following [schema](MQTT_SCHEMA.yaml) - it can be extended with additional fields
- backend can be written using any technology or framework
- frontend can be (but does not have to be) a separate application
- platform should collect, parse and display data from MQTT server
- platform should support displaying data from multiple devices and each device should be identifiable by it's location (city or more specific coordinates)

## Using prototype data 
Until your team finishes it's own device you can use data provided by the prototype.  
For the duration of the event the prototype will be sending it's data to the MQTT server.  
The data will be provided under topic: `/stx/office/pila/air_quality/0`

You can inspect the data using the following command:

```bash
mosquitto_sub -h 10.93.1.22 -t '/stx/office/pila/air_quality/0'
```

