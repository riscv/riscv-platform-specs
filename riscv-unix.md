# RISC-V Unix-class Platform Specification

# Copyright and license information

This RISC-V Unix-class platform specification is

 &copy; 2017 Krste Asanovic <krste@sifive.com>

 &copy; 2017 Palmer Dabbelt <palmer@sifive.com>

 &copy; 2017 Andrew Waterman <andrew@sifive.com>

It is licensed under the Creative Commons Attribution 4.0 International
License (CC-BY 4.0).  The full license text is available at
https://creativecommons.org/licenses/by/4.0/.

# Unix-class Platform Specification

This Unix-class platform specification is incomplete.  Currently, it only
lists the constraints on a RISC-V implementation beyond those in the
architecture manual.  It only describes the RV64 Unix platform; a future
version of this specification will describe the RV32 Unix platform.

RISC-V Unix-class systems implement the RV64GC ISA with supervisor mode and
the Sv39 page-based virtual-memory scheme.  Systems may support additional ISA
extensions, but if these extensions add user-visible architectural state, they
must be initially disabled.  Systems that support Sv48 must support Sv39,
systems that support Sv57 must support Sv48, and so forth.

Within main-memory regions, aligned instruction fetch must be atomic, up to
the smaller of ILEN and XLEN bits.  In particular, if an aligned 4-byte word
is stored with the `sw` instruction, then any processor attempts to execute
that word, the processor either fetches the newly stored word, or some
previous value stored to that location.  (That is, the fetched instruction is
not an unpredictable value, nor is it a hybrid of the bytes of the old and new
values.)

Unless otherwise specified by a given I/O device,
I/O regions are at least point-to-point strongly ordered.
All devices attached to a given PCIe root complex are on the same ordered
channel (numbered 2 or above), though different root complexes might not
be on the same ordering channel.

On RV64I-based Unix-class systems the negative virtual addresses are
reserved for the kernel.

External devices (DMA engines, the debug unit, non RISC-V cores, etc)
that are visible to RISC-V harts must appear as coherent agents, just
like any RISC-V hart would.  If additional ordering constraints are
necessary for a device to function, those will be provide by a
device-specific mechanism.
