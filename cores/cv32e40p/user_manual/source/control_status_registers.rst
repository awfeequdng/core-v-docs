Control and Status Registers
============================

CV32E40P does not implement all control and status registers specified in
the RISC-V privileged specifications, but is limited to the registers
that were needed for the PULP system. The reason for this is that we
wanted to keep the footprint of the core as low as possible and avoid
any overhead that we do not explicitly need.

+---------------------------------------------------------+-------------------+-------------+-------+------------------------------------------+
|   CSR Address                                           |   Hex             |   Name      |  Acc. |   Description                            |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
|   11:10           |   9:8     |   7:6      |   5:0      |                   |             |       |                                          |
+===================+===========+============+============+===================+=============+=======+==========================================+
| 00                | 11        | 00         | 000000     | 0x300             | MSTATUS     | R/W   | Machine Status                           |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 00                | 11        | 00         | 000100     | 0x304             | MIE         | R/W   | Machine Interrupt Enable Register        |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 00                | 11        | 00         | 000101     | 0x305             | MTVEC       | R     | Machine Trap-Vector Base Address         |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 00                | 11        | 01         | 000001     | 0x341             | MEPC        | R/W   | Machine Exception Program Counter        |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 00                | 11        | 01         | 000010     | 0x342             | MCAUSE      | R/W   | Machine Trap Cause                       |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 00                | 11        | 01         | 000100     | 0x344             | MIP         | R     | Machine Interrupt Pending Register       |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 00         | 0xxxxx     | 0x780-0x79F       | PCCRs       | R/W   | Performance Counter Counter Registers    |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 11         | 010000     | 0x7D0             | MIEX        | R/W   | Machine Interrupt Enable Ext Register    |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 11         | 010001     | 0x7D1             | MTVECX      | R     | Machine Trap-Vector Base Address Ext     |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 11         | 010010     | 0x7D2             | MIPX        | R     | Machine Interrupt Pending Ext Register   |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 10         | 100000     | 0x7E0, 0xCC0      | PCER        | R/W   | Performance Counter Enable               |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 10         | 100001     | 0x7E1, 0xCC1      | PCMR        | R/W   | Performance Counter Mode                 |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 10         | 110xxx     | 0x7B0-0x7B7       | HWLP        | R/W   | Hardware Loop Registers                  |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 11                | 00        | 00         | 010000     | 0xC10             | PRIVLV      | R     | Privilege Level                          |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 00                | 00        | 00         | 010100     | 0x014             | UHARTID     | R     | Hardware Thread ID                       |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 11                | 11        | 00         | 010100     | 0xF14             | MHARTID     | R     | Hardware Thread ID                       |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 10         | 100000     | 0x7A0             | TSELECT     | R     | Trigger Select Register                  |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 10         | 100001     | 0x7A1             | TDATA1      | R/W   | Trigger Data Register 1                  |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 10         | 100010     | 0x7A2             | TDATA2      | R     | Trigger Data Register 2                  |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 10         | 100011     | 0x7A3             | TDATA3      | R     | Trigger Data Register 3                  |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 10         | 101000     | 0x7A8             | MCONTEXT    | R/W   | Machine Context Register                 |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 10         | 101010     | 0x7AA             | SCONTEXT    | R/W   | Machine Context Register                 |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 10         | 110001     | 0x7AA             | DPC         | R/W   | Debug PC                                 |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 10         | 110000     | 0x7B0             | DCSR        | R/W   | Debug Control and Status                 |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 10         | 110001     | 0x7B1             | DPC         | R/W   | Debug PC                                 |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 10         | 110010     | 0x7B2             | DSCRATCH0   | R/W   | Debug Scratch Register 0                 |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+
| 01                | 11        | 10         | 110011     | 0x7B3             | DSCRATCH1   | R/W   | Debug Scratch Register 1                 |
+-------------------+-----------+------------+------------+-------------------+-------------+-------+------------------------------------------+

Table 7: Control and Status Register Map

Machine Status (MSTATUS)
------------------------

CSR Address: 0x300

