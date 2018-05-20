
# Hackaton IoT 2018 - Device

## General Rules:
- never connect directly positive and negative power wires
- never change connections when the circuit is powered
- beware not to short-circuit accidentally pins or other parts on any component
- before turning power on make sure all connections are correct and there is no short circuits
- never connect 5V output directly into a component working on 3.3V logic level

## Requirements:
- Linux / OSX
- Python 3.6
- CP210x USB to UART (OSX only)
- ampy
- Mosquitto-clients
- esptool.py
- picocom

## Installation:
### CP210x USB to UART:
OSX: https://www.silabs.com/documents/public/software/Mac_OSX_VCP_Driver.zip

### Ampy:
https://github.com/adafruit/ampy

Linux / OSX:

```bash
pip install adafruit-ampy
```

### Mosquitto-clients:
https://mosquitto.org

Linux:

```bash
sudo apt-get install mosquitto-clients
```

OSX:

```bash
brew install mosquitto
```

### Esptool.py:
https://github.com/espressif/esptool

Linux / OSX:

```bash
pip install esptool
```

### Picocom:
https://github.com/npat-efault/picocom

Linux:

```bash
sudo apt-get install picocom
```

OSX:

```bash
brew install picocom
```

## Parts
### ESP32-DevKitC ESP32 WiFi + BT 4.2

**Description:** A MicroPython-compatible microcontroller.
CPU 32-bit 160 Mhz, 520 KB SRAM
Integrated WiFi and BT module.
Uses 3.3V logic level.
Can be powered with 5V power supply

**Purpose:** Use it in the final device to connect all components.

### ESP8266 NodeMCU v2 microcontroller

**Description:** A MicroPython-compatible microcontroller.
Integrated WiFi module
Uses 3.3V logic level.

**Purpose:** Use it for development with a single component.

### PMS7003 Air quality sensor

**Description:** Air quality sensor capable of measuring particle pollution in ranges: PM1.0, PM2.5, PM10.

### Adapter IDC 10pin 1,27mm - JST 1,25mm

**Description:**
Adapter for PMS7003 air quality sensor

### OLED display 0,96'' (B) 128x64px I2C

**Description:** Uses 3.3V or 5V logic level.
Support 3/4-wire SPI and communication protocols.

**Purpose:** Use it to display information gathered by the device.

### DHT11 temperature / humidity sensor

**Description:** Collects information about temperature and relative humidity.
Temperature is measured in range from 0°C to 50°C and 2°C precision.
Humidity is measured in range 20% to 90% and ± 5%

**Purpose:** Use it collect information about current temperature and humidity.

### LED Ring RGB WS2812B 5050 x 8

**Description:** LED Ring composed of 8 addressable RGB LED.
Powered by 5V, consumes 18mA per LED, with a total power consumption of 150mA.

**Purpose:** Can be used to provide visual feedback from the device and represent gathered data.

### Sparkfun 2-way, 4-channel logic converter

**Description:** Logic level converter, allows for safe communication of 3.3V and 5V level components.
Requires 3.3V and 5V power input.
Converts 3.3V into 5V and 5V into 3.3V.

**Purpose:** Used to ensure 5V component ouputs do not damage 3V components and to ensure 3V component outputs are correctly read by 5V components.

### LM2596 3,2V-35V 3A step-down power converter

**Description:** Step-down converter. Converts input voltage in range 3.2V - 40V to voltage in range 3.2V - 35V (set with potentiometer)
Maximum power draw of 3A.

**Purpose:** Use it to convert 5V provided by USB Power Supply to 3.3V.

### microUSB socket

**Purpose:** Use it with the LED Ring to provide power from USB Power Supply.

### Breadboard 830 tie-points
### microUSB 5V / 2,5A power supply
### microUSB B - A cable
### Connection wires 20cm

## Components
Below listed are components that will constitute the final device. Each component can be worked on separately using an ESP8266 NodeMCU v2 microcontroller.

