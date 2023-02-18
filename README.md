# Signing-off Timing Analysis


##  OpenSTA
* OpenSTA is a gate level static timing verifier. It can be used as a standalone executable to verify the timing of a design using standard file formats.
     • Verilog netlist
  • Liberty library
  • SDC timing constraints
  • SDF delay annotation 
  • SPEF parasitics 
* OpenSTA is designed to be easily bolted on to other tools as a timing engine. Using a network adapter, OpenSTA can access the host netlist data structures without duplicating them.
* Query-based incremental update of delays, arrival, and required times
* Simulator for propagating constants from constraints and netlist tie high/low

Lab1

```
git clone https://github.com/vikkisachdeva/openSTA_sta_workshop
cd openSTA_sta_workshop/vlsideepdive_openSTA_labs/lab1
ls
leafpad simple.v

<img width="960" alt="verilog" src="https://user-images.githubusercontent.com/64173714/219878971-f637e0f4-ffe4-400c-8826-6eb4010a4336.png">


leafpad sky130_fd_sc_hd__tt_025C_1v80.lib
leafpad 
leafpad
sta run.tcl -exit | tee run.log



read_verilog simple.v
set_input_delay 0 -min -rise [get_ports inp1] -clock tau2015_clk

```
