# Infected NFC breakout board

## Rationale

Infected wishes to develop an NFC infrastructure due to new wishes from the local authorities, as well as internal needs. For this platform to be developed as flawlessly as possible, the amount constant design features between the different NFC units needs to be as large as possible. For this reason, the raspberry pi(which serves as the microcontroller on the platform), should have a one-size-fits-all breakout board.

## Board summary

The board is to act as a bridge between the Raspberry pi and external peripherals, a "breakout board". This is done for multiple reasons:

 * Easier replacement of defect parts
 * More solid electric connections, less prone to breakage due to wear and tear.
 * Space saving
 * Less chance of breaking raspberry pi on electrical failures

The nfc platform consists of different devices with common and unique input devices that this breakout has to accomodate. This includes:

 * LED's
 * LED strips
 * NFC reader
 * Camera
 * Display

The breakout board should accomodate to LED's, as well as supplying a breakout for the NFC reader and i2c ports

## Board requirements

 * LED ports, not dimmable
 * 3-channel LED strip output
 * Breakout for the NFC sensor
 * i2c ports for at least 2 i2c devices
 * TxD and RxD ports for possible expansion

## Components

### RFID(U1)

 * Breakout for wiring to a seperate NFC board

### TLC5917IN(U2)

 * `U2`
 * Responsible for single channel LED's
  - PWM probably not possible
 * LED driver
 * `R1` controls the maximum output current
   - Calculated using the follow formula(Check table below for bit names):
   - $VG=(1+HC)*(1+D/64)/4$
   - $D = CC0*2^5+CC1*2^4+CC2*2^3+CC3*2^2+CC4*2+CC5$
   - The voltage output by $V_{R-EXT}$ is calculated by: $1.26*VG$
   - $I_{ref}=\frac{V_{R-EXT}}{R_{ext}}$
   - $I_{OUT,target}=I_{ref}*15*3^{CM-1}=\frac{1.26V}{R_{ext}}*VG*15*3^{CM-1}=(\frac{1.26V}{R_{ext}}*15)*CG$

#### Configuration byte

|         |  0 |  1 |   2 |   3 |   4 |   5 |   6 |   7 |
|---------|----|----|-----|-----|-----|-----|-----|-----|
| Meaning | CM | HC | CC0 | CC1 | CC2 | CC3 | CC4 | CC5 |
| Default |  1 |  1 |   1 |   1 |   1 |   1 |   1 |   1 |

### FDT457N(Q1, Q2, Q3)

 * `Q1`, `Q2`, `Q3`
 * Responsible for LED strip control
   - Current too high to connect strip directly to Raspberry pi
 * N-channel MOSFET
 * Max 5A, 30V
 * Surface mount

### 1206SFS125F/63-2(F1, F2, F3)

 * `F1`, `F2`, `F3`
 * Fuse, responsible for cutting of the LED strip before it causes other damage
 * `1.25A` trigger point.

### Headers

 * Two different headers are suggested in the parts list:
 * 2x20 pin header
 * Pins for the pi zero

### Resistors(R1-R9)

* `R1` is a special resistor which controls the output current of `U2`
  - Suggested value: *40 ohm*
  - This allows a current flow from `0.24` to `0.49`
* `R2`-`R9` are decided by board assembler depending on LED used.

### Jumpers(J1-15)

 * `J1` - LED strip jumper. +5V, and ground for R, G and B
 * `J2` - Raspberry pi jumper. Make sure the side marked *top* is facing the same way as the top of the raspberry pi
 * `J3` - Additional power outputs, for expansion
 * `J4` - I2C socket
 * `J5` - TxD, RxD socket
 * `J6` - I2C socket
 * `J7` - I2C socket
 * `J8`-`J15` - LED sockets

## History

### Revision 1

 * Initial release

### Revision 2

 * Dunno

### Revision 3

 * Finalized for production
 * Added fuse and MOSFETs for LED strip
 * Added credits and decoration

### Revision 4

 * Added Signature pane and serial number pane