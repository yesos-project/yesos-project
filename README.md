# YesOS

A bare-metal operating system for 64-bit RISC-V (RV64GCV), written from
scratch: no Linux, no libc, no binary blobs. C, Rust `no_std` and assembly,
inspired by TempleOS.

The goal is security you can check, not just trust:

- **Proven memory protection.** The PMP is locked at boot, and every build
  asks the Z3 solver to prove that the layout matches the policy (W^X,
  immutable code, a NULL guard, nothing left to M-mode's default allow).
  If it cannot be proven, there is no image.
- **Untrusted code on isolated cores.** Apps run in PMP "cells" on their own
  harts, behind an instruction verifier and a watchdog.
- **A key that never touches memory.** The vault key lives only in vector
  registers of a dedicated "key hart".
- **Tests for everything,** run under QEMU.

## Repositories

| Repository | What it is |
|---|---|
| [yesos-core](https://github.com/yesos-project/yesos-core) | The shared kernel: boot, Z3-proven PMP, cells, key hart, RedSea filesystem, encrypted vault, HolyQ JIT, DNS engine. Start here. |
| [yesos-nas](https://github.com/yesos-project/yesos-nas) | Network storage: HTTPS panel (TLS 1.3 in the kernel), WebDAV with accounts, Noise NK channel, snapshots, encrypted replication. |
| [yesos-security](https://github.com/yesos-project/yesos-security) | An invisible bridge firewall between the ONT and the router. |
| [yesos-desktop](https://github.com/yesos-project/yesos-desktop) | The desktop, built one feature at a time (today: the core and its console). |

## Status

YesOS runs **under QEMU only**. Real boards (Orange Pi RV2, VisionFive 2)
still need storage and network drivers. Every README has an "Honest limits"
section.

Built with heavy AI assistance (Claude); the design, priorities and review
are the maintainer's.

## Support

Donations pay for development time and for real RISC-V boards:
[Liberapay](https://liberapay.com/yesos-project/donate).

MIT licensed. In memory of Terry A. Davis (1969–2018).