Reset Value: 0x0000_1800

+-------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   Bit #     |   R/W     |   Description                                                                                                                                                                                                                                                       |
+=============+===========+=====================================================================================================================================================================================================================================================================+
| 12:11       | R/W       | **MPP:** Machine Previous Priviledge mode, hardwired to 11 when the user mode is not enabled.                                                                                                                                                                       |
+-------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 7           | R/W       | **Previous Machine Interrupt Enable:** When an exception is encountered, MPIE will be set to MIE. When the mret instruction is executed, the value of MPIE will be stored to MIE.                                                                                   |
+-------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 4           | R/W       | **Previous User Interrupt Enable:** If user mode is enabled, when an exception is encountered, UPIE will be set to UIE. When the uret instruction is executed, the value of UPIE will be stored to UIE. *Note that PULP/issimo does not support USER interrupts.*   |
+-------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 3           | R/W       | **Machine Interrupt Enable:** If you want to enable interrupt handling in your exception handler, set the Interrupt Enable MIE to 1’b1 inside your handler code.                                                                                                    |
+-------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 0           | R/W       | **User Interrupt Enable:** If you want to enable user level interrupt handling in your exception handler, set the Interrupt Enable UIE to 1’b1 inside your handler code. *Note that PULP/issimo does not support USER interrupts.*                                  |
+-------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

User Status (USTATUS)
---------------------

CSR Address: 0x000

Reset Value: 0x0000_0000

Detailed:

+-------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   Bit #     |   R/W     |   Description                                                                                                                                                                                                                                                       |
+=============+===========+=====================================================================================================================================================================================================================================================================+
| 4           | R/W       | **Previous User Interrupt Enable:** If user mode is enabled, when an exception is encountered, UPIE will be set to UIE. When the uret instruction is executed, the value of UPIE will be stored to UIE. *Note that PULP/issimo does not support USER interrupts.*   |
+-------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 0           | R/W       | **User Interrupt Enable:** If you want to enable user level interrupt handling in your exception handler, set the Interrupt Enable UIE to 1’b1 inside your handler code. *Note that PULP/issimo does not support USER interrupts.*                                  |
+-------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Machine Interrupt Enable Register (MIE)
---------------------------------------

CSR Address: 0x304

Reset Value: 0x0000_0000

Detailed:

+-------------+-----------+------------------------------------------------------------------------------------------+
|   Bit #     |   R/W     |   Description                                                                            |
+=============+===========+==========================================================================================+
| 30:16       | R/W       | Machine Fast Interrupt Enables: Set bit x+16 to enable fast interrupt irq\_fast\_i[x].   |
+-------------+-----------+------------------------------------------------------------------------------------------+
| 11          | R/W       | **Machine External Interrupt Enable (MEIE)**: If set, irq\_external\_i is enabled.       |
+-------------+-----------+------------------------------------------------------------------------------------------+
| 7           | R/W       | **Machine Timer Interrupt Enable (MTIE)**: If set, irq\_timer\_i is enabled.             |
+-------------+-----------+------------------------------------------------------------------------------------------+
| 3           | R/W       | **Machine Software Interrupt Enable (MSIE)**: if set, irq\_software\_i is enabled.       |
+-------------+-----------+------------------------------------------------------------------------------------------+

Machine Interrupt Pending Register (MIP)
----------------------------------------

CSR Address: 0x344

Reset Value: 0x0000_0000

Detailed:

+-------------+-----------+---------------------------------------------------------------------------------------------------+
|   Bit #     |   R/W     |   Description                                                                                     |
+=============+===========+===================================================================================================+
| 31          | R         | Non-maskable interrupt pending: If set, irq\_nmi\_i is pending.                                   |
+-------------+-----------+---------------------------------------------------------------------------------------------------+
| 30:16       | R         | Machine Fast Interrupts Pending: If bit x+16 is set, fast interrupt irq\_fast\_i[x] is pending.   |
+-------------+-----------+---------------------------------------------------------------------------------------------------+
| 11          | R         | **Machine External Interrupt Pending (MEIP)**: If set, irq\_external\_i is pending.               |
+-------------+-----------+---------------------------------------------------------------------------------------------------+
| 7           | R         | **Machine Timer Interrupt Pending (MTIP)**: If set, irq\_timer\_i is pending.                     |
+-------------+-----------+---------------------------------------------------------------------------------------------------+
| 3           | R         | **Machine Software Interrupt Pending (MSIP)**: if set, irq\_software\_i is pending.               |
+-------------+-----------+---------------------------------------------------------------------------------------------------+

