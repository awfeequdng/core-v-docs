Optional private Floating Point Unit (FPU)
==========================================

It is possible to extend the core with a private FPU, which is capable
of performing all RISC-V floating-point operations that are defined in
the RV32F ISA extensions. The latency of the individual instructions and
information where they are computed are summarized in Table 3. FP
extensions can be enabled by setting the parameter of the toplevel file
“riscv\_core.sv” to one.

The FPU is divided into three parts:

1. A *simple FPU* of ~10kGE complexity, which computes FP-ADD, FP-SUB
   and FP-casts.

2. An *iterative FP-DIV/SQRT unit* of ~7 kGE complexity, which computes
   FP-DIV/SQRT operations.

3. An *FP-FMA unit* which takes care of all fused operations. This unit
   is currently only supported through a Synopsys Design Ware
   instantiation, or a Xilinx block for FPGA targets.

+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
|   FP-Operation     |   Executed in:     |   Latency     |   Operation                    |   Information                                                                                                               |
+====================+====================+===============+================================+=============================================================================================================================+
| flw                | LSU                | 2             | Loads 32 to FP-RF              | Mapped to lw                                                                                                                |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fsw                | LSU                | 2             | Stores FP-operand to memory    | Mapped to sw                                                                                                                |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fmadd              | FPU                | 3             | rd = rs1 \* rs2 + rs3          |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fmsub              | FPU                | 3             | rd = rs1 \* rs2– rs3           |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fnmadd             | FPU                | 3             | rd = – (rs1 \* rs2+ rs3)       |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fnmsub             | FPU                | 3             | rd = –(rs1 \* rs2 – rs3)       |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fadd.s             | FPU                | 2             | rd = rs1 + rs2                 |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fsub.s             | FPU                | 2             | rd = rs1 – rs2                 |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fmul.s             | FPU                | 2             | rd = rs1 \* rs2                |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fdiv.s             | FPU                | 5 – 8         | rd = rs1 / rs2                 | According to precision specified in CSR see Table 5: Custom CSR to control the precision of FP DIV/SQRT operationsTable 5   |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fsqrt.s            | FPU                | 5 – 8         | rd = sqrt(rs1)                 |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fclass.s           | ALU                | 1             | See specification              |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fmv.s.w            | ALU                | 1             | Move from int-RF to FP-RF      | Mapped to mv                                                                                                                |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fmv.w.s            | ALU                | 1             | Move from FP-RF to int-RF      |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fsgnj.s            | ALU                | 1             | Inserts sign of rs2            |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fsgnjn.s           | ALU                | 1             | Inserts negative sign of rs2   |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fsgnjx.s           | ALU                | 1             | Inserts xor of the two signs   |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| feq.s              | ALU                | 1             | (rs1 == rs2)                   | Reuses integer comparator                                                                                                   |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| flt.s              | ALU                | 1             | (rs1 < rs2)                    |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fle.s              | ALU                | 1             | (rs1 <= rs2)                   |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fmin               | ALU                | 1             | rd = min(rs1, rs2)             |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fmax               | ALU                | 1             | rd = max(rs1, rs2)             |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fcvt.x.w           | FPU                | 2             | Int to FP cast                 |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fcvt.x.wu          | FPU                | 2             | Unsigned int to FP cast        |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fcvt.w.x           | FPU                | 2             | FP to int cast                 |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
| fcvt.wu.x          | FPU                | 2             | FP to unsigned int cast        |                                                                                                                             |
+--------------------+--------------------+---------------+--------------------------------+-----------------------------------------------------------------------------------------------------------------------------+

Table 3: Overview of FP-operations

FP CSR
------

When using floating-point extensions the standard specifies a
floating-point status and control register (fcsr) which contains the
exceptions that occurred since it was last reset and the rounding mode.
fflags and frm can be accessed directly or over fcsr which is mapped to
those two registers.

Since CV32E40P includes an iterative div/sqrt unit, its precision and
latency can be controlled over a custom csr (fprec). This allows faster
division / square-root operations at the lower precision. By default,
the single-precision equivalents are computed with a latency of 8
cycles.

+---------------------------------------------------------+-------------------+----------+-------+---------------------------------------------------------------------------------------+
|   CSR Address                                           |                   |          |       |                                                                                       |
+-------------------+-----------+------------+------------+-------------------+----------+-------+---------------------------------------------------------------------------------------+
|   11:10           |   9:8     |   7:6      |   5:0      |   Hex             | Name     | Acc.  | Description                                                                           |
+===================+===========+============+============+===================+==========+=======+=======================================================================================+
| 00                | 00        | 00         | 00001      | 0x001             | fflags   | R/W   | Floating-point accrued exceptions                                                     |
+-------------------+-----------+------------+------------+-------------------+----------+-------+---------------------------------------------------------------------------------------+
| 00                | 00        | 00         | 00010      | 0x002             | frm      | R/W   | Floating-point dynamic rounding mode                                                  |
+-------------------+-----------+------------+------------+-------------------+----------+-------+---------------------------------------------------------------------------------------+
| 00                | 00        | 00         | 00011      | 0x003             | fcsr     | R/W   | Floating-point control and status register                                            |
+-------------------+-----------+------------+------------+-------------------+----------+-------+---------------------------------------------------------------------------------------+
| 00                | 00        | 00         | 00110      | 0x006             | fprec    | R/W   | Custom flag which controls the precision and latency of the iterative div/sqrt unit   |
+-------------------+-----------+------------+------------+-------------------+----------+-------+---------------------------------------------------------------------------------------+

Table 4: FP related CSRs

+--------------------+----------------------------------------------------------------+---------------+
|   fprec value      |   Precision                                                    |   Latency     |
+====================+================================================================+===============+
| 0                  | Default value: single precision                                | 8             |
+--------------------+----------------------------------------------------------------+---------------+
| 8 – 11             | Computes as many mantissa bits as specified in “fprec value”   | 5             |
+--------------------+----------------------------------------------------------------+---------------+
| 12 – 15            |                                                                | 6             |
+--------------------+----------------------------------------------------------------+---------------+
| 16 – 19            |                                                                | 7             |
+--------------------+----------------------------------------------------------------+---------------+
| 20 – 23            |                                                                | 8             |
+--------------------+----------------------------------------------------------------+---------------+

Table 5: Custom CSR to control the precision of FP DIV/SQRT operations

Floating-point Performance Counters:
------------------------------------

Some specific performance counters have been implemented to profile
FP-kernels.

Some hints on synthesizing the FPU
----------------------------------

The pipeline of the FPU is not balanced but it includes one pipeline
register in front of the *simple FPU* which is intended to be moved in
to the pipeline with automatic retiming commands. The same holds for the
*FP-FMA unit* which contains two pipeline registers (one in front, and
one after the unit).

Optimal performance is only achieved with retiming these two blocks.
This can for example be achieved with the “optimize\_register” command
of the Synopsys Design Compiler.
