# Cheetah - Hybrid SMD/THD FreeEMS circuit/PCB design

### Introduction

The name Cheetah was chosen for the following two reasons. The Cheetah is the
fastest of the big cats, and also the fastest land mammal on earth. The other
reason the name was chosen is that Cheetah is a fork of Puma, another great cat,
and thus, I consider it cheating (much of the hard work is already done).

The aim for the Cheetah project is to create a design that meets the needs of
the majority with the minimum possible special purpose circuits or parts and is
as conservative as possible in achieving that (low risk design features).

The reason for the above is that the FreeEMS project needs a stable hardware
platform as a foundation so that the firmware and associated software can move
forward with more testing, thus bringing more users, testers and developers
on-board as quickly as possible.

Please see [FreeEMS.org](http://freeems.org) for the most up to date information
and links for this project and all of the other aspects of the FreeEMS project.

At the end is a glossary which you may need to reference when reading the goals.

### Goals

These may be too ambitious and can be cut back inline with reality when required.

Core IO specs:

 - 1 FTDI USB (bus powered) + opto couplers for inverter powered laptops 
 - 2 RPM/Position inputs via MAX99xx devices (configurable: VR OR Hall/Opto)
 - 8 standard analogue inputs (AAP & MAT for later, TPS & EGO optional)
 - 6 ignition drives (low current) (min 4)
 - 12 injector drives (high impedance) (min 8)
 - 3 ground "input" connections (CPU/ADC reference, injector ground(s), Acc ground(s))
 - 2 "12V" connections (constant power, switched BRV/key-on signal)
 - 1 overkill fuel pump relay drive (AutoFET)
 - 1 ignition on "12V" key input signal
 - 2 SM load/run external interface pins
 - 2 Ignition polarity interface pins
 - 6 (or more) ground output pins for (external MAP, TPS, IAT, CHT, MAT, CAS)
 - 2 RPM/Position input grounds for VR use
 - 2 switched 5V outputs for TPS and external MAP

50 core connections, 18 high current, 32 low current.
Perhaps add 11 more ground pins and bring each injector driver in and out.
That would add up to 29 high current and 32 low current (61).
Perhaps route USB pins out through connector too, adding 5 or so more.

Possibly separate configuration connections from main connector stuff in some way.

Fuel injector and ignition drive detail:

 - Both ignition drivers and 6 injector drivers connected in parallel use upto 6 total
 - T2-7 feed both ignition drivers and the first 6 injector drivers via 1k resistors
 - B0-5 feed the first 6 injector drivers via unpopulated 1k resistor footprints
 - Easy to swap resistors from T channels to B channels for bit bang injector control
 - 12 high Z injector drivers (AutoFET + resistor)
 - OR 12 low Z injector drivers (darlington + LM1949)
 - IF low Z, then easy to bridge chip out and install autofets
 - 6 FET driver style low current ignition outputs
 - Ignition outputs configurable in polarity (invert/direct)
 - Ignition outputs default polarity for 12V high = dwell
 - Ignition outputs polarity override through connector
 - Ignition outputs configurable in voltage (5V/~12V)

Spare IO specs:

 - 8 PWM outputs with AutoFETs (usable as GP inputs too?)
 - 8 spare ADC inputs (configurable as thermistor or GP)
 - 8 low level digital GPIO channels (bullet proof)
 - All spare CPU pins on internal headers
 - At least one SPI header
 - At least one CAN header
 - At least one I2C header
 - At least one SCI header

24 spare connections, 8 high current, 16 low current.
Perhaps add 8 more ground pins and bring each PWM driver in and out.
That would add up to 16 high current and 16 low current (32).

Physical specs:

 - 160x100 eurocard PCB (2 layer for easy debug)
 - Single board design with connector(s) for up to ~100 pins
 - All externally exposed CPU pins buffered/protected
 - All high current devices TO220 staggered leg format
 - All high current drivers with isolated grounds
 - All other componentry SMD (large and common as possible)
 - Proper provision for mounting reliably to a case
 - Provision for mounting a smaller board above or below

Other specs:

 - Keep sub-circuits grouped and outline with silkscreen
 - Reduce component count from 400 to 200 and/or increase features in same board size
 - On board MAP and AAP sensors, both barbed (dual footprint?)
 - 5v outputs for MAP & TPS each protected with RXEF040 polyfuse (0.4A load, 0.8A trip)
 - CPU regulator always connected to power
 - ADC/Acc regulator enable pin controlled by CPU
 - CEL error light on SM load/run pin
 - SM load/run pins on a header and through the connector
 - LEDs on RPM, ignition, injection, FP, main Power, Acc Power, USB TX/RX
 - External MAP sensor connection
 - Weak (~100k) pull downs on core pins (FP, ignition, injector, Acc output)
 - Vias with sufficient pad to solder to
 - Light PCB colour scheme for easy debug during dev
 - Test points on all key connections
 - Independent filtering of battery reference
 - Physical separation of ADC/CPU from injection/PWM stuff
 - Star connections from regulator(s) to CPU
 - Optimised clock circuit as per manual
 - Optimised power CPU power filtering as per manual
 - Optimised ground planes under CPU as per manual
 - 144 pin CPU IF space constraints aren't hit with 112 pin CPU
 - 8 extra ADC inputs in spare if 144 pin CPU is used
 - BDM header (standard)

Nice to haves:

 - Prototyping region           - unlikely (only if spare space is available)
 - VR3/RPM3 (VVT/NDCAS)         - optional (most won't want/need this)
 - 3-wire PWM Idle drive        - optional (only Toyota and BMW)
 - RTC chip on board            - optional (requires dual regulators + key-on input)
 - Knock sensing interface      - dream    (only really useful for mild setups)
 - EGT thermocouple driver      - dream    (requires stable precision 12v supply)
 - SDCARD reader/writer slot    - dream    (requires dual regulators + key-on input)
 - Stepper driver chip on board - dream    (niche/minority requirement)
 - USB power up for board       - dream    (of limited use (turn on the key))
 - LEDs on all digital GPIO     - dream    (good idea, thanks Abe)

### Coarse Layout

With our maximum compliment of drivers by the above specs we would have 24 to220s:

 - 12 high z AutoFET injector drivers
 - 1 fuel pump AutoFET relay driver
 - 2 5V LDO voltage regulators
 - 8 PWM AutoFET general purpose drivers
 - 1 inverted PWM AutoFET 3 wire driver

This fits onto both sides of a 160mm long board perfectly but tightly. If mounting
holes are requires to be placed at closer spacings then we can reduce the number of
injector drivers by as much as four and the number of PWM drivers by as much as four.
Assuming four corner mounting holes, it probably makes sense to drop the four low
resolution PWM pins and add four extra mounting holes, plus two more that we can
probably fit anyway. That would give us 40mm centres on the holes and good rigity.

Board and case wise:

 - Front end = end facing the user
 - Back end = end facing the loom

For a coarse board layout I propose that the following be done:

 - Regulators at the front end of the board
 - Fuel pump relay drive next to regulators
 - PWM drivers at the back end of the board
 - Injector drivers all of one side of board
 - PWM, FP relay drive and regulators on other
 - Analogue conditioning 2/3 width of back of board
 - Analogue conditioning most of the way between back & cpu
 - Ignition low level drivers 1/3 width of back of board
 - RPM circuits between ignition drivers and cpu
 - Power supply filtration adjacent to regulators at front of board
 - USB mini, opto couplers, FTDI at front of board next to injector drivers
 - SD card slot at front of board between comms and power filtration
 - CPU centre and as close to SD/comms/power supply as possible
 - CPU clock circuits adjacent to the CPU where space is available
 - MAP and AAP, one on each side of CPU nearer to front of board
 - MAP and AAP, both taken to bulkhead fittings at back of board

This layout keeps high current and high noise stuff down each side of the board,
the CPU as far away from the majority of the noise as possible, and the paths of
sensitive signals as short as possible.

### Component Selection

A list of parts with purposes:

 - MC9S12XDP512??? - 112 pin XDP512 CPU - 91 IO pins
 - LM2937? - constant 5V LDO auto grade reg with 2mA quiescent current
 - LM2941? - switchable 5V LDO auto grade reg
 - FT232RL - UART to USB converter
 - RXEF0?0 - polyfuses for 5V external supplies
 - MAX99?? - VR/Hall/Optical input conditioners
 - MPXA6400AP - 20-400kPa MAP sensor SOP footprint, 1369-01 case preferred
 - MPXAZ6115AP - 15-115kPa AAP sensor SOP footprint, 1369-01 case preferred
 - VNP10N07 - Fully protected logic level FET or equivalent
 - HC86? XOR - Ignition output flexibility device
 - ? 12+V FET Driver - Ignitor low current drives
 - USB mini - comms connection, need std footprint
 - AND/XNOR - single gate for combining the extra MAX output with RPM2
 - Inverter - for 3 wire PWM second driver, could use XNOR for this too

More detailed parts to come...

Resistors, capacitors, diodes, headers and connectors to be decided later.

### History

In the beginning there was DFH, a circuit and PCB design by Jared Harvey that
was intended to use the TA card as a plug-in CPU module and be primarily of
through-hole construction. DFH reached a state of near completion at which point
the only remaining thing to do was finalise pin outs on the CPU, a task that
still remains to be done.

After some time of inactivity the DFH project was forked by Marcos Chaparro and
the Puma project began. Thirty seven spin 1 Puma boards were printed, however
due to a lack of accurate and complete documentation and an overly busy main
developer who can't provide enough support, to this day, only one has run an
engine.

In the mean time, the Puma project took a change in direction in several key
areas which I deemed too risky to be the cornerstone of our hardware experience.
Once this became apparent, I started to make notes on what to fix, improve,
change and add from the Puma spin 1 design, and the Cheetah project will be
result of those musings.

### Credits

This project would not be possible without the hard work of a large number of
people. In particular the following deserve to be given credit for their work:

- Jared Harvey - [DFH](https://github.com/jharvey/DFH_FreeEMS_1.0_hardware)
- Marcos Chaparro - [Puma](https://github.com/nitrousnrg/puma)
- Various others who contributed to both of the above projects and this one

### License

The license requires that all documentation from upstream projects be included
with this project. I have met that requirement by keeping carbon copies of both
the DFH project and the Puma project available in the same github account as
this project. This has been done because I take version control very seriously.
Both repositories have been polluted with various binary and temporary files and
both have become large. Furthermore the commit history of those projects is
largely not relevant to the Cheetah project. My forks are linked here:

- [DFH fork](https://github.com/fredcooke/DFH)
- [Puma fork](https://github.com/fredcooke/puma)

### Glossary

 - SMD Surface Mount Device
 - THD Through Hole Device
 - PCB Printed Circuit Board
 - DFH Defacto FreeEMS Hardware
 - TA Technological Arts
 - AutoFET - logic level temperature, current, voltage, static protected FET
 - FET Field Effect Transistor
 - LED Light Emitting Diode
 - SM Serial Monitor
 - USB Universal Serial Bus
 - RPM Revolutions Per Minute
 - BRV Battery Reference Voltage
 - MAP Manifold Absolute Pressure
 - AAP Atmospheric Absolute Pressure
 - TPS Throttle Position Sensor
 - IAT Inlet Air Temperature
 - CHT Coolant/Head Temperature
 - MAT Manifold Air Temperature
 - EGO Exhaust Gas Oxygen
 - CAS Cam Angle Sensor
 - EGT Exhaust Gas Temperature
 - RTC Real Time Clock
 - ADC Analogue to Digital Converter
 - Acc Accessory
 - Aux Auxiliary
 - CPU Central Processing Unit
 - I2C Inter-Integrated Circuit
 - SPI Serial Peripheral Interface
 - SCI Serial Communication Interface (UART)
 - UART Universal Asynchronous Receiver/Transmitter
 - CAN Controller Area Network
 - VR Variable Reluctance
 - CEL Check Engine Light
 - FP Fuel Pump (relay drive)
 - TX Transmit
 - RX Receive
 - Vias - Copper coated holes through a PCB
 - TO220 Transistor Outline 220, TH power devices
 - PWM Pulse Width Modulation
 - GP General Purpose
 - GPIO General Purpose Input/Output
 - low Z - Low impedance injector
 - high Z - High impedance injector
 - Knock - Pinging/Pinking/Rattling/Detonation/Pre-Ignition/Knock

### Footnote

Please post any and all feedback on the schematics, layout and documentation, 
even subtle and minor stuff, to [the forum](http://forum.diyefi.org).

Thanks for playing with FreeEMS :-)

Good luck and regards,

Fred.

