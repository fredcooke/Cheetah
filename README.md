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

### Goals

These may be too ambitious and can be cut back inline with reality when required.

Core IO specs:

 - 1 FTDI USB (bus powered) + opto couplers for inverter powered laptops 
 - 2 RPM/Position inputs via MAX99xx devices (configurable: VR OR Hall/Opto)
 - 8 standard analogue inputs (aap/mat for later, tps/o2 optional)
 - 6 ignition drives (low current) (min 4)
 - 12 injector drives (high impedance) (min 8)
 - 3 ground "input" connections (CPU/ADC ref, Inj gnd(s), ACC gnd(s))
 - 3 ~12v connections (Const pwr, switched ADC pwr, switched BRV/Key signal)
 - 1 overkill fuel pump relay drive (autofet)
 - 1 ignition on 12v key input signal
 - 2 SM load/run external interface pins
 - 2 Ignition polarity interface pins
 - 6 (or more) ground output pins for (ext map, tps, iat, cht, mat, CAS)
 - 2 RPM/Position input grounds for VR use
 - 2 switched 5v outputs for TPS and ext MAP

50 core connections, 18 high current, 32 low current.
Perhaps add 11 more ground pins and bring each injector driver in and out.
That would add up to 29 high current and 32 low current (61).
Perhaps route USB pins out through connector too, adding 5 or so more.

Fuel injector and ignition drive detail:

 - Easy jumpering of T2-7 to either Inj or Ign channels (temporary)
 - 12 high Z injector drivers (autofet + resistor)
 - OR 12 low Z injector drivers (darlington + lm1949)
 - IF low Z, then easy to bridge chip out and install autofets
 - 6 FET driver style low current ignition outputs
 - Ignition outputs configurable in polarity (invert/direct)
 - Ignition outputs default polarity for 12v high = dwell
 - Ignition outputs polarity override through connector
 - Ignition outputs configurable in voltage (5V/~12V)
 - Ignition outputs through on board drivers (MAYBE)

Spare IO specs:

 - 8 PWM outputs with autofets (usable as GP inputs too?)
 - 8 Spare ADC inputs (configurable as thermistor or GP)
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
 - All high current devices to220 staggered leg format
 - All high current drivers with isolated grounds
 - All other componentry SMD (large and common as possible)

Other specs:

 - CPU regulator always connected to power
 - ADC/ACC regulator enable pin controlled by CPU
 - CEL error light on SM load/run pin
 - SM load/run pins on a header and through the connector
 - LEDs on RPM, Ign, Inj, FP, Main Pwr, Aux Pwr, USB TX/RX
 - External MAP sensor connection
 - Vias with sufficient pad to solder to
 - Light PCB colour scheme for easy debug during dev
 - Test points on all key connections
 - Independent filtering of battery reference
 - Phyiscal separation of ADC/CPU from Inj/PWM stuff
 - Star connections from regulator(s) to CPU
 - Optimised clock circuit as per manual
 - Optimised power CPU power filtering as per manual
 - 144 pin CPU IF space constraints aren't hit with 112 pin CPU
 - 8 extra ADC inputs in spare if 144 pin CPU is used

Nice to haves:

 - Prototyping region - if spare space
 - RTC chip on board - optional
 - Knock sensing interface - dream
 - EGT thermocouple driver - dream
 - SDCARD reader/writer slot - dream
 - Stepper driver chip on board - dream

TODO: Add design decisions and reasoning in clear language to go with list.

### History

In the beginning there was DFH, a circuit and PCB design by Jared Harvey that
was intended to use the TA card as a plug-in CPU module and be primarily of
through-hole construction. DFH reached a state of near completion at which point
the only remaining thing to do was finalise pin outs on the CPU, a task that
still remains to be done.

After some time of inactivity the DFH project was forked by Marcos Chaparo and
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
- Marcos Chaparo - [Puma](https://github.com/nitrousnrg/puma)
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

### Footnote

Please post any and all feedback on the schematics, layout and documentation, 
even subtle and minor stuff, to [the forum](http://forum.diyefi.org).

Thanks for playing with FreeEMS :-)

Good luck and regards,

Fred.

