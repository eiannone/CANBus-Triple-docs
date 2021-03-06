---
title: Base Firmware Serial API
docs: true
section: Firmware
---

### Overview

The CANBus Triple is controlled by sending commands over the USB Serial connection, or over the Bluetooth LE Serial Service. Output will always be returned over the last serial interface that recieved data. The only exception is packet logging. Packets will always be sent over USB Serial unless they match the filters set vis the 0x04 command outlined below. 


### System Info and EEPROM

| Command | Sub-command | Function | Example Response |
|---------|-------------|----------|------------------|
| 0x01    | 0x01        | Print System Info | {"event":"version", "name":"CANBus Triple Mazda", "version":"0.2.5", "memory":"740"} |
| 0x01    | 0x02        | Dump eeprom value | "1:1:0:0:0:0:0:0..." A colon delimited list of the current EEPROM data as Hex Strings |
| 0x01    | 0x03        | Read and save eeprom | TODO |
| 0x01    | 0x04        | Restore eeprom to stock values | Clear all EEPROM data back to default settings |
| 0x01    | 0x10 0xN    | Print Bus N Debug to Serial | N is thr Bus number, 1 through 3 {"e":"busdgb", "name":"Bus 1", "canctrl":"7", "status":"14", "error":"150", "nextTxBuffer":"2"} |
| 0x01    | 0x16        | Reboot to bootloader | Restart the MCU into the Bootloader for flashing new program code |


### Send CAN Packet

Send a CAN Packet over the specified Bus. Bus Id should be 1 through 3

| Command   | Bus Id |  Message ID  | Data | Length |
|-----------|--------|--------------|------|--------|
| 0x02      | 01     | 290          | 00 00 00 00 00 00 00 00 | 8


### Set USB Serial Logging Output

(Filters are optional)

| Cmd  | Bus Id  | On/Off | Message ID 1 | Message ID 2 | Function |
|------|---------|--------|--------------|--------------|----------|
| 0x03 | 0x01    | 0x01   | 0x0290       | 0x0291       | Enable logging on Bus 1 and filter for Message Id 0x290 or 0x291 |
| 0x03 | 0x01    | 0x01   | 0x0000       | 0x0000       | Enable logging on Bus 1 and disable filtering |
| 0x03 | 0x02    | 0x01   | 0x0000       | 0x0000       | Enable logging on Bus 2 and disable filtering |
| 0x03 | 0x03    | 0x01   | 0x0000       | 0x0000       | Enable logging on Bus 3 and disable filtering |
| 0x03 | 0x01    | 0x00   |              |              | Disable logging on Bus 1 |


### Set Bluetooth Message ID filter

Somethign

| Command | BusId | Message ID 1 | Message ID 2 | Function                               |
|---------|-------|--------------|--------------|----------------------------------------|
| 0x04    | 0x01  | 0x0290       | 0x0291       | Enable Message ID 290 output over BT   |
| 0x04    | 0x01  | 0x0000       | 0x0000       | Disable |


### Bluetooth Functions

| Command  | Sub-Command | Function                     |
|----------|-------------|------------------------------|
| 0x08     | 0x01        | Reset BLE112    ( Deprecated ) |
| 0x08     | 0x02        | Enter pass through mode to talk to BLE112 |
| 0x08     | 0x03        | Exit pass through mode [Not yet implemented] |