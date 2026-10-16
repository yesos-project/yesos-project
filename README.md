<div align="center">

# YesOS

**A bare-metal operating system for 64-bit RISC-V, written from scratch.<br>
Security you can check, not just trust.**

<img alt="RISC-V RV64GCV" src="https://img.shields.io/badge/RISC--V-RV64GCV-283272?logo=riscv&logoColor=white">
<img alt="Machine mode only" src="https://img.shields.io/badge/runs%20in-Machine%20mode-4a5568">
<img alt="PMP proven with Z3" src="https://img.shields.io/badge/PMP-proven%20with%20Z3-6b46c1">
<img alt="No floating point" src="https://img.shields.io/badge/floating%20point-none%2C%20checked-2f855a">
<img alt="License MIT" src="https://img.shields.io/badge/license-MIT-2b6cb0">

<br>

<img src="https://raw.githubusercontent.com/yesos-project/yesos-desktop/main/docs/screenshot.png" width="720" alt="YesOS Desktop: a console window, a file browser and a text viewer">

<sub>YesOS Desktop in QEMU: the real screen, from <code>screendump</code>.</sub>

</div>

---

No Linux, no libc, no binary blobs: C, Rust `no_std` and assembly, inspired
by TempleOS.

- **Proven memory protection.** The PMP is locked at boot, and every build
  asks the Z3 solver to prove that the layout matches the policy (W^X,
  immutable code, a NULL guard, nothing left to M-mode's default allow).
  If it cannot be proven, there is no image.
- **Untrusted code on isolated cores.** Apps run in PMP "cells" on their
  own harts, behind an instruction verifier and a watchdog.
- **A key that never touches memory.** The vault key lives only in vector
  registers of a dedicated "key hart".
- **An integer-only kernel.** No floating-point instruction in any image,
  checked at every build.
- **Only what each product needs.** A product links only the core modules
  it names; NAS and Security refuse, at build time, any symbol of the
  desktop's code.
- **Tests for everything,** run under QEMU: screens compared pixel for
  pixel, sound bit for bit.

## Repositories

| Repository | What it is |
|---|---|
| [yesos-core](https://github.com/yesos-project/yesos-core) | The shared kernel: boot, Z3-proven PMP, cells, key hart, RedSea filesystem, encrypted vault, HolyQ JIT, DNS engine. **Start here.** |
| [yesos-nas](https://github.com/yesos-project/yesos-nas) | Network storage: HTTPS panel (TLS 1.3 in the kernel), WebDAV with accounts, Noise NK channel, snapshots, encrypted replication. |
| [yesos-security](https://github.com/yesos-project/yesos-security) | An invisible bridge firewall between the fibre modem and the router. |
| [yesos-desktop](https://github.com/yesos-project/yesos-desktop) | A desktop of overlapping windows: console, file browser, text viewer, HD Audio, virtio keyboard and tablet. |

Design choices and a comparison with Linux, seL4, Zephyr and TempleOS:
[yesos-core README](https://github.com/yesos-project/yesos-core#2-design-choices).

Experimental work lives in a separate tree and comes to the products only
with tests that measure it. One example: EML, an integer-only log-domain
number library, now accurate to the limit of its format; no product uses it
yet.

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
