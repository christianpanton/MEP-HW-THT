# MEP-HW-THT
Multipurpose Expansion Port (MEP) Through-Hole Technology (THT) Hardware for OSGP Smart Meters in Denmark

This is the repository holding the Thrugh-Hole Technology (THT) Hardware for OSGP Smart Meters (i.e. Echelon or NES) in Denmark.
The scematics can be use in other countries too if you gain access to the Multipurpose Expansion Port (MEP) terminals.
Please:
- see https://github.com/OSGP-Alliance-MEP-and-Optical for further hardware specifiactions of these meters.
- see https://www.dabbler.dk for further information about this hardware project


Notes:
- The Gerber files has been generated acording to [JLCPCBs](https://jlcpcb.com/) specifications.
- We have not been able to source the correct MEP connector, so please note that the pin-numbers does NOT corespond to the pin numbers in the official documentation. Also you should NOT insert the 2x3 pin headers all the way into the PCB (the pins should be aligned with the PCB back side).
- The ESP32 can be programmed "on board" using the J5 connector. We usually don't put headers on that one, but simply insert the pins of a FTDI232 directly into the holes. WARNING: Some FTDI232's supply 5v EVEN when they are jumped to 3.3v. Don't use those - they'll fry your ESP32!
- Debug information from the software are written on the serial out on J5. Remember it is 3.3v and if you use a FTDI232, please only connect GND, TXD and RXD during normal operation (i.e. when the module is connected to a meter)
- The Schematic for the THT and the SMD versions are currently the same, but they might branch in the future
- J7 have two functions: before "on board" programming of the ESP32 a jumper must be set between ESP_PROGRAM and GND (pin 1 and 2) and the power jumper between pin 3 and 4 must be removed
- WARNING: Remove all jumpers from J7 before adjusting the buck converter voltage to 3.6v. When done set a jumper between pin 3 and 4 for nomal operation.
- The buck converter must be mounted without pin headers, i.e. flush to the PCB. Just put solder blobs in the holes and use additional heat for a good connection. The module will not fit in the meter if pin headers are used!

Troubleshooting PCBs with fake MAX3232:
- If you don't have R3 populated (ESP32 controlled power for the MAX3232 - see below), you will be able to measure the set voltage from the buck converter on one pin, and the actual power suppliedby the ESP32 on the other. The ESP32 cannot supply much power, so you'll measure a voltage far below the 3.3v required by the MAX3232, the MAX3232 won't work and your ESP32 will get hot. Don't run in that configuration for long - replace your MAX3232 with a working one!
- If you don't have R3 populated (ESP32 controlled power for the MAX3232 - see below), remove the J7 jumper completely and measure resistance between the ESP32 pin 1 and 2, you'll see a short if your ESP32 is fried
- If you have R3 populated (but not R4 populated), you can measure if your ESP32 is fried by removing J7 and the MAX3232 chip
- You can check if your MAX3232 chip is fried or fake by measuring resistance between pin 15 and 16 when it is not in the board. It should be above 15Kohm - possible 30-40Kohm. Warning: It might still be a fake if it is, but at least it is not shorting anything then (see the first point in this section

Bill of Materials:
| Component | Value | Type | Note |
| ------------- | ------------- | ------------- | ------------- |
| R1-R2 | 10K ohm | | |
| R3-R7 | 0 ohm or wire | | Mount either R3 (permanent) or R4 (ESP32 controlled, recommended) depending on how you will power your MAX3232. See https://www.dabbler.dk/index.php/2022/04/03/echelon-nes-smart-meters-dabbling-the-hardware-v1-10-and-v2-00/ |
| D1 | 1N4007 | | |
| C1-C5 | 100nF | Non-polarized | |
| U1 | MAX3232 and socket | DIP-16 | 3.3v version (warning: lots of fake chips out there - we recommend using a socket). IMPORTANT: Make sure to tim the pins on the back of the PCB so they stay clear of the J6 connector pins! |
| U2 | ESP32-wroom-32e | 16MB recommended but currently NOT required |
| J1-J4 | DC-DC Buck converter | | i.e. https://www.ebay.com/itm/264731212329 |
| J6 | 2x3 Horizontal pin header | 2.54mm | IMPORTANT: Do not insert this pin header fully in the PCB. The pins should be soldered flush with the PCB surface to fit the connector in the meter! i.e. https://www.ebay.com/itm/253023279430 |
| J7 | 2x2 Vertical pin header | 2.54mm | |