Machine Interrupt Enable Register (MIEX)
----------------------------------------

CSR Address: 0x7D0

Reset Value: 0x0000_0000

Detailed:

+-------------+-----------+-------------------------------------------------------------------------------------------------+
|   Bit #     |   R/W     |   Description                                                                                   |
+=============+===========+=================================================================================================+
| 31:0        | R/W       | Machine Fast Interrupt ExtensionEnables: Set bit x to enable fast interrupt irq\_fastx\_i[x].   |
+-------------+-----------+-------------------------------------------------------------------------------------------------+

Machine Interrupt Pending Register (MIPX)
-----------------------------------------

CSR Address: 0x7D2

Reset Value: 0x0000_0000

Detailed:

+-------------+-----------+-----------------------------------------------------------------------------------------------------------+
|   Bit #     |   R/W     |   Description                                                                                             |
+=============+===========+===========================================================================================================+
| 31:0        | R         | Machine Fast Interrupts Extension Pending: If bit x is set, fast interrupt irq\_fastx\_i[x] is pending.   |
+-------------+-----------+-----------------------------------------------------------------------------------------------------------+

Machine Trap-Vector Base Address (MTVEC)
----------------------------------------

CSR Address: 0x305

Reset Value: 0x0000_0001

+-------------+-----------+---------------------------------------------------------------------------------------------------------------+
|   Bit #     |   R/W     |   Description                                                                                                 |
+=============+===========+===============================================================================================================+
| 31 : 2      |   R/W     | BASE: The trap-vector base address, always aligned to 256 bytes, i.e., mtvec[7:2] is always set to  0.        |
+-------------+-----------+---------------------------------------------------------------------------------------------------------------+
|  1 : 0      |   R       | MODE: Always set to 01 to indicate vectored interrupt handling.                                               |
+-------------+-----------+---------------------------------------------------------------------------------------------------------------+


When an exception or an interrupt (except irq\_fastx\_i) is encountered, the core jumps to the corresponding
handler using the content of the MTVEC[31:8] as base address. Only
8-byte aligned addresses are allowed. The only mode supported is
vectorized interrupt, thus the bits 1:0 are hardwired to 01.

Table 6: MTVEC

Machine Trap-Vector Base Address (MTVECX)
-----------------------------------------

CSR Address: 0x7D1

Reset Value: 0x0000_0001

+-------------+-----------+---------------------------------------------------------------------------------------------------------------+
|   Bit #     |   R/W     |   Description                                                                                                 |
+=============+===========+===============================================================================================================+
| 31 : 2      |   R/W     | BASE: The trap-vector base address, always aligned to 256 bytes, i.e., mtvec[7:2] is always set to  0.        |
+-------------+-----------+---------------------------------------------------------------------------------------------------------------+
|  1 : 0      |   R       | MODE: Always set to 01 to indicate vectored interrupt handling.                                               |
+-------------+-----------+---------------------------------------------------------------------------------------------------------------+


When an extended fast interrupt (irq\_fastx\_i) is encountered, the core jumps to the
corresponding handler using the content of the MTVECX[31:8] as base
address. Only 8-byte aligned addresses are allowed. The only mode
supported is vectorized interrupt, thus the bits 1:0 are hardwired to
01.

Table 7: MTVECX

User Trap-Vector Base Address (UTVEC)
-------------------------------------

CSR Address: 0x005

+--------+-----+-----+-----+-----+-----+-----+-----+-----+
| 31 : 8 | 7   | 6   | 5   | 4   | 3   | 2   | 1   | 0   |
+========+=====+=====+=====+=====+=====+=====+=====+=====+
|        | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 1   |
+--------+-----+-----+-----+-----+-----+-----+-----+-----+

