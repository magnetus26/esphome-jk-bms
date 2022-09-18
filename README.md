# esphome-jk-bms

![GitHub actions](https://github.com/syssi/esphome-jk-bms/actions/workflows/ci.yaml/badge.svg)
![GitHub stars](https://img.shields.io/github/stars/syssi/esphome-jk-bms)
![GitHub forks](https://img.shields.io/github/forks/syssi/esphome-jk-bms)
![GitHub watchers](https://img.shields.io/github/watchers/syssi/esphome-jk-bms)
[!["Buy Me A Coffee"](https://img.shields.io/badge/buy%20me%20a%20coffee-donate-yellow.svg)](https://www.buymeacoffee.com/syssi)

ESPHome component to monitor a Jikong Battery Management System (JK-BMS) via RS485 or BLE

![Lovelace entities card](lovelace-entities-card.png "Lovelace entities card")

## Supported devices

All JK-BMS models with software version `>=6.0` are using the implemented protocol and should be supported.

* JK-BD6A17S6P, hw 7.2, sw 7.1.0H
* JK-BD6A17S8P, hw 9.x, sw 9.01G (reported by [@jonadis](https://github.com/syssi/esphome-jk-bms/issues/35#issuecomment-1035312712))
* JK-BD6A20S10P, hw 10.XW, sw 10.07 (reported by [@adadrag](https://github.com/syssi/esphome-jk-bms/issues/123))
* JK-BD6A24S6P, hw 6.x, sw 6.10S (reported by [@ziporah](https://github.com/syssi/esphome-jk-bms/issues/41))
* JK-BD6A24S10P, hw 8.x, sw 8.0.6G (reported by [@spoonwzd](https://github.com/syssi/esphome-jk-bms/issues/67#issuecomment-1093844076))
* JK-B1A24S15P, hw 8.x, sw 8.1.0H (reported by [@killee](https://github.com/syssi/esphome-jk-bms/discussions/4))
* JK-B1A20S15P, hw 8.x, sw 8.14U (reported by  [@trippfam07](https://github.com/syssi/esphome-jk-bms/issues/31))
* JK-B2A24S15P, hw 6.x, sw 6.1.3S (reported by [@miguel300477](https://github.com/syssi/esphome-jk-bms/issues/57))
* JK-B2A24S15P, hw 8.x, sw 8.21W (reported by [@mariusvaida](https://github.com/syssi/esphome-jk-bms/issues/120))
* JK-B2A24S15P, hw 10.xw, sw 10.07
* JK-B2A24S20P, hw 8.x, sw 8.1.2H (reported by [@KlausLi](https://github.com/syssi/esphome-jk-bms/issues/15#issuecomment-961447064))
* JK-B2A24S20P, hw 8.x, sw 8.20G (reported by [@rob-oravec](https://github.com/syssi/esphome-jk-bms/discussions/46))
* JK-B2A24S20P, hw 10.X-W, sw 10.02 (reported by [@SeByDocKy](https://github.com/syssi/esphome-jk-bms/issues/37#issuecomment-1040569576))
* JK-B2A24S20P, hw 10.XG, sw 10.07D30 (reported by [@TheSmartGerman](https://github.com/syssi/esphome-jk-bms/discussions/122))
* JK-B2A24S20P, hw 10.XW, sw 10.07 (reported by [@amagr0](https://github.com/syssi/esphome-jk-bms/issues/124#issuecomment-1166366196))
* JK-B2A8S20P,  hw 9.x, sw 9.01M3 (reported by [@EasilyBoredEngineer](https://github.com/syssi/esphome-jk-bms/discussions/110))
* JK-B2A8S20P, hw 9.x, sw 9.08W (reported by [@vrabi-cv](https://github.com/syssi/esphome-jk-bms/discussions/144#discussioncomment-3285901))
* JK-B2A8S20P, hw 11.XW, sw 11.17 (reported by [@senfkorn](https://github.com/syssi/esphome-jk-bms/issues/147))
* JK-B2A20S20P, hw 10.XW, sw 10.09 (reported by [@markusgg84](https://github.com/syssi/esphome-jk-bms/discussions/173))
* GW-24S4EB (NEEY/Heltec 4A Smart Active Balancer), hw HW-2.8.0, sw ZH-1.2.3 (reported by [@cristi2005](https://github.com/syssi/esphome-jk-bms/issues/109))

## Untested devices

* JK-BD6A20S6P

## Requirements

* [ESPHome 1.18.0 or higher](https://github.com/esphome/esphome/releases).
* Generic ESP32 or ESP8266 board

## Schematics

<a href="https://raw.githubusercontent.com/syssi/esphome-jk-bms/main/images/circuit.jpg" target="_blank">
<img src="https://raw.githubusercontent.com/syssi/esphome-jk-bms/main/images/circuit_thumbnail.jpg" width="50%">
</a>


```
                RS485-TTL
┌──────────┐                ┌─────────┐
│          │<----- RX ----->│         │
│  JK-BMS  │<----- TX ----->│ ESP32/  │
│          │<----- GND ---->│ ESP8266 │<-- 3.3V
│          │                │         │<-- GND
└──────────┘                └─────────┘

# RS485-TTL jack (4 Pin, JST 1.25mm pitch)
┌─── ─────── ────┐
│                │
│ O   O   O   O  │
│GND  RX  TX VBAT│
└────────────────┘
  │   │   │
  │   │   └─── GPIO17 (`rx_pin`)
  │   └─────── GPIO16 (`tx_pin`)
  └─────────── GND
```


The RS485-TTL jack of the BMS can be attached to any UART pins of the ESP. A hardware UART should be preferred because of the high baudrate (115200 baud).

## Installation

You can install this component with [ESPHome external components feature](https://esphome.io/components/external_components.html) like this:
```yaml
external_components:
  - source: github://syssi/esphome-jk-bms@main
```

or just use the `esp32-example.yaml` as proof of concept:

```bash
# Install esphome
pip3 install esphome

# Clone this external component
git clone https://github.com/syssi/esphome-jk-bms.git
cd esphome-jk-bms

# Create a secrets.yaml containing some setup specific secrets
cat > secrets.yaml <<EOF
wifi_ssid: MY_WIFI_SSID
wifi_password: MY_WIFI_PASSWORD

mqtt_host: MY_MQTT_HOST
mqtt_username: MY_MQTT_USERNAME
mqtt_password: MY_MQTT_PASSWORD
EOF

# Validate the configuration, create a binary, upload it, and start logs
# If you use a esp8266 run the esp8266-examle.yaml
esphome run esp32-example.yaml

```

## Example response all sensors enabled

```
[sensor:127]: 'jk-bms cell voltage 1': Sending state 4.12500 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage 2': Sending state 4.12500 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage 3': Sending state 4.12800 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage 4': Sending state 4.12400 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage 5': Sending state 4.12500 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage 6': Sending state 4.12800 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage 7': Sending state 4.12400 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage 8': Sending state 4.12300 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage 9': Sending state 4.12800 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage 10': Sending state 4.12800 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage 11': Sending state 4.12800 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage 12': Sending state 4.13100 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage 13': Sending state 4.12400 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms power tube temperature': Sending state 24.00000 °C with 0 decimals of accuracy
[sensor:127]: 'jk-bms temperature sensor 1': Sending state 22.00000 °C with 0 decimals of accuracy
[sensor:127]: 'jk-bms temperature sensor 2': Sending state 22.00000 °C with 0 decimals of accuracy
[sensor:127]: 'jk-bms total voltage': Sending state 53.64000 V with 2 decimals of accuracy
[sensor:127]: 'jk-bms current': Sending state -0.00000 A with 2 decimals of accuracy
[sensor:127]: 'jk-bms capacity remaining': Sending state 99.00000 % with 0 decimals of accuracy
[sensor:127]: 'jk-bms temperature sensors': Sending state 2.00000  with 0 decimals of accuracy
[sensor:127]: 'jk-bms charging cycles': Sending state 0.00000  with 0 decimals of accuracy
[sensor:127]: 'jk-bms total charging cycle capacity': Sending state 0.00000  with 0 decimals of accuracy
[sensor:127]: 'jk-bms battery strings': Sending state 13.00000  with 0 decimals of accuracy
[sensor:127]: 'jk-bms errors bitmask': Sending state 0.00000  with 0 decimals of accuracy
[text_sensor:015]: 'jk-bms errors': Sending state ''
[sensor:127]: 'jk-bms operation mode bitmask': Sending state 0.00000  with 0 decimals of accuracy
[text_sensor:015]: 'jk-bms operation mode': Sending state ''
[sensor:127]: 'jk-bms total voltage overvoltage protection': Sending state 5.46000 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms total voltage undervoltage protection': Sending state 3.77000 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage overvoltage protection': Sending state 4.20000 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage overvoltage recovery': Sending state 4.10000 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage overvoltage delay': Sending state 5.00000 s with 0 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage undervoltage protection': Sending state 2.90000 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage undervoltage recovery': Sending state 3.20000 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms cell voltage undervoltage delay': Sending state 5.00000 s with 0 decimals of accuracy
[sensor:127]: 'jk-bms cell pressure difference protection': Sending state 0.30000 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms discharging overcurrent protection': Sending state 60.00000 A with 0 decimals of accuracy
[sensor:127]: 'jk-bms discharging overcurrent delay': Sending state 300.00000 s with 0 decimals of accuracy
[sensor:127]: 'jk-bms charging overcurrent protection': Sending state 25.00000 A with 0 decimals of accuracy
[sensor:127]: 'jk-bms charging overcurrent delay': Sending state 30.00000 s with 0 decimals of accuracy
[sensor:127]: 'jk-bms balance starting voltage': Sending state 3.30000 V with 3 decimals of accuracy
[sensor:127]: 'jk-bms balance opening pressure difference': Sending state 0.01000 V with 3 decimals of accuracy
[switch:045]: 'jk-bms balancing': Sending state ON
[sensor:127]: 'jk-bms power tube temperature protection': Sending state 90.00000 °C with 0 decimals of accuracy
[sensor:127]: 'jk-bms power tube temperature recovery': Sending state 70.00000 °C with 0 decimals of accuracy
[sensor:127]: 'jk-bms temperature sensor temperature protection': Sending state 100.00000 °C with 0 decimals of accuracy
[sensor:127]: 'jk-bms temperature sensor temperature recovery': Sending state 100.00000 °C with 0 decimals of accuracy
[sensor:127]: 'jk-bms temperature sensor temperature difference protection': Sending state 20.00000 °C with 0 decimals of accuracy
[sensor:127]: 'jk-bms charging high temperature protection': Sending state 70.00000 °C with 0 decimals of accuracy
[sensor:127]: 'jk-bms discharging high temperature protection': Sending state 70.00000 °C with 0 decimals of accuracy
[sensor:127]: 'jk-bms charging low temperature protection': Sending state -20.00000 °C with 0 decimals of accuracy
[sensor:127]: 'jk-bms charging low temperature recovery': Sending state -10.00000 °C with 0 decimals of accuracy
[sensor:127]: 'jk-bms discharging low temperature protection': Sending state -20.00000 °C with 0 decimals of accuracy
[sensor:127]: 'jk-bms discharging low temperature recovery': Sending state -10.00000 °C with 0 decimals of accuracy
[switch:045]: 'jk-bms charging': Sending state OFF
[switch:045]: 'jk-bms discharging': Sending state OFF
[sensor:127]: 'jk-bms current calibration': Sending state 0.72500 A with 3 decimals of accuracy
[sensor:127]: 'jk-bms device address': Sending state 1.00000  with 0 decimals of accuracy
[text_sensor:015]: 'jk-bms battery type': Sending state 'Ternary Lithium'
[sensor:127]: 'jk-bms sleep wait time': Sending state 10.00000 s with 0 decimals of accuracy
[sensor:127]: 'jk-bms alarm low volume': Sending state 20.00000  with 0 decimals of accuracy
[text_sensor:015]: 'jk-bms password': Sending state '123456'
[switch:045]: 'jk-bms dedicated charger': Sending state OFF
[text_sensor:015]: 'jk-bms device type': Sending state 'Input Us'
[sensor:127]: 'jk-bms total runtime': Sending state 0.00000 h with 0 decimals of accuracy
[text_sensor:015]: 'jk-bms software version': Sending state 'H7.X__S7.1.0H__'
[sensor:127]: 'jk-bms actual_battery_capacity': Sending state 186.00000 Ah with 0 decimals of accuracy
[text_sensor:015]: 'jk-bms manufacturer': Sending state 'BT3072020120000200521001'
[sensor:127]: 'jk-bms protocol version': Sending state 1.00000  with 0 decimals of accuracy
```

## Known issues

* The battery type sensor is pretty useless because the BMS reports always the same value (`Ternary Lithium`). Regardless of which battery type was set / parameter set was loaded via the android app. ([#9][i9])
* ESP32: Adding all supported sensors can lead to a stack overflow / boot loop. This can be solved by increasing the stack size. ([#63][i63])
* BLE: Please stick to the `esp-idf` framework because the Arduino framework crashs on the first received BLE notification.
* Raspberry Pi & ESP-IDF: If the project doesn't compile because of `Error: Could not find the package with 'platformio/toolchain-esp32ulp @ ~1.22851.0' requirements for your system 'linux_aarch64'` please use a host with another processor architecture (f.e. x86). The toolchain isn't `linux_aarch64` (ARM64) compatible at the moment.
* MQTT & BLE: Please use ESPHome `>=2022.4.0` if you want to use the BLE component (requires `esp-idf`) and MQTT. The MQTT component wasn't ESP-IDF compatible until then.

[i9]: https://github.com/syssi/esphome-jk-bms/issues/9
[i63]: https://github.com/syssi/esphome-jk-bms/issues/63

## Debugging

If this component doesn't work out of the box for your device please update your configuration to enable the debug output of the UART component and increase the log level to the see outgoing and incoming serial traffic:

```
logger:
  level: DEBUG

uart:
  id: uart0
  baud_rate: 115200
  rx_buffer_size: 384
  tx_pin: GPIO14
  rx_pin: GPIO4
  debug:
    direction: BOTH
```

## References

* https://secondlifestorage.com/index.php?threads/jk-b1a24s-jk-b2a24s-active-balancer.9591/
* https://github.com/jblance/jkbms
* https://github.com/jblance/mpp-solar/issues/112
* https://github.com/jblance/mpp-solar/blob/master/mppsolar/protocols/jk232.py
* https://github.com/jblance/mpp-solar/blob/master/mppsolar/protocols/jk485.py
* https://github.com/sshoecraft/jktool
* https://github.com/Louisvdw/dbus-serialbattery/blob/master/etc/dbus-serialbattery/jkbms.py
* https://blog.ja-ke.tech/2020/02/07/ltt-power-bms-chinese-protocol.html
