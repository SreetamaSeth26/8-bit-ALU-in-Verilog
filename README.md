# 8-bit ALU in Verilog

A combinational Arithmetic Logic Unit (ALU) built in Verilog. It takes two 8-bit inputs and a 4-bit select line, and outputs the result of one of 16 operations (arithmetic, logical, shift, rotate, and compare).

## Features

- 8-bit wide inputs and output
- 16 selectable operations using a 4-bit select signal
- Fully combinational design (`always @(*)`)
- Covers arithmetic, bitwise logic, shift, rotate, increment/decrement, and comparison operations

## Module Ports

**alu**

| Port   | Direction | Width | Description              |
|--------|-----------|-------|---------------------------|
| A      | input     | 8     | First operand             |
| B      | input     | 8     | Second operand            |
| sel    | input     | 4     | Operation select          |
| result | output    | 8     | Result of selected op     |

## Operation Table

| sel (binary) | Operation | Description                          |
|--------------|-----------|----------------------------------------|
| 0000         | ADD       | result = A + B                        |
| 0001         | SUB       | result = A - B                        |
| 0010         | AND       | result = A & B                        |
| 0011         | OR        | result = A \| B                       |
| 0100         | XOR       | result = A ^ B                        |
| 0101         | NOT       | result = ~A                           |
| 0110         | NAND      | result = ~(A & B)                     |
| 0111         | NOR       | result = ~(A \| B)                    |
| 1000         | XNOR      | result = ~(A ^ B)                     |
| 1001         | LSL       | Logical left shift A by 1             |
| 1010         | LSR       | Logical right shift A by 1            |
| 1011         | ROL       | Rotate A left by 1                    |
| 1100         | ROR       | Rotate A right by 1                   |
| 1101         | INC       | result = A + 1                        |
| 1110         | DEC       | result = A - 1                        |
| 1111         | CMP       | result = 1 if A > B, else 0           |
| default      | -         | result = 0                            |

## Testbench

The testbench (`alu_tb`) instantiates the ALU and applies a sequence of `sel` values every 10 time units, changing `A` and `B` where needed to exercise each operation, including both cases of the compare operation (A > B and A < B).

It uses `$dumpfile` and `$dumpvars` to generate a VCD waveform, and `$monitor` to print live values of `A`, `B`, `sel`, and `result` to the console.

## Simulation

Simulated on EDA Playground using Icarus Verilog.

Sample console output:

```
Time=0   A=20  B=10 sel=0000 result=30
Time=10  A=20  B=10 sel=0001 result=10
Time=20  A=20  B=10 sel=0010 result=0
Time=30  A=20  B=10 sel=0011 result=30
Time=40  A=20  B=10 sel=0100 result=30
Time=50  A=20  B=10 sel=0101 result=235
Time=60  A=20  B=10 sel=0110 result=255
Time=70  A=20  B=10 sel=0111 result=225
Time=80  A=20  B=10 sel=1000 result=225
Time=90  A=20  B=10 sel=1001 result=40
Time=100 A=20  B=10 sel=1010 result=10
Time=110 A=179 B=10 sel=1011 result=103
Time=120 A=179 B=10 sel=1100 result=217
Time=130 A=25  B=10 sel=1101 result=26
Time=140 A=25  B=10 sel=1110 result=24
Time=150 A=30  B=20 sel=1111 result=1
Time=160 A=10  B=20 sel=1111 result=0
```

All 16 operations were verified to produce correct results.

## Synthesis (Yosys + netlistsvg)

The design was synthesized using Yosys, and the netlist was visualized using netlistsvg.

Steps:

```
cd ~/Desktop
ls
yosys
```

Inside the Yosys shell:

```
read_verilog alu.v
prep -top alu
write_json alu.json
exit
```

Back in the terminal:

```
netlistsvg alu.json -o alu.svg
open alu.svg
```

This generates `alu.json` (the synthesized netlist in JSON form) and `alu.svg` (a visual schematic of the synthesized ALU), useful for checking that the design synthesizes correctly and for understanding the resulting hardware structure.

## File Structure

```
alu.v        - ALU design module
alu_tb.v     - Testbench
alu.json     - Synthesized netlist (from Yosys)
alu.svg      - Netlist schematic (from netlistsvg)
```

## How to Run

1. Open the design and testbench files on EDA Playground (or any Icarus Verilog setup).
2. Run the simulation to view console output and the VCD waveform.
3. Run the Yosys and netlistsvg commands above to generate and view the synthesized netlist.