When an exception is encountered in user-mode, the core jumps to the
corresponding handler using the content of the UTVEC[31:8] as base
address. Only 8-byte aligned addresses are allowed. The only mode
supported is vectorized interrupt, thus the bits 1:0 are hardwired to
01. *Note that PULP/issimo does not support USER interrupts.*

Table 6: UTVEC

Machine Exception PC (MEPC)
---------------------------

CSR Address: 0x341

Reset Value: 0x0000\_0000

+------+-------+
| 31   | 30: 0 |
+======+=======+
| MEPC |       |
+------+-------+

When an exception is encountered, the current program counter is saved
in MEPC, and the core jumps to the exception address. When a mret
instruction is executed, the value from MEPC replaces the current
program counter.

User Exception PC (UEPC)
------------------------

CSR Address: 0x041

Reset Value: 0x0000_0000

+------+-------+
| 31   | 30: 0 |
+======+=======+
| UEPC |       |
+------+-------+

When an exception is encountered in user mode, the current program
counter is saved in UEPC, and the core jumps to the exception address.
When a uret instruction is executed, the value from UEPC replaces the
current program counter.

Machine Cause (MCAUSE)
----------------------

CSR Address: 0x342

Reset Value: 0x0000_0000

+-------------+-----------+----------------------------------------------------------------------------------+
|   Bit #     |   R/W     |   Description                                                                    |
+=============+===========+==================================================================================+
| 31          |   R       | **Interrupt:** This bit is set when the exception was triggered by an interrupt. |
+-------------+-----------+----------------------------------------------------------------------------------+
|  5 : 0      |   R       | **Exception Code**                                                               |
+-------------+-----------+----------------------------------------------------------------------------------+


Table 7: MCAUSE

User Cause (UCAUSE)
-------------------

CSR Address: 0x042

Reset Value: 0x0000_0000

+-----------+----+----+----+---+
| 31 : 4    | 3  | 2  | 1  | 0 |
+===========+====+====+====+===+
| Interrupt | Exception Code   |
+-----------+------------------+

Detailed:

+-------------+-----------+------------------------------------------------------------------------------------+
|   Bit #     |   R/W     |   Description                                                                      |
+=============+===========+====================================================================================+
| 31          | R/W       | **Interrupt:** This bit is set when the exception was triggered by an interrupt.   |
+-------------+-----------+------------------------------------------------------------------------------------+
| 4:0         | R/W       | **Exception Code**                                                                 |
+-------------+-----------+------------------------------------------------------------------------------------+

Table 8: MCAUSE

Privilege Level
---------------

CSR Address: 0xC10

Reset Value: 0x0000_0003

+--------+-----------+
| 31 : 2 | 1:0       |
+========+===========+
|        | PRV LVL   |
+--------+-----------+

+-----------+----+----+----+----+---+
| 31 : 5    | 4  | 3  | 2  | 1  | 0 |
+===========+====+====+====+====+===+
| Interrupt | Exception Code        |
+-----------+-----------------------+

Detailed:

+-------------+-----------+-------------------------------------------------------------------------------+
|   Bit #     |   R/W     |   Description                                                                 |
+=============+===========+===============================================================================+
| 1:0         | R         | **PRV LVL**: It contains the current privilege level the core is executing.   |
+-------------+-----------+-------------------------------------------------------------------------------+

Table 9: PRIVILEGE LEVEL

MHARTID/UHARTID
---------------

CSR Address: 0xF14/0x014

Reset Value: Defined


+-------------+-----------+--------------------------------------------------+
|   Bit #     |   R/W     |   Description                                    |
+=============+===========+==================================================+
| 31:6        | R         | 0                                                |
+-------------+-----------+--------------------------------------------------+
| 10:5        | R         | **Cluster ID:** ID of the cluster                |
+-------------+-----------+--------------------------------------------------+
| 4           | R         | 0                                                |
+-------------+-----------+--------------------------------------------------+
| 3:0         | R         | **Core ID:** ID of the core within the cluster   |
+-------------+-----------+--------------------------------------------------+

