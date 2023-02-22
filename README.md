# Signing-off Timing Analysis

Sign-off timing analysis is a vital step in the electronic design automation process. This analysis validates a design's timing and ensures that it meets the performance specifications. It involves simulating the design at various levels of abstraction in order to test its timing behaviour and identify and correct any potential timing violations. Typically, the analysis is performed at the end of the design flow, just before the design is sent to fabrication. This ensures that the final product meets the desired performance specifications. The analysis of sign-off timing is crucial in ensuring the reliability and functionality of electronic devices and systems.

# Table of Contents
* [DAY- 1](#day-1)
   * [Static timing analysis (STA)](#static-timing-analysis-(sta)) 
   * [STA inputs](#sta-inputs)
   * [Timing paths](#timing-paths)
   * [Components of Timing paths](#components-of-timing-paths)
   * [Setup Check & Hold Check](#setup-check-&-hold-check)
   * [Slack Calculation](#slack-calculation)
   * [SDC commands](#sdc-commands)
   * [Clocks](#clocks) 
   * [Port delays](#port-delays)
   * [Boundary Constraints](#boundary-constraints)    
 * [Lab - 1](#lab-1)

<a name="day-1"></a>
# DAY - 1 

<a name="static-timing-analysis-(sta)"></a>
## Static timing analysis (STA) 
STA is a technique for improving a design's timing performance.
* Features - static, exhaustive, functionality, conservative
* STA is only used for Synchronous Design

<a name="sta-inputs"></a>
## STA inputs 
- Netlist
- Constraint file or Synopsys Design Constraint (SDC) 
- logic libraries

<a name="timing-paths"></a>
## Timing paths
To perform design analysis, STA divides the logic design into `ports` and `sequential elements`.

     pic 1

<a name="components-of-timing-paths"></a>
## Components of Timing paths
* Startpoint 
     * where the data is launched by the clock edge 
     * where the data must be available at a specific time
     * input ports or register clock pin
* Endpoint 
     * where the data is captured by clock edge
     * where the data must be available at a specific time
     * output ports or register data pin
* Combinational Logic 
     * Elements that have no memory element or internal state
     
  
  pic 2
     
<a name="setup-check-&-hold-check"></a>
## Setup Check & Hold Check

                  pic 3

SETUP
* The data should be stable at the input of sequential device for sometime before the clock edge that captures the data.
* This sets the maximum data path delay.

HOLD
* The data should be stable at the input of sequential device for sometime before the clock edge that caputers the data.
* This sets the manimum data path delay.
 
** For setup and hold checks of a flop is dependent on the technology node and value is available in logic libraries.

                                 pic 4

To understand the setup and hold checks, DATA (d) is denoted as D1, D2, D3, D4 in the waveform above.

D1 - Data arrives before the setup time window

D2 - Data stays stable for hold for hold time window

D3 - Data arrives close to clock edge violates the setup constraint

D4 - Data arrives close to clock edge violates the hold time constraint


<a name="slack-calculation"></a>
## Slack Calculation

                pic 5

Arrival Time - Time it takes the signal to travel from startpoint to endpoint.
             - one endpoint can have multiple arrival times.

Required Time - The signal should arrive before the specified time.

Slack - The difference between the required arrival time of a signal and the actual arrival time of the signal at the destination
* slack is +ve - when data arrives earlier then required time
* slack is -ve - when data arrives later then the required time

setup slack 
```
    setup slack= Data Required Time- Data Arrival Time
```
hold slack 
```
    hold slack= Data Arrival Time- Data Required Time
```

<a name="sdc-commands"></a>
## SDC commands

1) Constarints for timing - specify parameters affecting operational frequency of the design
   ```
      create_clock
      create_generated_clock
      set_clock_groups
      set_clock_transition
      set_timing_derate
   ```
2) Constraints for area and power - specify restrictions about the area and power
``` 
   set_max_area
   set_max_dynamic_power
```
3) Constaints for design rules - requirements of the target technology
```
    set_max_capacitance
    set_min_capacitance
    set_max_transition
    set_max_fanout
```
4) constarints for interfaces - assumptions on the design boundary
```
    set_driving_cell
    set_input_delay
    set_output_delay
    set_load
```
5) Contarints for specific modes and configurations - assumptions on the values allowed
```
   set_case_analysis
   set_logic_dc
   set_logic_one
   set_logic_zero
```
6) Exceptions to design constraints - relax the requirements set by the other commands or default sta tool analysis
```
   set_false_path
   set_multicycle_path
   set_max_delay
   set_min_delay
   set_disable_timing
```
<a name="clocks"></a>
## Clocks 
clocks are specified in timing constraints using command
```
create_clock -period 10 -waveform {2 4} {C1 Ck} -name

```
Where 
    
    create_clock - It specifies primary source input ports of the design
    -period 10 - period of the clock (10 units)
    -waveform {2, 4} - period when it rises and falls
    {C1 Ck} - attaching  the clocks in the design
    -name - name of the clock
    
<a name="generated-clocks"></a>
## Generated clocks   
clocks which are created inside the design using commands

```
create_generated_clock -divide_by 2 -source C1 -master_clock CLK1 {GC2 GC1}

## Divide by (edges have odd number of elements - rise at 1, fall at 5, rise at 7)
create_generated_clock -edges{1 5 7} -name DIV3A -source SYSCLK {GC}

## To generate pulses
create_geerated_clock -name pulse -source CLK1 -edges{1 1 3} -edge_shift{0 2 0}{pulsegen}
```
<a name="port-delay"></a>
## Port delays
```
 set_input_delay
 set_output_delay
```
<a name="boundary-constraints"></a>
## Boundary constraints
```
set_port_fanout_number - number of pins in fanout
set_fanout_load - load in multiple of standard cell/buffer
set_load - load in terms of capacitance value
set_drive - specifies resistance value
set-driving_cell - specifies cell driving port
set_input_transition - specifiestransition value
```
<a name="lab-1"><a/a>
# Lab - 1

<a name="day-1"></a>
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
