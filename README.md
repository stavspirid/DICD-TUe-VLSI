# 8-bit Brent-Kung Adder on Cadence Virtuoso
VLSI implementation of optimized 8-bit Brent-Kung parallel prefix adders in 45nm CMOS technology.
### Authors
Stavros Spyridopoulos ([stavspirid@gmail.com](mailto:stavspirid@gmail.com))

Quinten Kustermans ([quinten.kustermans@home.nl](mailto:quinten.kustermans@home.nl))
## Basic Components

### Logic Gates

| Gate         | Schematic                                                                                                | Transistors |
| ------------ | -------------------------------------------------------------------------------------------------------- | ----------- |
| **Inverter** | <img src="https://raw.githubusercontent.com/stavspirid/DICD-TUe-VLSI/main/resources/inverter_sch.png" height="200"> | 2           |
| **NOR**      | <img src="https://raw.githubusercontent.com/stavspirid/DICD-TUe-VLSI/main/resources/nor_sch.png" height="200">           | 4           |
| **NAND**     | <img src="https://raw.githubusercontent.com/stavspirid/DICD-TUe-VLSI/main/resources/nand_sch.png" height="200">         | 4           |

**NMOS**: 90nm width | **PMOS**: 180nm width (2:1 ratio for balanced timing)

### XOR Gate

Selected implementation: `(A + B) + (A ∗ B)` using 3 gates (2×NOR, 1×AND)

## Brent-Kung Cells

### Unoptimized Cells

| Cell       | Schematic                                                                                       | Transistors |
| ---------- | ----------------------------------------------------------------------------------------------- | ----------- |
| **Buffer** | <img src="https://raw.githubusercontent.com/stavspirid/DICD-TUe-VLSI/main/resources/buffer_un_sch.png" height="200"> | 8           |
| **Black**  | <img src="https://raw.githubusercontent.com/stavspirid/DICD-TUe-VLSI/main/resources/black_un_sch.png" height="200">   | 18          |
| **Gray**   | <img src="https://raw.githubusercontent.com/stavspirid/DICD-TUe-VLSI/main/resources/gray_un_sch.png" height="200">     | 12          |

### Optimized Cells (Even/Odd Variants)
Compound gate technique reduces transistor count:
- **Buffer**: 4 transistors (50% reduction)
- **Black**: 14 transistors (22% reduction)
- **Gray**: 10 transistors (17% reduction)

## Complete Adders

### Architecture

<img src="https://raw.githubusercontent.com/stavspirid/DICD-TUe-VLSI/main/resources/brent-kung_top_sch.png" height="300">

### Implementations

| Version         | Schematic                                                                                                  | Transistors | Area (μm²) |
| --------------- | ---------------------------------------------------------------------------------------------------------- | ----------- | ---------- |
| **Unoptimized** | <img src="https://raw.githubusercontent.com/stavspirid/DICD-TUe-VLSI/main/resources/adder_un_sch.png" width="400"> | 228         | 130.87     |
| **Optimized**   | <img src="https://raw.githubusercontent.com/stavspirid/DICD-TUe-VLSI/main/resources/adder_op_sch.png" width="400">          | 160         | 109.13     |

**Improvement**: 30% fewer transistors, 16.6% smaller area

### Boost Buffers

<img src="https://raw.githubusercontent.com/stavspirid/DICD-TUe-VLSI/main/resources/boost_buffer_sch.png" height="270">

2-stage inverter chains improve timing:

- Rise time: 1115ps → 84ps
- Fall time: 743ps → 97ps

## Physical Layout

### Gate Layouts

| NAND                                                                                                    | NOR                                                                                              |
| ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| <img src="https://raw.githubusercontent.com/stavspirid/DICD-TUe-VLSI/main/resources/nand_layout.png" height="300"> | <img src="https://raw.githubusercontent.com/stavspirid/DICD-TUe-VLSI/main/resources/nor_layout.png" height="300"> |

### Complete Layouts

| Unoptimized (14.41×9.09 μm)                                                                                | Optimized (14.60×7.48 μm)                                                                              |
| ---------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| <img src="https://raw.githubusercontent.com/stavspirid/DICD-TUe-VLSI/main/resources/unopt_layout.png" height="500"> | <img src="https://raw.githubusercontent.com/stavspirid/DICD-TUe-VLSI/main/resources/opt_layout.png" height="500"> |

**Layout Strategy**: Lane-based design with hierarchical metal layers (M1-M5)

## Performance Results

| Metric           | Unoptimized | Optimized  | Improvement |
| ---------------- | ----------- | ---------- | ----------- |
| **Delay**        | 1.065ns     | 1.044ns    | 2.0%        |
| **Power**        | 136.4μW     | 140.1μW    | -2.7%       |
| **Area**         | 130.87μm²   | 109.13μm²  | 16.6%       |