Table 10: MHARTID

PMP Configuration (PMPCFGx)
---------------------------

CSR Address: 0x3A{0,1,2,3}

Reset Value: 0x0000_0000

+----------+
| 31 : 0   |
+==========+
| PMPCFGx  |
+----------+

If the PMP is enabled, these four registers contain the configuration of
the PMP as specified by the official privileged spec 1.10.

PMP Address (PMPADDRx)
----------------------

CSR Address: 0x3B{0x0, 0x1, …. 0xF}

Reset Value: 0x0000_0000

+----------+
| 31 : 0   |
+==========+
| PMPADDRx |
+----------+


If the PMP is enabled, these sixteen registers contain the addresses of
the PMP as specified by the official privileged spec 1.10.

Debug Control and Status (DCSR)
-------------------------------

CSR Address: 0x7B0

Reset Value: 0x0000_0003

+-------------+-----------+-------------------------------------------------------------------------------------------------+
|   Bit #     |   R/W     |   Description                                                                                   |
+=============+===========+=================================================================================================+
| 31:28       | R         | **xdebugver:** returns 4 - External debug support exists as it is described in this document.   |
+-------------+-----------+-------------------------------------------------------------------------------------------------+
| 15          | R/W       | **ebreakm**                                                                                     |
+-------------+-----------+-------------------------------------------------------------------------------------------------+
| 12          | R/W       | **ebreaku**                                                                                     |
+-------------+-----------+-------------------------------------------------------------------------------------------------+
| 11          | R/W       | **stepi**                                                                                       |
+-------------+-----------+-------------------------------------------------------------------------------------------------+
| 8:6         | R/W       | **cause**                                                                                       |
+-------------+-----------+-------------------------------------------------------------------------------------------------+
| 2           | R/W       | **step**                                                                                        |
+-------------+-----------+-------------------------------------------------------------------------------------------------+
| 1:0         | R         | **priv:** returns the current priviledge mode                                                   |
+-------------+-----------+-------------------------------------------------------------------------------------------------+

Debug PC (DPC)
--------------

CSR Address: 0x7B1

Reset Value: 0x0000_0000

+----------+
| 31 : 0   |
+==========+
| DPC      |
+----------+

When the core enters in Debug Mode, DPC contains the virtual address of
the next instruction to be executed.

Debug Scratch Register 0/1 (dscratch0/1)
----------------------------------------

CSR Address: 0x7B2/0x7B3

Reset Value: 0x0000_0000

+-------------+
| 31 : 0      |
+=============+
| DSCRATCH0/1 |
+-------------+

Scratch register that can be used by implementations that need it.


Trigger Select Register (tselect)
---------------------------------

CSR Address: 0x7A0

Reset Value: 0x0000_0000

Accessible in Debug Mode or M-Mode when trigger support is enabled (using the DbgTriggerEn parameter).

CV32E40P implements a single trigger, therefore this register will always read as zero


Trigger Data Register 1 (tdata1)
--------------------------------

CSR Address: 0x7A1

Reset Value: 0x2800_1000

Accessible in Debug Mode or M-Mode when trigger support is enabled (using the DbgTriggerEn parameter).
Since native triggers are not supported, writes to this register from M-Mode will be ignored.

CV32E40P only implements one type of trigger, Match Control. Most fields of this register will read as a fixed value to reflect the single mode that is supported, in particular, instruction address match as described in the Debug Specification 0.13.2 section 5.2.2 & 5.2.9.


