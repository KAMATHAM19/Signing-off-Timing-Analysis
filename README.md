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
      * [OpenSTA](#opensta)
      * [lab](#lab)
* [DAY - 2](#day-2)
    * [Timing Checks](#timing-checks)
    * [Design Rule Checks](#design-rule-checks)
    * [Latch Timing](#latch-timing)
    * [STA Text Report](#sta-text-report)
    * [Lab - 2](#lab-2)
       * [Liberity File](#liberity-file)
       * [Lab](#lab)
       * [SPEF](#spef)
* [DAY - 3](#day-3)
    * [Multiple Clocks](#multiple-clocks)
    * [Timing arcs and Timing Sense](#timing-arcs-and-timing-sense)
    * [Cell delays and Models](#cell-delays-and-models)
    * [Lab - 3](#lab-3)
       * [Exercise](#exercise)
 * [DAY - 4](#day-4)
    * [Crosstalk and Noise](#crosstalk-and-noise)
    * [Clock gating checks](#clock-gating-checks)
    * [Async pins checks](#async-pins-checks)
    * [Lab - 4](#lab-4)
       * [Clock gating checks](#clock-gating-checks)
       * [Async pin checks](#async-pin-checks)
  * [DAY - 5](#day-5)
    * [Clock groups](#clock-groups)
    * [Clock properties](#clock-properties)
    * [Timing Exceptions](#timing-exceptions)
    * [Lab - 5](#lab-5)
       * [CPPR](#cppr)
       * [ECO](#eco)
  * [Refernces](#refernces)
  * [Acknowledgement](#refernces)
       
       

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

![1](https://user-images.githubusercontent.com/64173714/220641759-6bd2f9aa-296b-49c3-aff5-0727fba6b333.jpg)

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
     
  ![2](https://user-images.githubusercontent.com/64173714/220641929-590b5bda-b96c-4b8c-9cd1-fad1c14b532f.jpg)

     
<a name="setup-check-&-hold-check"></a>
## Setup Check & Hold Check
               
![3](https://user-images.githubusercontent.com/64173714/220641999-f7bbfebf-0e52-4ff3-80a2-814068537f40.jpg)

SETUP
* The data should be stable at the input of sequential device for sometime before the clock edge that captures the data.
* This sets the maximum data path delay.

HOLD
* The data should be stable at the input of sequential device for sometime before the clock edge that caputers the data.
* This sets the manimum data path delay.
 
** For setup and hold checks of a flop is dependent on the technology node and value is available in logic libraries.

                                 
![4](https://user-images.githubusercontent.com/64173714/220642088-3939b35c-ef4e-4439-b319-3c541cd9c54c.jpg)

To understand the setup and hold checks, DATA (d) is denoted as D1, D2, D3, D4 in the waveform above.

D1 - Data arrives before the setup time window

D2 - Data stays stable for hold for hold time window

D3 - Data arrives close to clock edge violates the setup constraint

D4 - Data arrives close to clock edge violates the hold time constraint


<a name="slack-calculation"></a>
## Slack Calculation

 ![5](https://user-images.githubusercontent.com/64173714/220644424-66778446-7e38-4c90-bbeb-ec21eda3cc62.JPG)


Arrival Time - Time it takes the signal to travel from startpoint to endpoint.

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
<a name="lab-1"></a>
# Lab - 1

<a name="OpenSTA"></a>
##  OpenSTA
* OpenSTA is a gate level static timing verifier. It can be used as a standalone executable to verify the timing of a design using standard file formats.
    * Verilog netlist
    * Liberty library
    * SDC timing constraints
    * SDF delay annotation 
    * SPEF parasitics 
* OpenSTA is designed to be easily bolted on to other tools as a timing engine. Using a network adapter, OpenSTA can access the host netlist data structures without duplicating them.
* Query-based incremental update of delays, arrival, and required times
* Simulator for propagating constants from constraints and netlist tie high/low

<a name="lab"></a>
## Lab

```
git clone https://github.com/vikkisachdeva/openSTA_sta_workshop
cd openSTA_sta_workshop/vlsideepdive_openSTA_labs/lab1
ls
leafpad simple.v
```
<img width="960" alt="verilog" src="https://user-images.githubusercontent.com/64173714/219878971-f637e0f4-ffe4-400c-8826-6eb4010a4336.png">

```
leafpad sky130_fd_sc_hd__tt_025C_1v80.lib
leafpad  simple.sdc
leafpad run.tcl
```
To run sta 

```
sta run.tcl -exit | tee run.log
```
<img width="959" alt="d1 -1" src="https://user-images.githubusercontent.com/64173714/220656077-eae71440-bf07-4d97-83b0-51e1f7c9b7da.png">
<img width="960" alt="d1 -2" src="https://user-images.githubusercontent.com/64173714/220656378-779c79f6-5b93-4041-9b42-611ff2ae26ef.png">
<img width="960" alt="d1 -3" src="https://user-images.githubusercontent.com/64173714/220656751-6c88455f-6563-418c-a2d8-675c8ba7c67d.png">

<a name="day-2"></a>
# DAY - 2 


<a name="timing-checks"></a>
## Timing Checks

![d2 1](https://user-images.githubusercontent.com/64173714/220770997-f967f1e6-ea53-424f-b9fd-c4622d3e379e.jpg)

STA will perform additional checks in addition to the setup and hold checks.
1. Clock gating check
2. Asynchronous pin checks
3. Data to data checks

<a name="design-rule-checks"></a>
## Design Rule Checks
1. Slew/Transition analysis 
    * Rise Slew - Time taken by a signal when it rises from 70% to 30% of vdd
    * Fall Slew - Time taken by a signal when it falls from 70% to 30% of vdd
2. Load analysis

   It is the process of evaluating a design's timing by analyzing  the effects of load capacitance on signal propagation.

- minimum and maximum capacitance on ports and nets
- fanout load on ports and output pins

3. Clock skew analysis

The Difference in delays of the clock at different points in the path.

- skew +ve - Capture clock has more delay than launch
- skew -ve - Launch clock has more delay than capture

4. pulse width checks
 
 Pulse width checks are used to ensure that a sequential element's output waveform meets a minimum or maximum width requirement.
 
<a name="latch-timing"></a>
## Latch Timing

![d2 2](https://user-images.githubusercontent.com/64173714/220771107-e3dfba4e-b3f1-40e5-ac34-f63c3b63d972.jpg)

 - The flop-based design data is launched and captured on the edges, and regardless of the delay, it must be stable for one clock period.
 - It allows more flexibility in timing
 
 ![d2 3](https://user-images.githubusercontent.com/64173714/220771149-982a06dd-3959-4a97-93e9-514827f4b098.jpg)

Time borrowing is a technique for adjusting the timing of two distinct paths. It is possible to accomplish this by borrowing or lending some delay from/to a neighbouring path. A latch is placed in the path that has a timing violation. This latch acts as a buffer, delaying the signal and allowing the previous stage to complete its operation. The latch's output is then transferred to the next stage.

<a name="sta-text-report"></a>
## STA Text Report

<img width="478" alt="sta report" src="https://user-images.githubusercontent.com/64173714/220771774-5260032a-c4e2-4ecb-8f64-965c08cf3f20.png">


<a name="lab-2"></a>
# Lab - 2

<a name="liberity-file"></a>
Liberity File
The.lib file is an ASCII representation of the timing and power parameters associated with any cell in a specific semiconductor technology.
* The .lib file contains timing models and data to calculate
* I/O delay paths
* Timing check values
* Interconnect delays

<img width="878" alt="lib 1" src="https://user-images.githubusercontent.com/64173714/220772734-1a6ee922-ec08-410f-8e13-fa83c2ab3c5c.png">
<img width="889" alt="lib 2" src="https://user-images.githubusercontent.com/64173714/220772764-1bc24755-55cd-4bb0-ae56-a7fb3d791cd7.png">
<img width="878" alt="lib 3" src="https://user-images.githubusercontent.com/64173714/220772783-0801064a-16c6-4119-b3d1-d332efb68716.png">


Lab
```
cd lab2
leafpad simple_min.lib
leafpad simple_max.lib
```
<img width="960" alt="d2 -1" src="https://user-images.githubusercontent.com/64173714/220660262-d75b2352-7286-4b72-a58a-7f0e881769ee.png">

To find all of the cells in simple max.lib
```
# total no of cells
grep -c " End cell" simple_max.lib 
# cells
grep -c " End cell" simple_max.lib
```
<img width="960" alt="d2 - 2" src="https://user-images.githubusercontent.com/64173714/220673290-1e7c644a-a351-453f-8197-125f67c0d0b9.png">

The pins of the cell NAND2_X1 in simple_max.lib

<img width="635" alt="final nad" src="https://user-images.githubusercontent.com/64173714/220676727-229895fe-355b-43a1-ad67-20ed19dc0e1e.png">

The difference between NAND2_X1 and NAND3_X1

<img width="635" alt="final nad" src="https://user-images.githubusercontent.com/64173714/220679191-bebcdb61-c727-45e1-b591-eddc46e04faf.png"> 
<img width="629" alt="final nad3" src="https://user-images.githubusercontent.com/64173714/220679288-7aed8e8d-09cf-43fa-8f31-613b3f6e446e.png"> 

<a name="spef"></a>
# SPEF

A SPEF (Standard Parasitic Exchange Format) file describes the design's parasitic information.
 * Users would never manually create this file.
 * It is generated automatically by the tool.
 * It is primarily used to transfer parasitic data from one tool to another.

<img width="960" alt="spef" src="https://user-images.githubusercontent.com/64173714/220685189-9ca39ed9-e30e-43cd-bdc5-c8d41a74be63.png">

add  report_timing –num_paths 5 in tcl file
<img width="922" alt="d2 1" src="https://user-images.githubusercontent.com/64173714/220682933-129b1899-9c57-4869-9e08-7013f1e9b823.png">

run sta 
```
sta run.tcl | tee run.log
```
<img width="960" alt="d2 - 3" src="https://user-images.githubusercontent.com/64173714/220684435-31a45d63-7768-44b9-9730-65d6749ea92e.png">

understanding timing report

<img width="907" alt="d2 timing" src="https://user-images.githubusercontent.com/64173714/220773552-c30661f1-ddae-4e06-9fe9-544887393f07.png">


<a name="day-3"></a>
# DAY - 3

<a name="multiple-clocks"></a>
## Multiple Clocks

<a name="timing-arcs-and-timing-sense"></a>
## Timing arcs and Timing Sense

<a name="cell-delays-and-models"></a>
## Cell delays and Models
    
       

<a name="lab-3"></a>
# Lab - 3

<a name="exercise"></a>
## Exercise
```
cd lab3
ls
sta run.tcl | tee out.txt
```
<img width="923" alt="d3 2" src="https://user-images.githubusercontent.com/64173714/220686704-8ed65d6a-f82b-4f25-bbe4-663affc82235.png">

<img width="539" alt="d3 3" src="https://user-images.githubusercontent.com/64173714/220686834-4cb29e4b-7d2f-49c3-b4b3-f6759fe23280.png">

report_checks –from F1/CK -endpoint_count 100

<img width="960" alt="8" src="https://user-images.githubusercontent.com/64173714/220889593-bde7b1f3-b7be-4491-8ffb-30e8a06226f0.png">

paths

<img width="960" alt="path 1" src="https://user-images.githubusercontent.com/64173714/220889647-8fb5168a-01f6-4151-967f-da87a711f4ce.png">
<img width="963" alt="path 2" src="https://user-images.githubusercontent.com/64173714/220889663-2c3f5d24-9cfc-4c06-acc6-06de808fceac.png">

<img width="972" alt="path 3" src="https://user-images.githubusercontent.com/64173714/220889677-5320f8a1-89d8-4ad5-a78d-87782e07cb59.png">
<img width="960" alt="path 4" src="https://user-images.githubusercontent.com/64173714/220889689-0c49b8d3-ea2f-4524-88fe-4ed58416eeae.png">

<img width="960" alt="path 5" src="https://user-images.githubusercontent.com/64173714/220889706-2a8af071-36a8-4828-bb80-8eecaf35eb67.png">
<img width="961" alt="path 6" src="https://user-images.githubusercontent.com/64173714/220889719-2553f9e6-bb85-4ccf-b77f-88c92db6beb6.png">

<img width="967" alt="path 7" src="https://user-images.githubusercontent.com/64173714/220889736-103bd2d9-aa9e-47fd-89ec-4133ce506f97.png">

<img width="962" alt="path 8" src="https://user-images.githubusercontent.com/64173714/220889797-61ece514-984a-4ad9-9ade-b5b4e402f03a.png">

<a name="day-4"></a>
# DAY - 4

<a name="crosstalk-and-noise"></a>
## Crosstalk and Noise

<a name="clock-gating-checks"></a>
## Clock gating checks

<a name="async-pins-checks"></a>
## Async pins checks

<a name="lab-4"></a>
## Lab - 4

<a name="clock-gating-checks"></a>
## Clock gating checks
```
  cd lab6
  leafpad run.tcl
  sta run.tcl | tee run.log
```
<img width="960" alt="d4 1" src="https://user-images.githubusercontent.com/64173714/220689445-a0e80c70-afa5-4500-a239-d09e55db6f1e.png">
<img width="960" alt="d4 2" src="https://user-images.githubusercontent.com/64173714/220689509-47b2f21e-5a4d-43f4-9378-7be582146af2.png">
<img width="960" alt="d4 3" src="https://user-images.githubusercontent.com/64173714/220689554-7fc79a10-0ce0-4d96-b90b-93332d13a24f.png">
<img width="960" alt="d4 4" src="https://user-images.githubusercontent.com/64173714/220689598-7935b3f5-8132-430a-82c1-c778c77566a4.png">

<a name="async-pin-checks"></a>
## Async pin checks
```
  cd lab7
  leafpad run.tcl
  sta run.tcl | tee run.log
```
<img width="960" alt="d4 6" src="https://user-images.githubusercontent.com/64173714/220689652-1a3532c0-94ea-4a73-a95d-6fcdea9b7b9a.png">

<a name="day-5"></a>
# DAY - 5

<a name="clock-groups"></a>
## Clock groups

<a name="clock-properties"></a>
## Clock properties

<a name="timing-exceptions"></a>
## Timing Exceptions

<a name="lab-5"></a>
## Lab - 5
<a name="cppr"></a>
## CPPR
<img width="827" alt="day 5" src="https://user-images.githubusercontent.com/64173714/220872644-b718af94-c6b3-44a9-aa2a-084599ae0a6e.png">

```
cd lab4
leafpad run.tcl

```
<img width="956" alt="day 5 2" src="https://user-images.githubusercontent.com/64173714/220874622-9d902ad8-4dae-4367-bf6e-879661c0fd6d.png">
<img width="960" alt="day 5 1" src="https://user-images.githubusercontent.com/64173714/220873899-d46ed050-c519-4d3d-bf02-fa0cd3a5417d.png">

```
 sta run.tcl | tee out.txt
```
<img width="960" alt="d5 1" src="https://user-images.githubusercontent.com/64173714/220874109-e8902969-fbd1-4d63-82b6-a0e55221e6c7.png">
<img width="960" alt="d5 2" src="https://user-images.githubusercontent.com/64173714/220874125-965fd598-9df9-4916-a901-674a19b2743d.png">

with enable
<a name="eco"></a><img width="960" alt="d6 1" src="https://user-images.githubusercontent.com/64173714/220875922-8daf884c-f4b3-4e09-9fb1-377a825f47c7.png">

<img width="960" alt="d6 2" src="https://user-images.githubusercontent.com/64173714/220875986-6e98f6df-a71e-4f1d-8fc9-0c8e3037d2e0.png">

## ECO
ECO – Engineering Change Order

* During the ECO cycle, we perform various analyses one by one for each check that needs to be closed but cannot be closed until the PnR stage.

* There are specialised signoff tools that assist us in analysing the issue and recommending the changes that must be made in order to close the issue.

* The suggested change is captured in an eco file.
```
cd lab5
leafpad run.tcl
```
<img width="960" alt="d6 4" src="https://user-images.githubusercontent.com/64173714/220877356-c6462cd5-b9e8-4ce0-933d-829a2f10a15a.png">
<img width="960" alt="d6 3" src="https://user-images.githubusercontent.com/64173714/220877121-80a9984d-9205-4164-a4c4-334d4ae7af16.png">

```
sta run.tcl | tee run.log
```

<img width="960" alt="eco 1" src="https://user-images.githubusercontent.com/64173714/220880095-1484259f-ea75-412b-942e-870abf58bbd7.png">
<img width="957" alt="eco 2" src="https://user-images.githubusercontent.com/64173714/220880111-3667315e-158f-48f2-9345-1255d2222f5a.png">
<img width="960" alt="ec0 3" src="https://user-images.githubusercontent.com/64173714/220880125-77f1acb9-8771-4ec9-b28a-1deb5c1be53b.png">
<img width="960" alt="eco4" src="https://user-images.githubusercontent.com/64173714/220880144-e7f145f8-4e3d-41f2-a7af-aedb6e3addb5.png">

Difference between s27_eco.v and s27.v
<img width="960" alt="7 1" src="https://user-images.githubusercontent.com/64173714/220883196-4cb1640d-801f-4cf8-82ec-cd7cd2fa67f7.png">
<img width="952" alt="7 2" src="https://user-images.githubusercontent.com/64173714/220883211-1844364a-0818-40c5-a7fb-68bc47c28de1.png">

<a name="refernces"></a>
## Refernces
1. https://github.com/The-OpenROAD-Project/OpenSTA/blob/master/doc/OpenSTA.pdf
2. https://people.eecs.berkeley.edu/~alanmi/publications/other/liberty07_03.pdf
3. https://www.vlsisystemdesign.com/spef-format-part-1/
4. https://www.vlsisystemdesign.com/?s=cppr
5. https://www.vlsisystemdesign.com/the-beauty-of-slack-based-timing-eco/

<a name="acknowledgement"></a>
## Acknowledgement
1. [Kunal Gosh](https://www.linkedin.com/in/kunal-ghosh-vlsisystemdesign-com-28084836/)
2. [Vikas Sachdeva](https://www.linkedin.com/in/vikas-sachdeva-0849577/)

