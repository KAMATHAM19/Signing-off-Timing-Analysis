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