+-------+------+------------------------------------------------------------------+
| Bit#  | R/W  | Description                                                      |
+=======+======+==================================================================+
| 31:28 | R    | **type:** 2 = Address/Data match trigger type.                   |
+-------+------+------------------------------------------------------------------+
| 27    | R    | **dmode:** 1 = Only debug mode can write tdata registers         |
+-------+------+------------------------------------------------------------------+
| 26:21 | R    | **maskmax:** 0 = Only exact matching supported.                  |
+-------+------+------------------------------------------------------------------+
| 20    | R    | **hit:** 0 = Hit indication not supported.                       |
+-------+------+------------------------------------------------------------------+
| 19    | R    | **select:** 0 = Only address matching is supported.              |
+-------+------+------------------------------------------------------------------+
| 18    | R    | **timing:** 0 = Break before the instruction at the specified    |
|       |      | address.                                                         |
+-------+------+------------------------------------------------------------------+
| 17:16 | R    | **sizelo:** 0 = Match accesses of any size.                      |
+-------+------+------------------------------------------------------------------+
| 15:12 | R    | **action:** 1 = Enter debug mode on match.                       |
+-------+------+------------------------------------------------------------------+
| 11    | R    | **chain:** 0 = Chaining not supported.                           |
+-------+------+------------------------------------------------------------------+
| 10:7  | R    | **match:** 0 = Match the whole address.                          |
+-------+------+------------------------------------------------------------------+
| 6     | R    | **m:** 1 = Match in M-Mode.                                      |
+-------+------+------------------------------------------------------------------+
| 5     | R    | zero.                                                            |
+-------+------+------------------------------------------------------------------+
| 4     | R    | **s:** 0 = S-Mode not supported.                                 |
+-------+------+------------------------------------------------------------------+
| 3     | R    | **u:** 1 = Match in U-Mode.                                      |
+-------+------+------------------------------------------------------------------+
| 2     | RW   | **execute:** Enable matching on instruction address.             |
+-------+------+------------------------------------------------------------------+
| 1     | R    | **store:** 0 = Store address / data matching not supported.      |
+-------+------+------------------------------------------------------------------+
| 0     | R    | **load:** 0 = Load address / data matching not supported.        |
+-------+------+------------------------------------------------------------------+

Trigger Data Register 2 (tdata2)
--------------------------------

CSR Address: 0x7A2

Reset Value: 0x0000_0000

Accessible in Debug Mode or M-Mode when trigger support is enabled (using the DbgTriggerEn parameter). Since native triggers are not supported, writes to this register from M-Mode will be ignored.

This register stores the instruction address to match against for a breakpoint trigger.

+-------+------+------------------------------------------------------------------+
| Bit#  | R/W  | Description                                                      |
+=======+======+==================================================================+
| 31:0  | R    | **data**                                                         |
+-------+------+------------------------------------------------------------------+



Trigger Data Register 3 (tdata3)
--------------------------------

CSR Address: 0x7A3

Reset Value: 0x0000_0000

Accessible in Debug Mode or M-Mode when trigger support is enabled (using the DbgTriggerEn parameter).

CV32E40P does not support the features requiring this register. Writes are ignored and reads will always return zero.

+-------+------+------------------------------------------------------------------+
| Bit#  | R/W  | Description                                                      |
+=======+======+==================================================================+
| 31:0  | R    | 0                                                                |
+-------+------+------------------------------------------------------------------+



Machine Context Register (mcontext)
-----------------------------------

CSR Address: 0x7A8

Reset Value: 0x0000_0000

Accessible in Debug Mode or M-Mode when trigger support is enabled (using the DbgTriggerEn parameter).

CV32E40P does not support the features requiring this register. Writes are ignored and reads will always return zero.

+-------+------+------------------------------------------------------------------+
| Bit#  | R/W  | Description                                                      |
+=======+======+==================================================================+
| 31:0  | R    | 0                                                                |
+-------+------+------------------------------------------------------------------+


Supervisor Context Register (scontext)
--------------------------------------

CSR Address: 0x7AA

Reset Value: 0x0000_0000

Accessible in Debug Mode or M-Mode when trigger support is enabled (using the DbgTriggerEn parameter).

CV32E40P does not support the features requiring this register. Writes are ignored and reads will always return zero.

+-------+------+------------------------------------------------------------------+
| Bit#  | R/W  | Description                                                      |
+=======+======+==================================================================+
| 31:0  | R    | 0                                                                |
+-------+------+------------------------------------------------------------------+
