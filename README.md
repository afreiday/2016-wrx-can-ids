# 2016 Subaru WRX CAN BUS Ids
CAN BUS ids sniffed from a 2016 Subaru WRX

## Introduction

The following outlines CAN BUS ids and data I've discovered while sniffing the (high speed, 500KB/s) CAN BUS on my 2016 Subaru WRX.

Some of these are certain, some are still only suspicions. These are my personal notes that can hopefully help anyone else looking at the CAN network for this particular car.

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