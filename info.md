Home Assistant Custom Component for Bosch Nefit Easy thermostat

## Configuration

```
nefiteasy:
  devices:
  - serial: 'XXXXXXXXX'
    accesskey: 'xxxxxxxxx'
    password: 'xxxxxxxxx'
    # name: Nefit Easy
    # min_temp: 15
    # max_temp: 27
    # temp_step: 0.5
    # sensors:
    #  - list of sensors to be exposed
    # switches:
    #  - list of switches to be exposed
```

If any of your secrets in the configuration is numbers only, make sure to put it between quotes (`'`) to have homeassistant parse them correctly.

## Examples
### Basic
```
nefiteasy:
  devices:
  - serial: '01234567'
    accesskey: !secret nefitaccesskey
    password: !secret nefitpassword
```

### Limit sensors and switches
```
nefiteasy:
  devices:
  - serial: '01234567'
    accesskey: !secret nefitaccesskey
    password: !secret nefitpassword
    sensors:
      - status
      - supply_temperature
      - hot_water_operation
    switches:
      - hot_water
      - holiday_mode
      - preheating
```

### Multiple devices
```
nefiteasy:
  devices:
  - serial: '01234567'
    accesskey: !secret nefitaccesskey
    password: !secret nefitpassword
    name: Nefit 1
  - serial: '76543210'
    accesskey: !secret nefitaccesskey2
    password: !secret nefitpassword2
    name: Nefit 2
```

## Available sensors & switches
### Sensors
```
- year_total
- status
- supply_temperature
- outdoor_temperature
- system_pressure
- active_program
- hot_water_operation
```

### Switches
```
- hot_water
- holiday_mode
- fireplace_mode
- today_as_sunday
- tomorrow_as_sunday
- preheating
- home_entrance_detection
```

## Debugging problems
Turn on debug for both aionefit as custom_components.nefiteasy:
```
logger:
  default: info
  logs:
    custom_components.nefiteasy: debug
    aionefit: debug
```

## FAQ
#### HA cannot connect to Bosch cloud 

If you experience errors during the boot of HA regarding the connection to the Bosch cloud ie ```Timeout while connecting to Bosch cloud. Retrying in the background```, you may want to change the OpenSSL configuration.
The OpenSSL defaults for Buster are causing the problem, probably because Nefit/Bosch is using either an outdated TLS version or an outdated cipher.

Edit ```/etc/ssl/openssl.cnf``` and change the system wide defaults back to their previous values(pre-Buster):
```
MinProtocol = None
CipherString = DEFAULT
```

https://www.debian.org/releases/stable/amd64/release-notes/ch-information.en.html#openssl-defaults