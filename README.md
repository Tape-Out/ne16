# ne16

PULP's NE16, a 16-input-channel neural engine, taken as a black box.

![maturity](https://img.shields.io/badge/maturity-planned-lightgrey) ![license](https://img.shields.io/badge/license-MIT%20OR%20Apache--2.0%20OR%20MulanPSL--2.0-blue) ![upstream](https://img.shields.io/badge/upstream-SHL--0.51-lightgrey)

Part of the [Tape-Out](https://github.com/Tape-Out) IP library, wired up by
[`xirang`](https://github.com/Tape-Out/xirang). Five submodules under `third_party/`;
nothing in them is modified.

## What this repository adds

Eight knobs, every one of them a real parameter of `ne16_top_wrap`:

| knob | domain | what it sizes |
| :-- | :-- | :-- |
| `tpIn` / `tpOut` | 8, 16, 32 / 16, 32, 64 | input and output elements per cycle |
| `bw` | 288, 576 | external memory bandwidth |
| `nCores` / `nContext` | 4, 8, 9, 16 / 1, 2, 4 | cores served, hardware contexts |
| `cnt` / `id` / `mp` | 8, 16, 32 / 8, 16, 32 / 4, 9 | counter width, id width, memory ports |

**The domains were measured, not guessed.** Each candidate value was elaborated on its
own; `mp = 16` and `bw = 144` do not elaborate, so they are not in the domain. The
matrix runs 18 points and every one of them elaborates.

## Versions are pinned, and they have to be

The four sibling repositories are pinned to what NE16's own `Bender.yml` asks for:
`hwpe-stream 1.6.4`, `hwpe-ctrl 1.6.2`, `hci 1.0.6`. Taking newer ones breaks: current
`hci` expects interface parameters that NE16 does not supply, and current `hwpe-stream`
dropped the `test_mode_i` port that `hci 1.0.6` still drives. That is 35 elaboration
errors from a version skew alone.

## Limits

The control port speaks HWPE's peripheral protocol and the memory port speaks HCI's
TCDM; neither is AXI, so a bridge belongs outside. Only the generic technology cells are
compiled — `tech_cells_generic/src/fpga` holds Xilinx primitives (`BUFGCE`) that have no
place in an ASIC path. Upstream ships no runnable testbench, so this version has the
per-point elaboration and the declaration receipt, and nothing else.

## License

This repository: 任选其一 [MIT](LICENSE-MIT) · [Apache 2.0](LICENSE-APACHE) ·
[木兰宽松许可证 第2版](LICENSE-MULAN). Everything under `third_party/` stays
**SHL-0.51**, which is not an OSI-approved license.
