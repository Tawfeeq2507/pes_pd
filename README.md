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


# DAY 1 

## Inception of open-source EDA, OpenLANE and Sky130 PDK

<details>
<summary> How to talk to computers </summary>

### Introduction to QFN-48 Package, chip, pads, core, die and IPs

Lets take an example of an Arduino Board,An Arduino board is a small computer that you can use to control and interact with electronic devices. It's a physical platform that allows you to write and upload programs (called "sketches") to make things like lights, motors, sensors, and other components work together.
we take an Arduino board since we will be working with something similar, **we will be talking about a field which is lying inside the chip shown below**:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/0783c87c-1949-4fa8-8a9b-b6fb487ad22c)

- if we want to desgin this particular Arduino board, we can describe it in a form of a block diagram shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/5f0aa3ef-2b05-414d-9234-ddc4d438b5ec)

- the highlighted area of the chip is nothing but the processor shown above and along with the processors we have all the interfaces that we see around the particular processor.
- This is the typical arduino board diagram looks like.

we wont be talking about the embedded desgin and rather will be looking into the chip desgining.

when we open up the IC it looks something like this shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/049a19cc-766c-46f6-9879-ca05b3c9ae8b)

what we see above is usually what we call a **"chip"**, but its known as a **"PACKAGE"**, these packages have been assigned with certain names for ex: we see that the above package is named **"QNF-48"**.Similarly there are multiple packages in the market with different flavours and pins.
- Here the pin loacations of the particular package are all given by the particular arduino board.
- the package seen above has a size of 7mm x 7mm

- the main Brain of the package the chip sits in the middle of the package and the way the chip is connected to the package is shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/9ff58809-bd74-41fa-9043-cb09f788ed90)

- Here we have used **"wire bounds"** to connect the pins to the boundaries of the Chip, In this way we are able to transfer all the signal from outside world into the chip.

When we Open the chip it looks like this shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/f90561ac-8bc0-4f3f-9ab0-6c4cf846bb62)

- The chip that is shown above has many various components and one of the Important componants is the **"PADs"**.
- **"PADs"** in a chip are like the little metal feet or points on the bottom of the chip. They're used to connect the chip to a circuit through which we can send the outside signal into the chip so it can do its job.
- the Middle free space area seen above inside is known as the **"Core"** of the chip.
- **"Core"** of a chip is like the brain of the chip. It's the central part that does most of the actual thinking and processing of information.Its a place where all our Digital logical sits,ex:AND gate,OR gate,MUXs,etc.
- the size of the chip is known as the **"Die"**.
- **"Die"** is like the heart of a computer chip. It's a tiny, flat piece of silicon that contains the actual electronic circuits. It's where all the important computations and operations happen.This the Die that gets manufactured on the **"Silicon Wafer"**.

The typical **Core** of a CHIP consists of an SoC(we will be working with RISC-V SoC),SRAM,ADCs,DACs,PLL,SPI and couple of components shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/e88f8c8a-3d09-4555-8b92-89a7bb29b5e9)

- these SRAM,ADC,DAC,PLL all together are known As **"Foundry IP's(Intellectual Properties)"**
- **Foundry** is an important term in chip Designing Chips, all our devices,mobiles,everything is depended on the Foundry's.
- Foundry is a place where the chips are manufactured, Foundry's are set of machines that we communicate with.
- The digital Blocks placed the SoC and the SPI are basically called as **"Macros"**.

</details>


<details>
<summary> Inroduction to RISC-V </summary>

#### What is RISC-V?
**"RISC-V intruction set architecture"** popularly known as **"ISA"**.It is nothing but a language of the computer,a way through which we are able to talk and communicate with the computers.RISC-V is an open-source instruction set architecture (ISA) based on established reduced instruction set computing (RISC) principles. An instruction set architecture is essentially the set of instructions that a computer's processor can execute. 

For a C program to run on a computer hardware which has got a particular Layout(qFlow), where this layout is nothing but interior of a chip present inside our devices,There are certain flow to pass the C program to the layout.



- here the C program is first compiled in its assembly language program which is nothing but the RISC-V assembly language program.
- this Aseembly language program is then converted into machine language program aka the binary language program(ie: 1's and 0's) which is understood by the hardware of the computer.
- here the codes in hexadecimal format but in real term they are in binary format, therefore this needs to be converted into binary format.
- after converting these bits gets finnaly executed in the layout and get the required output.


Another layer present betweeen the C pragram and the layout is the **"HDL(Hardware discriptive language)"**.

#### What is HDL?
**"HDL"** stands for **Hardware Description Language**. It's a specialized programming language used to describe the structure and behavior of electronic circuits and systems. HDLs are used in the design, simulation, and synthesis of digital circuits, such as those found in microprocessors, memory chips, and other integrated circuits.
There are two main types of HDLs:
 1)**Verilog**: Developed by Phil Moorby and Prabhu Goel in the 1980s
 2)**VHDL (VHSIC Hardware Description Language)**:Developed by the U.S. Department of Defense in the 1980s


- To Implement these RISC-V specifications we need **RTL(Register-Transfer Level)**,in this case shown below the RTL used is **picorv32 cpu core**

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/f8e12d12-72af-4d9a-b951-14769924a11f)


- this RTL implments these RISC-V specifications.
- and from here its RTL-GDS flow










  
</details>

