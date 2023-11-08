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
  - [0x282](#can-id-0x282)
  - [0x6D1](#can-id-0x6D1)
- [Logs](#logs)
- [Tools](#tools)
- [Resources/References](#resourcesreferences)

## Introduction

The following outlines CAN BUS ids and data I've discovered while sniffing the (high speed, 500KB/s) CAN BUS on my 2016 Subaru WRX.

Some of these are certain, some are still only suspicions. These are my personal notes that can hopefully help anyone else looking at the CAN network for this particular car.

## CAN IDs and Data

I'm generally only highlighting the data that is significant. `00` indicates either observed `00` data, or unknown/irrelevant data.

### CAN ID `0x152`

#### Byte 8

Headlights:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8                | Comments        |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ----------------- | --------------- |
| `0x152` | -    | -    | -    | -    | -    | -    | -    | `80` (`10000000`) | Lights off      |
| `0x152` | -    | -    | -    | -    | -    | -    | -    | `84` (`10000100`) | Parking lights  |
| `0x152` | -    | -    | -    | -    | -    | -    | -    | `8C` (`10001100`) | Full headlights |
| `0x152` | -    | -    | -    | -    | -    | -    | -    | `98` (`10011000`) | High-beams      |

Turn signal:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8                | Comments     |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ----------------- | ------------ |
| `0x152` | -    | -    | -    | -    | -    | -    | -    | `00` (`00000000`) | Off          |
| `0x152` | -    | -    | -    | -    | -    | -    | -    | `10` (`00010000`) | Left signal  |
| `0x152` | -    | -    | -    | -    | -    | -    | -    | `20` (`00100000`) | Right signal |

Windshield wipers:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8                | Comments |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ----------------- | -------- |
| `0x152` | -    | -    | -    | -    | -    | -    | -    | `C0` (`11000000`) | Active   |

### Byte 7

Parking/hand brake and brake pedal:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7                | B8   | Comments                      |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ----------------- | ---- | ----------------------------- |
| `0x152` | -    | -    | -    | -    | -    | -    | `00` (`00000000`) | -    | None                          |
| `0x152` | -    | -    | -    | -    | -    | -    | `08` (`00001000`) | -    | Parking/hand brake            |
| `0x152` | -    | -    | -    | -    | -    | -    | `10` (`00010000`) | -    | Brake pedal engaged           |
| `0x152` | -    | -    | -    | -    | -    | -    | `18` (`00011000`) | -    | Brake pedal and parking brake |

### CAN ID `0x140`

#### Bytes 6 & 7

Accelerator pedal pressure:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments              |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | --------------------- |
| `0x140` | -    | -    | -    | -    | -    | `FF` | `FF` | -    | B6 & B7, little endian|

### CAN ID `0x0D1`

#### Bytes 1 & 2

Vehicle speed:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments               |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---------------------- |
| `0x0D1` | `FF` | `FF` | ---- | ---- | ---- | ---- | ---- | ---- | B1 & B2, little endian |

For example, if `B1` and `B2` are `0x0F` and `0x04` respectively:

```
(4 * 256) + 15 = 1039
1039 * 0.05625 = ~58 km/h
```

#### Byte 3

Brake pedal pressure:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments              |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | --------------------- |
| `0x0D1` | -    | -    | `00` | -    | -    | -    | -    | -    | No pressure           |
| `0xOD1` | -    | -    | `53` | -    | -    | -    | -    | -    | Some (full?) pressure |

I wasn't able to observe a value higher than about `0x53` for byte 3 for brake pressure while casually stomping on the pedal.

### CAN ID `0x0D4`

#### Bytes 1-8

Individual wheel speeds:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments                            |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ----------------------------------- |
| `0x0D1` | `FF` | `FF` | -    | -    | -    | -    | -    | -    | B1 & B2, little endian, left front  |
| `0x0D1` | -    | -    | `FF` | `FF` | -    | -    | -    | -    | B3 & B4, little endian, right front |
| `0x0D1` | -    | -    | -    | -    | `FF` | `FF` | -    | -    | B5 & B6, little endian, left rear   |
| `0x0D1` | -    | -    | -    | -    | -    | -    | `FF` | `FF` | B7 & B8, little endian, right rear  |

See [0x0D1](#can-id-0x0d1) above for speed calculation.


### CAN ID `0x375`

#### Byte 1

Door locking state:

| ID      | B1                | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments                                |
| ------- | ----------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | --------------------------------------- |
| `0x375` | `00` (`00000000`) | -    | -    | -    | -    | -    | -    | -    | Driver and passenger doors locked       |
| `0x375` | `01` (`00000001`) | -    | -    | -    | -    | -    | -    | -    | Passenger door (RF) only locked (bit 1) |
| `0x375` | `02` (`00000010`) | -    | -    | -    | -    | -    | -    | -    | Driver door (LF) only locked (bit 2)    |
| `0x375` | `03` (`00000011`) | -    | -    | -    | -    | -    | -    | -    | All doors unlocked                      |

#### Byte 2

Door open state:

| ID      | B1   | B2   | B3         | B4   | B5   | B6   | B7   | B8   | Comments                                |
| ------- | ---- | ---- | ---------- | ---- | ---- | ---- | ---- | ---- | --------------------------------------- |
| `0x375` | -    | `00` (`00000000`) | -    | -    | -    | -    | -    | -    | All doors closed                 |
| `0x375` | -    | `01` (`00000001`) | -    | -    | -    | -    | -    | -    | LF (driver) door open (bit 1)    |
| `0x375` | -    | `02` (`00000010`) | -    | -    | -    | -    | -    | -    | RF (passenger) door open (bit 2) |
| `0x375` | -    | `04` (`00000100`) | -    | -    | -    | -    | -    | -    | RR door open (bit 3)             |
| `0x375` | -    | `08` (`00001000`) | -    | -    | -    | -    | -    | -    | LR door open (bit 4)             |
| `0x375` | -    | `08` (`00100000`) | -    | -    | -    | -    | -    | -    | Trunk open (bit 6)               |

### CAN ID `0x002`

#### Bytes 1 & 2

Steering wheel position:

Combination of two bytes used for steering wheel position.

| ID      | B1        | B2        | B3   | B4   | B5   | B6   | B7   | B8   | Comments                 |
| ------- | --------- | --------- | ---- | ---- | ---- | ---- | ---- | ---- | ------------------------ |
| `0x002` | `[01-19]` | `[00-FF]` | -    | -    | -    | -    | -    | -    | Turning wheel left (CCW) |

`B1` and `B2` are used together to get a pretty high definition value of steering wheel motion. From a position of dead centre (`00,00`), turning the wheel left: `00,01` -> `00,02` -> ... -> `01,00` -> `01,01` etc

Cranking the wheel hard left, I was only able to observe the first byte go as high as `0x19`.

Turning the wheel right from a centered position shows a reduction in values:

| ID      | B1        | B2        | B3   | B4   | B5   | B6   | B7   | B8   | Comments                 |
| ------- | --------- | --------- | ---- | ---- | ---- | ---- | ---- | ---- | ------------------------ |
| `0x002` | `[FF-E6]` | `[FF-00]` | -    | -    | -    | -    | -    | -    | Turning wheel right (CW) |

I didn't test with the wheel all the way right, but I assumed `B1` would range from `0xFF` to about `0xE6`.

I used a calculation such as below to determine steering wheel position as a percentage, from -100% (right) to 0 to 100% (left), assuming conversion to decimal:

```
LEFT: (B1 * 255 + B2) / 26*255
RIGHT: (((255 - B1) * 255) + B2) / 26*255 * -1
* where the max observed was B1 and B2 were 0x19 (25 in dec) and 0xFF respectively
```

I saw previously somewhere that this is actually a measurement of degrees, but I can no longer find the calculation.

### CAN ID `0x0D0`

I observed very similar data on bytes 1 and 2 from CAN ID `0x0D0` for steering wheel position but didn't not investigate further on maximum values.

### CAN ID `0x281`

#### Byte 1

Climate control fan speed (bits 5-8):

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments  |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | --------- |
| `0x281` | `XX` | -    | -    | -    | -    | -    | -    | -    |           |
| `0x281` | `XX` | -    | -    | -    | -    | -    | -    | -    |           |

Fan speed shows increases on byte 1 by `0x20` for each level (`20`, `40`, `60`, ..., `E0`), utilizing the last 4 bits:

- `00000000` - off
- `00010000` - speed 1
- `00100000` - speed 2
- `00110000` - speed 3
- `01000000` - speed 3
- `01010000` - speed 5
- `01110000` - speed 6
- `10000000` - speed 7

Air direction/vent modes (bits 3-4):

- `00000000` - Footwell vents + windshield 
- `00000100` - Upper vents
- `00001000` - Upper vents + footwell vents
- `00001100` - Footwell vents

#### Byte 2

Appears to be used for combinations the automatic climate control modes, by bit:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments  |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | --------- |
| `0x281` | -    | `XX` | -    | -    | -    | -    | -    | -    |           |
| `0x281` | -    | `XX` | -    | -    | -    | -    | -    | -    |           |

- `00000100` - Windshield defrost (bit 3)
- `00001000` - "Full auto" mode? (bit 4)
- `00010000` - Air conditioning (bit 5)
- `00100000` - Rear defroster (bit 6)
- `01000000` - Air recirculation (bit 7)
- `10000000` - Not sure? (bit 8)

#### Byte 3

Climate control temperature:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments                  |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ------------------------- |
| `0x281` | -    | -    | `00` | -    | -    | -    | -    | -    | 'LOW' setting (18&deg;C)  |
| `0x281` | -    | -    | `2c` | -    | -    | -    | -    | -    | 18.5&deg;C                |
| `0x281` | -    | -    | `30` | -    | -    | -    | -    | -    | 19&deg;C                  |
| `0x281` | -    | -    | `34` | -    | -    | -    | -    | -    | 19.5&deg;C                |
| `0x281` | -    | -    | `38` | -    | -    | -    | -    | -    | 20&deg;C                  |
| `0x281` | -    | -    | `..` | -    | -    | -    | -    | -    | etc                       |
| `0x281` | -    | -    | `94` | -    | -    | -    | -    | -    | 31.5&deg;C                |
| `0x281` | -    | -    | `B0` | -    | -    | -    | -    | -    | 'HIGH' setting (32&deg;C) |

### CAN ID `0x282`

#### Byte 6

Seatbelt buckle (inverse) state for driver and passenger (bits 1 and 2)

| ID      | B1                | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments          |
| ------- | ----------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ----------------- |
| `0x282` | `03` (`00000011`) | -    | -    | -    | -    | -    | -    | -    | Both unbuckled    |
| `0x282` | `02` (`00000010`) | -    | -    | -    | -    | -    | -    | -    | Driver buckled    |
| `0x282` | `01` (`00000001`) | -    | -    | -    | -    | -    | -    | -    | Passenger buckled |

### CAD ID `0x6D1`

Odometer reading:

| ID      | B1   | B2   | B3   | B4   | B5   | B6   | B7   | B8   | Comments        |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | --------------- |
| `0x282` | `0D` | `0C` | `0B` | `0A` | -    | -    | -    | -    | Little endian   |

Calculation per [OBD PID A6 spec](https://en.wikipedia.org/wiki/OBD-II_PIDs)

```
(A*2^24 + B* B*2^16 + C*2^8 + B) / 10
```

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
