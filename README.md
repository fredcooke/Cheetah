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

This section is for the goals, plans, specs, design decisions and reasoning. It
is to be filled out soon!

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

In the mean time, the Puma project took a change in direction in several key areas which I deemed too risky to be the cornerstone of our hardware experience.
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

