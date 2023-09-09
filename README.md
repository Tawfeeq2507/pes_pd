![image](https://github.com/VardhanSuroshi/pes_asic_class/assets/132068498/33403244-c9dd-4aef-a022-da52e2eef51c)

Welcome to my GitHub repository dedicated to VLSI Physical Design for ASICs using open-source tools.

**Course Name** : VLSI Physical Design for ASICs  
**Instructor** : Kunal Ghosh 


# TABLE OF CONTENTS

## DAY 1 

**Inception of open-source EDA, OpenLANE and Sky130 PDK**

+ How to talk to computers
  - Introduction to QFN-48 Package, chip, pads, core, die and IPs
  - Introduction to RISC-V  
  - From Software Applications to Hardware 
+ SoC design and OpenLANE
  - Introduction to all components of open-source digital asic design
  - Simplified RTL2GDS flow
  - Introduction to OpenLANE and Strive chipsets 
  - Introduction to OpenLANE detailed ASIC design flow 
+ Get familiar to open-source EDA tools
  - OpenLANE Directory structure in detail 
  - Design Preparation Step
  - Review files after design prep and run synthesis
  - OpenLANE Project Git Link Description
  - Steps to characterize synthesis results 
  
## DAY-2

**Good floorplan vs bad floorplan and introduction to library cells**

+ Chip Floor planning considerations
  - Utilization factor and aspect ratio
  - Concept of pre-placed cells 
  - De-coupling capacitors 
  - Power planning 
  - Pin placement and logical cell placement blockage
  - Steps to run floorplan using OpenLANE
  - Review floorplan files and steps to view floorplan 
  - Review floorplan layout in Magic
+ Library Binding and Placement
  - Netlist binding and initial place design 
  - Optimize placement using estimated wire-length and capacitance
  - Final placement optimization 
  - Need for libraries and characterization 
  - Congestion aware placement using RePlAce 
+ Cell design and characterization flows
  - Inputs for cell design flow 
  - Circuit design step 
  - Layout design step 
  - Typical characterization flow 
+ General timing characterization parameters
  - Timing threshold definitions 
  - Propagation delay and transition time

## DAY-3

**Design library cell using Magic Layout and ngspice characterization**

+ Labs for CMOS inverter ngspice simulations
  - placer revision 
  - SPICE deck creation for CMOS inverter
  - SPICE simulation lab for CMOS inverter 
  - Switching Threshold Vm
  - Static and dynamic simulation of CMOS inverter
  - Lab steps to git clone vsdstdcelldesign 
+ Inception of Layout A CMOS fabrication process
  - Create Active regions 
  - Formation of N-well and P-well
  - Formation of gate terminal 
  - Lightly doped drain (LDD) formation
  - Source A drain formation 
  - Local interconnect formation 
  - Higher level metal formation 
  - Lab introduction to Sky130 basic layers layout and LEF using inverter
  - Lab steps to create std cell layout and extract spice netlist
+ Sky130 Tech File Labs
  - Lab steps to create final SPICE deck using Sky130 tech 
  - Lab steps to characterize inverter using sky130 model files 
  - Lab introduction to Magic tool options and DRC rules 
  - Lab introduction to Sky130 pdk's and steps to download labs
  - Lab introduction to Magic and steps to load Sky130 tech-rules
  - Lab exercise to fix poly.9 error in Sky130 tech-file
  - Lab exercise to implement poly resistor spacing to diff and tap
  - Lab challenge exercise to describe DRC error as geometrical construct 
  - Lab challenge to find missing or incorrect rules and fix them 

## DAY-4

**Pre-layout timing analysis and importance of good clock tree**

+ Timing modelling using delay tables
  - Lab steps to convert grid info to track info 
  - Lab steps to convert magic layout to std cell LEF
  - Introduction to timing libs and steps to include new cell in synthesis 
  - Introduction to delay tables 
  - Delay table usage Part 1
  - Delay table usage Part 2 
  - Lab steps to configure synthesis settings to fix slack and include vsdinv 
+ Timing analysis with ideal clocks using openSTA
  - Setup timing analysis and introduction to flip-flop setup time
  - Introduction to clock jitter and uncertainty
  - Lab steps to configure OpenSTA for post-synth timing analysis 
  - Lab steps to optimize synthesis to reduce setup violations 
  - Lab steps to do basic timing ECO
+ Clock tree synthesis TritonCTS and signal integrity
  - Clock tree routing and buffering using H-Tree algorithm
  - Crosstalk and clock net shielding
  - Lab steps to run CTS using TritonCTS
  - Lab steps to verify CTS runs 
+ Timing analysis with real clocks using openSTA
  - Setup timing analysis using real clocks
  - Hold timing analysis using real clocks
  - Lab steps to analyze timing with real clocks using OpenSTA
  - Lab steps to execute OpenSTA with right timing libraries and CTS assignment
  - Lab steps to observe impact of bigger CTS buffers on setup and hold timing 

## DAY-5

**Final steps for RTL2GDS using tritonRoute and openSTA**
+ Routing and design rule check (DRC)
  - Introduction to Maze Routing A LeeAs algorithm
  - LeeAs Algorithm conclusion
  - Design Rule Check 
+ Power Distribution Network and routing
  - Lab steps to build power distribution network 
  - Lab steps from power straps to std cell power
  - Basics of global and detail routing and configure TritonRoute 
+ TritonRoute Features
  - TritonRoute feature 1 - Honors pre-processed route guides 
  - TritonRoute Feature2 & 3 - Inter-guide connectivity and intra- & inter-layer routing
  - TritonRoute method to handle connectivity
  - Routing topology algorithm and final files list post-route






