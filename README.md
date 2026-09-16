# VHDL Digital Design

A collection of VHDL modules and testbenches covering behavioral, structural and dataflow digital design.

![VHDL](https://img.shields.io/badge/language-VHDL-6f42c1)
![Simulator](https://img.shields.io/badge/simulator-ModelSim-0a7bbb)

## Overview

Each module lives in its own folder with its sources under `SRC/`. Most modules come with a
`tb_*.vhdl` testbench, many include ModelSim project files (`sim.mpf`), and several include
screenshots of their simulation waveforms in `sim_photo/`.

The designs are grouped by modeling style:

- **Behavioral** — counters, registers, shift registers, RAM/ROM blocks and finite state machines
- **Structural** — adders, subtractors, comparators, latches and flip-flops built from smaller components
- **Dataflow** — a tri-state buffer
- **`_prj`** — the data path of a 4-bit multiplier

## Modules

### Behavioral (`Behavioral/`)

| Category | Modules |
| --- | --- |
| Counters | `counter_4_bit`, `counter_n_bit`, `upDown_counter_n_bit` |
| Flip-flops and registers | `d_flip_flop`, `register_4_bit`, `register_n_bit`, `synch_register_4bit` |
| Shift registers | `left_PIPO_4_bit`, `left_PIPO_n_bit`, `right_PIPO_4_bit`, `right_PIPO_n_bit`, `left_PISO_n_bit`, `right_PISO_n_bit`, `left_SIPO_n_bit`, `right_SIPO_n_bit`, `left_right_shift_reg_4_bit` |
| Memory | `block_ram`, `block_ram_bidirectional`, `block_ram_bidirectional_general` (generic address/data width, `inout` data bus), `distributed_ram`, `block_rom`, `distributed_rom`, `dynamic_rom` |
| State machines | `state_machine/seq_det_1011_mealy`, `state_machine/seq_det_1011_moore` — "1011" sequence detectors |

### Structural (`Structural/`)

| Category | Modules |
| --- | --- |
| Adders | `full_adder`, `full_adder_4_bit`, `adder_n_bit` (generic width), `adder_n_bit_packege` |
| Subtractors | `full_subtractor_1_bit`, `full_subtractor_4_bit`, `full_subtractor_4_bit_with_full_adder_1_bit` |
| Adder / subtractor | `full_sub_and_adder_4_bit` |
| Comparators | `bit_comparator`, `comparator_4_bit_generate`, `comparator_n_bit_generate` (built with `generate`) |
| Storage | `d_latch`, `d_flip_flop` (from D latches), `register_4_bit` |
| Multiplexers | `mux2_1` |

### Dataflow (`dataflow/`)

| Module | Description |
| --- | --- |
| `tri_state_buffer` | Passes the input when enabled and drives `'Z'` otherwise |

### Project (`_prj/`)

| Module | Description |
| --- | --- |
| `Multiplier_4_bit` | Data path of a 4-bit multiplier (`data_path.vhdl`) built from an n-bit adder, registers and shift registers. The folder contains the data path only, without a controller or testbench. |

## Project structure

```
vhdl-digital-design/
├── Behavioral/
│   ├── block_ram/
│   │   ├── SRC/
│   │   │   ├── block_ram.vhdl
│   │   │   └── tb_block_ram.vhdl
│   │   └── sim.mpf              # ModelSim project
│   ├── counter_4_bit/
│   │   ├── SRC/
│   │   └── sim_photo/           # waveform screenshot
│   ├── state_machine/
│   │   ├── seq_det_1011_mealy/SRC/
│   │   └── seq_det_1011_moore/SRC/
│   └── ...                      # 24 behavioral module folders in total
├── Structural/
│   ├── full_adder/
│   ├── comparator_n_bit_generate/
│   └── ...                      # 15 structural module folders in total
├── dataflow/
│   └── tri_state_buffer/SRC/
└── _prj/
    └── Multiplier_4_bit/SRC/
```

## Simulating a module

The ModelSim project files in the repository were created with ModelSim (INI version 2020.4),
but any VHDL simulator can compile the sources.

**ModelSim GUI:** open a module's `sim.mpf`, compile the files, and simulate the testbench entity.

**ModelSim command line** (example: 4-bit counter):

```bash
cd Behavioral/counter_4_bit/SRC
vlib work
vcom counter_4_bit.vhdl tb_counter_4_bit.vhdl
vsim test_counter_4_bit
```

Testbench entity names vary by module (for example, `test_counter_4_bit`), so check the
`ENTITY` line in the `tb_*.vhdl` file. Compile a module's component files before the files
that instantiate them. Some modules use the Synopsys `std_logic_unsigned` package, so use a
simulator that provides it (ModelSim does).

## Notes

- `.gitignore` excludes the compiled `work` library. Some ModelSim files (`sim.mpf`, `sim.cr.mti`, `vsim.wlf`) are committed alongside the sources.
- `Structural/register_4_bit` keeps its source at the folder root instead of in `SRC/`.
- In `dataflow/tri_state_buffer`, the architecture is declared `OF ent` instead of `OF tri_state_buffer`, so rename it before compiling.
