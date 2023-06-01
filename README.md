# Tasmota Berry driver for Sonoff TX Ultimate

Berry driver for Sonoff TX Ultimate touch panel.

TX Ultimate is a touch switch with RGB LEDs, haptic motor and I2S audio speaker. It is available from [Sonoff's store](https://itead.cc/product/sonoff-tx-ultimate-smart-touch-wall-switch/ref/34) in 1, 2 and 3 gang versions.

## Functions

Touch panel is divided into 10 vertical segments or channels. 
It has the following events: 
- touch event with position - triggers on each touch
- short press 
- long press
- multi touch - touching multiple channels at once
- swipe left and right - has starting and ending coordinate
- dash - shorter than swipe, usually moving from one channel to the adjacent one

Shown in console and in MQTT as:

```json
tele/tasmota/RESULT = {"TXUltimate":{"Action":"Touch","Channel":3}}
tele/tasmota/RESULT = {"TXUltimate":{"Action":"Multi"}}
tele/tasmota/RESULT = {"TXUltimate":{"Action":"Short","Channel":4}}
tele/tasmota/RESULT = {"TXUltimate":{"Action":"Long","Channel":4}}
tele/tasmota/RESULT = {"TXUltimate":{"Action":"Swipe left","From":0,"To":252}}
```

## Serial Protocol

Field | Length | (Byte) | Explanation
   |   |   |
Frame | Header | 2 | 0xaa55
Version | 1 | 0x01
Opcode | 1 | Specific  command  type
Data | Length | 1 | Data  Length  0-64
Data | N
Checksum | 2 | CRC16 | CRC-16/CCITT-FALSE checksum  starting from the version byte

Quick and dirty skeleton driver. All touch events are reported to a RESULT topic. There's a beginning of a command interface which will be expanded to have calibration of the touch channels.

## Thanks to
Would not be possible without the help of [smarthomeyourself.de](https://smarthomeyourself.de/) and his [esphome custom component](https://github.com/SmartHome-yourself/sonoff-tx-ultimate-for-esphome/)