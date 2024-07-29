[![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg)](https://github.com/custom-components/hacs) [![apsystems](https://img.shields.io/github/v/release/jeremletrol81/homeassistant-apsystems.svg)](https://github.com/jeremletrol81/homeassistant-apsystems) ![Maintenance](https://img.shields.io/maintenance/yes/2024.svg)

# APsystems Sensor for Home Assistant
This component simplifies the integration of a APsystems inverter:
* creates up to individuals sensors for easy display or use in automations
* collects power (W) and energy (KWH) every 5 minutes. There is also a sensor for daily total and max power.
* extract data from apsystemsema.com web portal instead of hack the ECU connection
* supports any kind of ASsystems inverter or ECU
* if enabled, pauses from sunset to sunrise (basically when there no sun)
* have a cache system to avoid individual sensors request the same data to apsystemsema.com. It is a great feature for I/O (HTTP) performance.
* there is a date sensor to identify exactly date/time refers each sensor data

### URL's Utilised
The URL called is ``https://apsystemsema.com/ema/ajax/getReportApiAjax/getPowerOnCurrentDayAjax``
It is only called from sunset to sunrise and the sensor going offline at night

### Installation
Use [HACS](https://custom-components.github.io/hacs/) to point to this github URL:
https://github.com/jeremletrol81/homeassistant-apsystems

### Configuration
Use your apsystemsema.com to configure the configuration.yaml.

```yaml
# Minimal configuration.yaml entry:
sensor:
  - platform: apsystems
    authId: apsystemsem_authid
    systemId: apsystemsema_system_id
    ecuId: apsystemsema_ecu_id
    sunset: off
```
1 - set "Allow visitors to access to this system" and get the authid from here

2 - your systemId is found at apsystemsema.com. See the page source code and at the Settings Menu there is a code like that:
```html
<span>Settings</span>
<ul>
    <li onclick="managementClickCustomer('YOUR SYSTEM ID')"><a>Settings</a></li>
    <li onclick="intoFaq(10)"><a>Help</a></li>
</ul>
```
Get the system id inside the ```managementClickCustomer()```.

3 - There is an ecu id data at https://apsystemsema.com/ema/security/optmainmenu/intoLargeReport.action

4 - sunset attribute could be on or off

5 - Your view id is found at https://www.apsystemsema.com/ema/security/optsecondmenu/intoViewOptModule.action
See the source code and find group view
```html
<option ... value="YOUR VIEW ID" ... vn="group view" ...>group view</option>
```

6 - Your panels are found at https://www.apsystemsema.com/ema/security/optsecondmenu/intoViewOptModule.action
Right click on a panel and inspect the source code
```html
<div iii="YOUR PANEL ID" id="module4"
```

### Debug
To get debug info in the logs do
```yaml
logger:
  default: info
  logs:
    custom_components.apsystems: debug
```

and then grep the log for output

```bash
grep apsystem home-assistant.log
```
### http request test
here is the two commands to run, todo a request test if it works
### Login
GET https://www.apsystemsema.com/ema/intoDemoUser.action?id=apsystemsem_authid

### Get Data
POST https://www.apsystemsema.com/ema/ajax/getReportApiAjax/getPowerOnCurrentDayAjax

queryDate=20230615&selectedValue=apsystemsema_ecu_id&systemId=apsystemsema_system_id

### Thanx
Thanx to the author bgbraga(https://github.com/bgbraga/) and skelgaard(https://github.com/skelgaard/) for his work
