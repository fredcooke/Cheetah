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

Fuel injector and ignition drive detail:

 - Easy jumpering of T2-7 to either injection or ignition channels (temporary)
 - 12 high Z injector drivers (AutoFET + resistor)
 - OR 12 low Z injector drivers (darlington + LM1949)
 - IF low Z, then easy to bridge chip out and install autofets
 - 6 FET driver style low current ignition outputs
 - Ignition outputs configurable in polarity (invert/direct)
 - Ignition outputs default polarity for 12V high = dwell
 - Ignition outputs polarity override through connector
 - Ignition outputs configurable in voltage (5V/~12V)
 - Ignition outputs through on board drivers (MAYBE)

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

Other specs:

 - On board MAP and AAP sensors, both barbed (dual footprint?)
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
 - Reverse polarity protection (FET crowbar? sshottky diodes?)
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
 - IAP Intercooler Pressure     - dream    (non core, does not belong)
 - RTC chip on board            - optional (requires dual regulators + key-on input)
 - Knock sensing interface      - dream    (only really useful for mild setups)
 - EGT thermocouple driver      - dream    (requires stable precision 12v supply)
 - SDCARD reader/writer slot    - dream    (requires dual regulators + key-on input)
 - Stepper driver chip on board - dream    (niche/minority requirement)
 - USB power up for board       - dream    (of limited use (turn on the key))
 - LEDs on all digital GPIO     - dream    (good idea, thanks Abe)

TODO: Add design decisions and reasoning in clear language to go with list.

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

