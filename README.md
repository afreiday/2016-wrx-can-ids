# 2016 Subaru WRX CAN BUS Ids
CAN BUS ids sniffed from a 2016 Subaru WRX

- [Introduction](#introduction)
- [CAN IDs and Data](#can-ids-and-data)
  - [0x152](#can-id-0x152)
  - [0x140](#can-id-0x140)
  - [0x0D1](#can-id-0x0d1)
  - [0x0D4](#can-id-0x0d4)
  - [0x375](#can-id-0x375)
  - [0x002](#can-id-0x002)
  - [0x0D0](#can-id-0x0d0)
  - [0x281](#can-id-0x281)
- [Logs](#logs)
- [Tools](#tools)
- [Resources/References](#resourcesreferences)

## Introduction

The following outlines CAN BUS ids and data I've discovered while sniffing the (high speed, 500KB/s) CAN BUS on my 2016 Subaru WRX.

Some of these are certain, some are still only suspicions. These are my personal notes that can hopefully help anyone else looking at the CAN network for this particular car.

## CAN IDs and Data

I'm generally only highlighting the data that is significant. `00` indicates either observed `00` data, or unknown/irrelevant data.

### CAN ID `0x152`

Pulling in the left steering wheel stock for the high beam lights:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments        |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | --------------- |
| `0x152` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | `80` | Normal position |
| `0x152` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | `98` | Pull inwards    |

Engaging/disengaging the parking brake:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments   |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---------- |
| `0x152` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | Disengaged |
| `0x152` | `00` | `00` | `00` | `00` | `00` | `00` | `08` | `00` | Engaged    |

Turn signal:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments     |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ------------ |
| `0x152` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | Off          |
| `0x152` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | `10` | Left signal  |
| `0x152` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | `20` | Right signal |

Headlights:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments       |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | -------------- |
| `0x152` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | `80` | Off/daytime    |
| `0x152` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | `84` | Nighttime/full |
| `0x152` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | `8C` | Parking lights |

Windshield wipers:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | -------- |
| `0x152` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | `C0` | Active   |

### CAN ID `0x140`

Accelerator pedal pressure:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments      |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ------------- |
| `0x140` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | No pressure   |
| `0x140` | `FF` | `00` | `00` | `00` | `00` | `FF` | `00` | `00` | Full pressure |

Data in first and sixth bytes (`B1` and `B6`) range from `00` to `FF` for pressure applied to the pedal.

### CAN ID `0x0D1`

Brake pedal pressure:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments              |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | --------------------- |
| `0x0D1` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | No pressure           |
| `0xOD1` | `00` | `00` | `53` | `00` | `00` | `00` | `00` | `00` | Some (full?) pressure |

I wasn't able to observe a value higher than about `0x53` for byte 3 for brake pressure while casually stomping on the pedal.

Vehicle speed:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments               |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---------------------- |
| `0x0D1` | `FF` | `FF` | `00` | `00` | `00` | `00` | `00` | `00` | B1 & B2, little endian |

For example, if `B1` and `B2` are `0x0F` and `0x04` respectively:

`
(4 * 256) + 15 = 1039
1039 * 0.05625 = ~58 km/h
`

### CAN ID `0x0D4`

Individual wheel speeds:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments                            |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ----------------------------------- |
| `0x0D1` | `FF` | `FF` | `00` | `00` | `00` | `00` | `00` | `00` | B1 & B2, little endian, left front  |
| `0x0D1` | `00` | `00` | `FF` | `FF` | `00` | `00` | `00` | `00` | B3 & B4, little endian, right front |
| `0x0D1` | `00` | `00` | `00` | `00` | `FF` | `FF` | `00` | `00` | B5 & B6, little endian, left rear   |
| `0x0D1` | `00` | `00` | `00` | `00` | `00` | `00` | `FF` | `FF` | B7 & B8, little endian, right rear  |

See [0x0D1](#can-id-0x0d1) above for speed calculation.


### CAN ID `0x375`

Door locking state:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments                          |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | --------------------------------- |
| `0x375` | `03` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | All doors unlocked                |
| `0x375` | `02` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | Driver door (LF) only locked      |
| `0x375` | `01` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | Passenger door (RF) only locked   |
| `0x375` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | Driver and passenger doors locked |

First bit of `B1` represents driver lock, second bit for passenger lock.

- `11` - unlocked
- `10` - driver lock
- `01` - passenger lock
- `00` - both locked

Door open status:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments                 |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ------------------------ |
| `0x375` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | All doors closed         |
| `0x375` | `00` | `01` | `00` | `00` | `00` | `00` | `00` | `00` | LF (driver) door open    |
| `0x375` | `00` | `02` | `00` | `00` | `00` | `00` | `00` | `00` | RF (passenger) door open |
| `0x375` | `00` | `04` | `00` | `00` | `00` | `00` | `00` | `00` | RR door open             |
| `0x375` | `00` | `08` | `00` | `00` | `00` | `00` | `00` | `00` | LR door open             |

Similar to lock status above, with each bit representing a door:

- `0001` - LF open
- `0010` - RF open
- `0100` - RR open
- `1000` - LR open

### CAN ID `0x002`

Steering wheel position:

Combination of two bytes used for steering wheel position.

| ID      | B1        | B2        | B3   | B4   | B5   | B6   | B7   | B8   | Comments                 |
| ------- | --------- | --------- | ---- | ---- | ---- | ---- | ---- | ---- | ------------------------ |
| `0x002` | `[01-19]` | `[00-FF]` | `00` | `00` | `00` | `00` | `00` | `00` | Turning wheel left (CCW) |

`B1` and `B2` are used together to get a pretty high definition value of steering wheel motion. From a position of dead centre (`00,00`), turning the wheel left: `00,01` -> `00,02` -> ... -> `01,00` -> `01,01` etc

Cranking the wheel hard left, I was only able to observe the first byte go as high as `0x19`.

Turning the wheel right from a centered position shows a reduction in values:

| ID      | B1        | B2        | B3   | B4   | B5   | B6   | B7   | B8   | Comments                 |
| ------- | --------- | --------- | ---- | ---- | ---- | ---- | ---- | ---- | ------------------------ |
| `0x002` | `[FF-E6]` | `[FF-00]` | `00` | `00` | `00` | `00` | `00` | `00` | Turning wheel right (CW) |

I didn't test with the wheel all the way right, but I assumed `B1` would range from `0xFF` to about `0xE6`.

I used a calculation such as below to determine steering wheel position as a percentage, from -100% (right) to 0 to 100% (left), assuming conversion to decimal:

```
LEFT: (B1 * 255 + B2) / 26*255
RIGHT: ((255 - B1) * 255) / 26*255 * -1
* where the max observed was B1 and B2 were 0x19 (25 in dec) and 0xFF respectively
```

### CAN ID `0x0D0`

I observed very similar data on bytes 1 and 2 from CAN ID `0x0D0` for steering wheel position but didn't not investigate further on maximum values.

### CAN ID `0x281`

Climate control fan speed:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments  |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | --------- |
| `0x281` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | Fan off   |
| `0x281` | `E0` | `00` | `00` | `00` | `00` | `00` | `00` | `00` | Fan full  |

Fan speed shows increases on byte 1 by `0x20` for each level (`20`, `40`, `60`, ..., `E0`). So obviously fan speed is represented by the last 4 bits of `B1`:

- `00000000` - off
- `00010000` - speed 1
- `00100000` - speed 2
- `00110000` - speed 3
- `01000000` - speed 3
- `01010000` - speed 5
- `01110000` - speed 6
- `11110000` - speed 7

Climate control temperature:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments       |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | -------------- |
| `0x281` | `00` | `00` | `2C` | `00` | `00` | `00` | `00` | `00` | 'LOW' setting  |
| `0x281` | `00` | `00` | `B0` | `00` | `00` | `00` | `00` | `00` | 'HIGH' setting |

Climate control temperature is represented by the last 4 bits of the third byte, from at `0x2c`, in increments of `0x04` for each 0.5 degrees centigrade, to `B0`. For example, `00`, `2C`, `30`, `34`, ..., `AF`, `BO` In binary:

- `00101100`
- `00110000`
- `00110100`
- `00111000`
- ...
- `10110000`

## Logs

The following are some log files I obtained using `candump` with certain events. It's helpful to run these through `canplayer` on a virtual device. These were done with the ignition on but without the car running (unless otherwise specified)

- [logs/brake-pedal.log](logs/brake-pedal.log)
- [logs/gas-pedal.log](logs/gas-pedal.log)
- [logs/doors-open-close.log](logs/doors-open-close.log)
- [logs/steering-wheel.log](logs/steering-wheel.log)
- [logs/accessory-to-ignition.log](logs/accessory-to-ignition.log) - Going from accessory mode to ignition on (engine still off)
- [logs/car-running-idle.log](logs/car-running-idle.log) - From ignition on to turning engine on, idling
- [logs/driving.log](logs/driving.log) - A log from a short drive (5 minutes?)

## Tools

I've used the following tools to perform my sniffing in various ways:

- [Carloop](https://www.carloop.io/)
  - [socketcan_serial.ccp](https://github.com/carloop/carloop-library/blob/master/examples/socketcan_serial/socketcan_serial.cpp)
- [can-utils](https://github.com/linux-can/can-utils)
- [socketcand](https://github.com/dschanoeh/socketcand)
- Arduino
  - [Sparkfun CAN-BUS Shield](https://github.com/dschanoeh/socketcand)
  - [slcauino](https://github.com/kahiroka/slcanuino)
  - [ElecFreaks CAN-BUS Shield](http://elecfreaks.com/store/canbus-shield-p-746.html)
  - [Seeed-Studio CAN BUS Shield library](https://github.com/Seeed-Studio/CAN_BUS_Shield)

## Resources/References

These are the resources I used either when I was getting started or to dig a bit deeper:

- [Use Carloop with SocketCAN and can-utils](https://community.carloop.io/t/use-carloop-with-socketcan-and-can-utils/117)
- [CAN Messages | Subaru Diesel Crew](https://subdiesel.wordpress.com/ecu-analysis/can-messages/)
- [The Car Hacker's Handbook](http://opengarages.org/handbook/ebook/)