### PMS7003 Air quality sensor

> **Difficulty:** High

**Task:** Wire up the parts and write a MicroPython module, which will collect, parse and return sensor data. Writing a driver for the sensor took us quite some time to implement, so if you want you can use the [driver](https://github.com/pkucmus/micropython-pms7003) we already have.

> **Caveat**  
> The data sheet for this sensor will tell you to run it from 5V, this was not the case - for whatever reason it has to be ran from 3.3V to have proper readings.

**Parts:**
- ESP32-DevKitC ESP32 WiFi + BT 4.2
- PMS7003 Air quality sensor
- Adapter IDC 10pin 1,27mm - JST 1,25mm
- microUSB B - A cable
- Breadboard

**Boilerplate:**

```python
from pms7003 import PMS7003

pms = PMS7003()
pms_data = pms.read()
```

**Reference:**
- https://botland.com.pl/index.php?controller=attachment&id_attachment=2182 (Appendix 1)

### LED Ring RGB WS2812B 5050 x 8
**Difficulty:** Medium

**Task:** Wire up the parts and write a MicroPython module, which will provide a visual feedback given the data collected by sensors and the state of the device. You have freedom in choosing how the ring will represent the data and how it will interact with other components.

**Parts:**
- ESP8266 NodeMCU v2 microcontroller
- LED Ring RGB WS2812B 5050 x 8
- microUSB 5V / 2,5A power supply
- microUSB B - A cable
- microUSB socket
- Sparkfun 2-way, 4-channel logic converter 
- Breadboard

**Boilerplate:**

```python
import machine, neopixel
np = neopixel.NeoPixel(machine.Pin(4), 8)
np[0] = (255, 0, 0)
np.write()
```

**Reference:**
- https://docs.micropython.org/en/v1.8/esp8266/esp8266/tutorial/neopixel.html

### OLED display 0,96'' (B) 128x64px SPI/I2C
**Difficulty:** Medium

**Task:** Wire up the parts and write a python module, which will communicate with OLED display. The display can render data gathered from sensors and the state of the device. You have freedom in choosing what information will be rendered on display and how it will be rendered.

**Parts:**
- ESP8266 NodeMCU v2 microcontroller
- OLED display 0,96'' (B) 128x64px SPI/I2C
- microUSB B - A cable
- breadboard

**Boilerplate:**

```python
import sh1106
from machine import I2C, Pin

i2c = I2C(-1, Pin(5), Pin(4))
display = sh1106.SH1106_I2C(128, 64, i2c, Pin(14))
display.fill(1)
display.show()
display.fill(0)
display.show()
```

**Reference:**
- [Driver](https://github.com/robert-hh/SH1106/blob/master/sh1106.py)

### Temperature and Humidity Sensor
**Difficulty:** Low

**Task:** Wire up the parts and write a python module which will collect, parse and return data from temperature and humidity sensor

**Parts:**
- ESP8266 NodeMCU v2 microcontroller
- DHT11 temperature / humidity sensor
- microUSB B - A cable

**Boilerplate:**

```python
import dht
import machine

d = dht.DHT11(machine.Pin(4))
d.measure()
d.temperature()
d.humidity()
```

**Reference:**
https://docs.micropython.org/en/latest/esp8266/esp8266/tutorial/dht.html

### MQTT - api integration
**Difficulty:** Medium

**Task:** Write a python module which will communicate with MQTT server and send data collected from the sensors.

**Parts:**
- ESP8266 NodeMCU v2 microcontroller
- microUSB B - A cable

**Boilerplate:**

```python
from umqtt.simple import MQTTClient

client = MQTTClient("<device_id>", todo)
client.connect()
```

**Reference:**
- https://github.com/micropython/micropython-lib/tree/master/umqtt.simple
- [MQTT message schema](MQTT_SCHEMA.yaml)
