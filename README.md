# CoralNPU RVV Core

RISC-V Vector (RVV 1.0) execution engine from the [Google CoralNPU](https://github.com/google-coral/coralnpu) project.

## Features

- **ISA**: RISC-V Vector Extension 1.0
- **Base extension**: Zve32x (32-bit integer vector)
- **Optional**: Zve32f (32-bit floating-point vector)
- **VLEN**: Configurable 128/256/512/1024-bit vector registers
- **SEW**: 8/16/32/64-bit element widths
- **LMUL**: 1/8, 1/4, 1/2, 1, 2, 4, 8 register grouping
- **Dispatch**: Multi-lane (default 4 lanes), 3-decode/2-dispatch pipeline
- **Execution units**: 2 ALU, 2 MUL, 1 DIV, 1 PMT/RDT, 2 LSU, 2 FMA (optional)
- **Register file**: 32 vector registers (v0-v31)

## Architecture

```
RvvCore (top-level)
├── RvvFrontEnd (instruction decode, vsetvli/vsetivli handling)
│   └── Config state management (LMUL, SEW, VSTART)
└── rvv_backend (execution engine)
    ├── rvv_backend_decode (instruction decode pipeline)
    ├── rvv_backend_dispatch (operand fetch, reservation stations)
    ├── rvv_backend_alu x2 (integer: add/sub/logic/shift/compare/mask)
    ├── rvv_backend_mac_unit x2 (multiply-accumulate)
    ├── rvv_backend_div_unit (integer division)
    ├── rvv_backend_pmtrdt_unit (permutation/reduction)
    ├── rvv_backend_fma_wrapper x2 (FP, optional)
    ├── rvv_backend_lsu x2 (vector load/store)
    └── ROB (reorder buffer, 8 entries)
```

## Supported Operations

- **Arithmetic**: VADD, VSUB, VSADD, VSSUB, VMIN, VMAX, widening/narrowing variants
- **Logical**: VAND, VOR, VXOR, shifts (SLL, SRL, SRA)
- **Multiply**: VMUL, VMULH, VMACC, VWMUL variants
- **Compare**: VMSEQ, VMSNE, VMSLT, VMSLTU, VMSLE, VMSGT
- **Mask**: VMAND, VMOR, VMXOR, VMNAND, VMORNOT, etc.
- **Permutation**: VRGATHER, VSLIDEUP, VSLIDEDOWN, VCOMPRESS
- **Reduction**: VREDSUM, VREDAND, VREDOR, VREDXOR, VREDMIN, VREDMAX
- **Load/Store**: Unit-stride, strided, indexed

## Upstream

Extracted from [google-coral/coralnpu](https://github.com/google-coral/coralnpu) `hdl/verilog/rvv`. See `upstream.yaml` for commit tracking.

## License

Apache-2.0 — see [LICENSE](LICENSE).
