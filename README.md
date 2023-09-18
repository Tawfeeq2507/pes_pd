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

### What is RISC-V?
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

### From Software Applications to Hardware 

Any **application software** or aka **"Apps"** run on **Hardware**... but how does this happen??

- First the Application software enters the Blocks called the **System Software**,where the system software intern converts the application program into the binary language.
- the Major components of **"System Software"** concists of the OS(Operating system),Assembler,compiler.
- The **"Opertaing system:** handles lots of things, it handles the IO operations,allocate memory, low level system functions, OS also helps in taking the application program and converting it into its respective assembly language and finally inot binary program that is understood by the hardware.
- The output of the OS are nothing but small functions in the given programs(C++,Java,C,python,etc).
- these are taken by the respective **Compiler** and then converted into intstructions.
- The syntax of these instructions depends on what kind of the hardware is,(ex if the hardware is for intel x86 then the instructions will be of intel x86 only).
- all these instructions all together form the **.exe file**

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/79883606-8f16-453f-a8e0-7ace2b93f68f)

- The job of the **"Assembler"** is to take these instructions and convert it into its respective binary numbers aka **Machine language** program.
- These binary numbers aka machine language is then fed to the hardware, where hardware understands the type of pattern of the machine language and does the respective operation.

This is the flow how the application software runs through many different layers before entering the hardware.

lets take an example of a application of "stop watch".

- the C program for the stop watch enters the compiler and the output of the compiler will be intructions based on the type of the hardware(ex RISC-V) therefore the instructions will be of RISC-V.
- these intructions go into the assembler as a .exe file which gives the output in form of machine language.
- these hexadecimals are converted into binary before entering into the hardware.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/cd3ff2fc-9fd5-4a77-b1f2-4daa50b6ba1f)

- in general terms these binary numbers are entering into the chip layout and the functions are performed in this layout.

when we take a much deeper look into the program we try to understand the RISC-V intrucstions-

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/ac8b1be7-b508-4ee6-915e-243c495668cf)

- here we see that in left we have the input to the compiler,in the left we have the output of the compiler and the output of the assembler is in hexadecimal in the middle we have assembler output.
- the instructions after the compiler acts as an **"Abstract interface"** between the C language and the hardware, this Abstract interface is called as the **"Instruction set architecture"** or **"Architecture of the computer"**,in the case shown above its the RISC-V architecture.

There is another inteface between the Assembly language and the hardware which is known as the **HDL(Hardware discriptive language**.

#### What is RTL?

RTL stands for Register-Transfer Level. It's a level of abstraction used in digital circuit design and describes how data moves between registers and how operations are performed on that data.In RTL design, the behavior of the digital system is defined by describing how data is transferred between registers and how operations are performed on that data. This is typically done using a hardware description language (HDL) like Verilog or VHDL.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/52f5c6ce-ea1a-4bf8-8b89-dc223163f440)

- Our hardware only understands 1's and 0's therefore we need An RTL which implements the output of the assembly language that is the machine language into the Hardware.This is known as the RTL implementation of our instruction set.
- This RTL is then Synthesized into a Netlist, where this Synthesized Netlist of the RTL consists of Gates,flip flops,inverters,MUX's,etc.
- and from this Synthesized Netlist to hardware is **Physical design implementation of the Netlist**.

</details>

<details>
<summary> SoC design and OpenLANE </summary>

### Introduction to all components of open-source Digital ASIC Design

- To implement Digital ASIC design we require certain elements these elements are RTL IP's,EDA Tools,PDK data.

**What is PDK?**

- **"PDK"** a **(Process Design Kit)** is a set of files provided by semiconductor manufacturers to help designers use their fabrication process to create integrated circuits (ICs). It contains a comprehensive set of information, models, and files that enable designers to develop and verify their designs using the specific process technology offered by the manufacturer.


**What is EDA Tools?**

- **EDA** stands for **(Electronic Design Automation)**, and EDA tools refer to a category of software applications and tools used in the design and development of electronic systems, including integrated circuits (ICs), printed circuit boards (PCBs), and other electronic components.EDA tools are essential for designing and testing electronic hardware and ensuring that it functions correctly before it is manufactured.hese tools automate various aspects of the design process, making it more efficient and error-free. 

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/e2db98f6-b946-45ef-8c33-b2ce3ea80889)

- Therfore for making Open source Digital ASIC Design we have Open source for RTL IP's(librecores.org,opencores.org,github.com,etc),EDA tools(qflow,openROAD,openLANE,etc),PDK(Foss 130nm production PDK).

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/ce2d29cb-02c9-4f40-b68b-3472527cbea1)

- these 3 Elements can be used to achieve 100% open-source Digital ASIC design.

- The methodology is then Implemented Through A Flow, this Flow is nothing but a piece of software known as RTL to GDS2,this is the main objective of the ASIC Design Flow which takes the design from RTL(REgister transfer level) to GDS2 format used for final layout.


### Simplified RTL2GDS flow

The simplified RTL to GDS2 flow shown below starts from RTL model and ends with the readied fabricate masked set layout in the GDS 2 format:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/35deba59-3088-454f-86c8-e1733b02289f)

- From above the Major implementation Steps are:

  - Synthesis 
  - Floor planning(FP) + power planning(PP)
  - Placement
  - CTS(Cook Tree Synthesis)
  - Routing
  - Sign off

##### 1) SYNTHESIS:

- Its the first major step in a typical ASIC flow is the Synthesis.
- In Synthesis Design RTL is translated into Circuits made out of components from standard Cell Library(SCL).

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/9c66b611-e157-4d34-bd01-39fabcb905a1)

- The resulted circuit is described in HDL(Hardware descriptive language) usually referred to as gate level Netlist.
- The Gate level Netlist is functionally equivalent to RTL.
- The Library building blocks or the cells have regular layout,Typically the cell Layout is enclosed by fixed hieght standard.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/043a92e1-9929-4d14-92af-be6c14886769)

- The cell width is variable but its Discrete.
- Each cell comes with different models/views utilized by different EDA Tools
- some examples of these views are the liberty view that includes the electrical modeles,HDL.SPICE,Layout views,etc.

##### 2) FLOOR AND POWER PLANNING:

- The 2nd step of the ASIC flow is FLOOR AND POWER PLANNING
- It is based on weather we are implmenting single component of our design aka **Macro**,or we are implementing the whole Chip.
- The Objective here is to plan the silicon area and create Robust power Distribution Network to power the circuits.
- In **CHIP FLOOR PLANNING** the Chip Die is partitioned between different chip components.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/31295871-26ec-467c-b40d-7191cfc250e2)

- in **MACRO FLOOR PLANNING** we define the Macro dimensions and its pin locations and also Routing tracks and Rows are defined,Which will be used later during the placement and routing steps.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/1cd2f5ec-10e5-4e6c-8a52-dd06397c436f)

- In **POWER PLANNING** the power Network is constructed,typically its chip is powered by multiple VDD and Ground Pins.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/aac140a2-dc0f-493b-9963-766849970872)

- The Power pins are connected to all components through Power rings and vertical and horizontal metal Power Straps.
- Such parallel structures are meant to reduce the resistance.
- Typically the Power distribution Network uses Upper metal layers as they are thicker than lower metal Layers,Hence they have less resistance.

##### 3) PLACEMENT:

- This is the 3rd step in ASIC design flow it is the placement for Macros.
- We place the Gate level Netlist cells on the vertical Rows, connected cells must be placed very closed to each other to reduce the inter connect delay and also to enable successful routing afterwards.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/5f3d41fa-057c-4f10-9cde-1a9c2b81d790)

- Cell Placement is done in 2 steps:
  - Global placements
  - Detailed placements.
- **Global placements** tries to find optimal postions for cells such positions are not nessecerily legal so cells may overlap or Go off Rows

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/ca85947d-d212-4dea-b1de-afd9ddd1b35d)

- In **Detailed placements** positions obtained by global placements are minimally altered to be legal.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/f759cd33-413e-4199-8feb-86d489060552)

##### 4) CLOCK TREE SYNTHESIS (CTS):

- This is 4th step in ASIC flow design is the Clock tree synthesis, before the routing the signals we need to route the clock by creating clock distribution Network to deliver the clock to all clock cells ex(Flip-Flops).

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/34e9e0eb-b116-4e60-b9a3-e759d1cf431c)

- The clock network looks like a tree where the clock source is the Roots and the clock elements are the end leaves.
- The clock tree is synthesized to deliver the clock to all cells in a good shape with minimum skew and minimum latency.
- Clock skew means arrival of different components at different parts.

##### 5) ROUTING:

- This is the 5th step in ASIC Design Flow known as the Routing.
- After Routing the Clock comes the signal Routing, Given the placements and fixed number of metal layers it is required to find a valid pattern of horizontal and vertical wires to implement the Nets that connects the cells together.
- The Router uses the available metal layers as defined by the PDK, for each metal layer the PDK defines the Thickness,pitch,tracks and the minimum width.
- The sky130 PDK defines extra layers the lowest connecting layer is called the lower interconnect layer,the following 5 layers after this are all aluminum layers.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/a3ebd737-9146-4400-8447-abba4d5b033a)

- Most Routers are grid routers, they construct the routing grid out of the metal layer tracks.
- As routing is huge Divide and conguer is usually used for routing.
- First Global Routing is performed and then followed by detailed routing.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/db46cc8a-cd81-4009-b3e0-7ff9b4f20a33)

##### 6) SIGN OFF:

- This is the last step of ASIC Design flow known as the Sign OFF.
- Once done with Routing we can construct the final layout,Which undergoes Verifications which includes Physical Verifications and Timing Verifications.
- The **Physical Verification** includes the **DRC(Design Rule Checking)** where we make sure that the final layout honours all design rule.It is then proceded by **LVS(Layout bs Schematic)** that makes sure that the final layout matches the Gate level Netlist.
- The **Timing Verifcation** has **STA(Static Timing Analysis)** to make sure that all time constraints are matched and the circuit Runs on the designated Clock frequency.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/4b7c477c-f2fe-444a-b9fc-0b793149aee4)

### Introduction to OpenLANE and Strive chipsets 

#### What is OpenLANE?
**OpenLANE*** is an open-source digital ASIC (Application-Specific Integrated Circuit) design flow and toolchain that aims to automate the process of designing and manufacturing custom silicon chips. It was primarily developed by efabless, an open-source semiconductor company, and it is part of the Google/Skywater 130nm PDK (Process Design Kit) based ASICs.

- Started as a Open-Source flow for a true Open-Source Tape out experiment.
- There is family of Soc's called as striVe,which is Open PDK,Open EDA,Open RTL.
- this family has several members as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/638433b3-244a-4573-8d78-33e507aea9a0)

The Main goal of OpenLANE is to produce a clean GDS2 with no human intervention,by clean we mean that there is no LVS violations,DRC violations.

- OpenLANE is tuned for SkyWater 130nm Open PDK, it also supports XFAB180 and GF130G.
- it is functional out of the box.
- intructions to build and run natively will follow
- OpenLANE can be used to harden Macros and chips.
- it has two modes of operation:
   1) Autonomous : Autonomous is a push button flow,where we configure the flow and then push button and wait for some time based on the design size and get the final GDS tool.
   2) Interactive : With Interactive we can run commands and steps one by one.we can watch the immediate results whicle we run the steps and commands.
- It has Design Space Exploration which can be used to find the best set of flow configuration.

### Introduction to OpenLANE detailed ASIC design flow 

- OpenLANE ASIC flow has many steps as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/d396300d-34ed-4b89-bf42-19e2e589853c)

- The flow starts with Design RTL and ends at GDS2 format,to function this it needs the PDK Sky130.
- OpenLANE is based on several Opensource projects such as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/c0aff382-8fda-4f95-b165-1285a0503b09)

- The flow starts with RTL synthesis where it is fed to Yosys with design constraints.Yosys translates the RTL into a Logic circuit.This circuit is then optimised and then mapped into a cell using abc,abc has to guided during Optimisation,this guidance comes in the form of abc Script.
- OpenLANE comes with several abc scripts referred as Synthesis Strategies.Different Designs can use different Strategies to achieve the Objectives and for that we have the Synth exploration utility.
- Synth Exploration utility can used to generate a report that shows about the design delay and area is effected by the Synthesis Strategy and based on this exploration we can pick the best strategy to continue with.

  ![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/083b930b-6393-44c8-ad75-3462fe761a51)

- OpenLANE also has Design exploration utility which can be used to sweep the design configurations.It generates a report shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/67d52c2d-9c0d-47c3-bebf-7f2328bd7c46)

- It shows different design metrics, number of violations generated after generating the final layout.
- This is best to find the design configurations for OpenLANE for any given Design.

- The design Exploration is also used for **Regression testing(CI)**

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/a484e953-8899-4a35-8b64-3a60bf366743)

- After Synthesis comes the testing structural insertion,if we want our design to be ready for testing after fabrication we can enable this step called **DFT** which is optional.
- **DFT(Design For Test)** used Opensource Fault to perform scan insertion,Automatic test pattern generation(ATPG),Test patterns compaction,Fault coverage,fault simulation.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/ed81342c-ba87-40bd-a52d-4c44a9597ecf)

- After DFT the **Physical Implementations** involve the following steps shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/b709b4e9-103d-4587-9110-7585272c623d)

- All these are done by the OpenROAD app.
- After Physical implementation we Do **LEC(Logic Equivalence Check)** using Yosys.
- LEC is used to formally confirm that the function did not change after modifying the Netlist.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/af709cb7-20a8-4571-a447-6518cdcffae3)

- During Physical implementation we have an important step and this step is known as **fake antenna diodes insertion**
- This step is required to address the Antenna diodes violations but there are some issues as explained in the figure:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/48bfde76-7a14-4821-837d-ff4c8ea71d7c)

- To deal with this and not allowing our transistor gates to get damaged during fabrication there are two solutions:
1) Bridging:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/7c652e2c-277c-4d8f-930c-c07dbed481d5)

2) addition of Antenna diodes:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/a6401584-e5a6-43c0-a404-ecef0b44938f)

- With OpenLANE we take a preventive approach:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/b2b53089-5391-45a0-834c-8c2dd51a8089)

- After this process we have the SIGN OFF which has the Static timing analysis,design rule checking and Layout vs schematic.
- Timing Sign off involves interconnect RC extraction from the routed Layout followed by static timing  analysis performed by OpenSTA(an OpenROAD) tool.
- The result of this report Highlights any timing violations of if any are present.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/198cdc0f-616a-4434-9560-f47c9ab5a597)

- Physical Sign off involves DRC and LVS done by Netgen and Magic VLSI design tool shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/b8580063-6b56-43f4-8726-e554d8808022)

</details>


<details>
<summary> Get familiar to open-source EDA tools </summary>

#### OpenLANE Directory structure in detail

- To access the Openlane go to `desktop/work/tools` and then change directory to `openlane_working_directory`.
- to access the PDK's chnage directory to `pdks` and then we see the different PDK's.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/9b2ef07b-94c7-4a14-b404-53f824c89e52)

- The `SkyWater.pdk` is the folder that has all the related PDK files.(ie: timing libraries,lib files,techlib)
- `sky130A pdk` is that pdk that has made compatible to Open source environment,`sky130A` is a varient of PDK.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/cb5f47c3-f15b-4b91-b42f-1c393a66a276)

- when we check the files inside the sky130A pdk we have libs.ref and libs.tech.
- inside libs.ref we will be working with `sky130_fd_sc_hd`
- where `sky130` is the process name and `fd` is the abrievated foundry name and `sc` is known as standard cell and `hd` says the varient of the PDK.

#### Design Preparation Step

- To work on Openlane type `docker` in the openlane directory.
- in bash type `flow.tcl -interactive`.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/6b39e5f6-d8c4-435e-a211-8937aee055c0)

</details>

# DAY-2

## Good floorplan vs bad floorplan and introduction to library cells

<details>
<summary>  Chip Floor planning considerations</summary>

### Utilisation Factor and aspect ratio

- How to come up with the width and height of core and die

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/a67c205c-265c-461b-908c-1c7c2f5c62ae)

- we need to see how we come up with the values of 'W' and 'H'.

lets take an example of a netlit to Identify the width and height of core and die.

- we begin with a simple netlist takiing two D flip flips,aka launch flop and the capture flop with a simple combinational logic between them.
- a Netlist basically defines the connectivity between all the components.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/2080d268-5165-4e52-b1bf-0827d2bd686b)

- For dimensions of the chip we are mostly dependent on the dimensions of the Logic gate.
- we then convert it into physical dimension.

  ![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/eea1b776-03db-45b9-b309-ab3e8a4b2f78)

- since we want to find the dimentions of the core and die we will be needing the dimensions of the standard cells.
- we start giving some unit area for the each logic gate as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/d448e63b-fa86-4608-8b99-21fe88b90931)

- with the help of this netlist we try to calculate the area occupied on the silicon wafer.
- we club all together to form a rough image of the area the netlist occupied,(ie 4 sq units for the image shown below):

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/a10ce57c-b25d-4d09-8d30-856a40ffbc91)

- On the silicon wafer we have many die and core in it and this is the core section where the fundamental logic goes and sits into and die is the outer layer where our fundamental logic fits within this itself and does not exceed it.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/5d477e6f-f3d1-4c71-b109-d91003a1f6b2)
  
- we implment this die multiple times on the silicon wafer to increase the throughput.
- when we implment the logic into the core,the logic cells occupied 100% of the core,thereby occupying Utilising 100% of the core.
- To come up with the Utilisation, we have the Utilisation factor given by:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/c39e4e8c-817c-47fe-80b6-c6a522b4244e)

- for the above logic the `utilisation factor = 1`,when we get utilisation factor as 1 it means that our logic has occupied 100% of the core and ther are no gaps or spaces left to fill.
- Idealyy we go for Utilisation of 50 to 60% and Utilisation factor of 0.5 or 0.6.
- We also have **ASPECT RATIO**,aspect ratio refers to the ratio of the width to the height of a transistor. It is a critical parameter in the design and fabrication of integrated circuits.

- Aspect Ratio is given by-

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/6a4eb22f-7c79-4225-930b-d56c461419d6)

- In this case the Aspect ratio = 1, Whenever the aspect ratio is 1 it signifies that the chip is a square shaped chip.when the aspect ratio is other than 1 then it signifies that our chip is rectangle in shape.

### Concept of Pre placed cells

"preplaced cells" refer to specific components or modules that are manually placed in predetermined locations on the chip before the automated placement and routing tools are used to complete the design.These preplaced cells are typically larger and more critical components, such as processors, memory blocks, or custom-designed circuits.

- To define the locations of pre placed cells lets consider an example using a combinational logic, and the output of this combinational logic is a huge circuit.
- we need not implment this circuit as part of the main circuitry itself always but by taking this peice of circuit out of the main circuit and then implment it separately by dividing the circuit itself into 2 parts.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/864a0a58-4c3f-427f-bb01-32e7877536ae)

- Now we try separate these two into 2 different blocks where each block will be implemented separately.
- we take thw two blocks separately and extend I/O pins.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/72fd9433-5a5c-4351-89a5-97bdf14f6091)

- we then black box these blocks, when we do it this becomes invisible.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/f8111421-6b33-45e1-b918-f26331fe223f)

- we then separate the two blocks as two different IP's and modules.
- By doing this we can implment this one time and can be REUSED multiple times.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/ab13d592-0653-4adc-b050-d6bb98004765)

- similarly there are multiple IP's available as shown belo:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/f2ba8da9-0306-45e7-aa8d-c7de71355760)

- Arrangment of these IP's in the  chip is referred as FLOORPLANNING.

To place the pre placed cells in the chip lets consider three pre placed cells a,b,c, the chip has pins at the sides which can be output and input pins.

- since preplaced cells communicate with the inputs a lot we place them close to the input pins,once the pre placed cells are placed the locations cant be moved in the complete design cycle hence they need to be placed carefully

- We surround the preplaced cells with Decouplling capacitors.

### De-coupling capacitors 

**What are Decoupling capacitors?**

- Decoupling capacitors, often referred to as bypass capacitors, are components used in electronic circuits to stabilize and improve the performance of integrated circuits (ICs) and other electronic devices.decoupling capacitors are placed very close to the power pins of integrated circuits. They come in various capacitance values and are selected based on the specific requirements of the circuit.

- Having a large distance from the power supply and the main circuit has a dissadvantage as there are multiple volrage drops happening before it reaches the main circuit giving a less voltage at the main circuit due to voltage drops therefore we cant gaurantee that our logic gates in the circuit are getting either a high voltage(logic 1) or a low voltage(logic 0) or a danger region or gray region(Either Logic can go to 1 or 0 giving high or low volts) hence we have a dissadvantage of Voltage being far from our circuit design.

- To solve this we use Decoupling Capacitors are huge capacitors completely filled with charge,therefore if our main voltage is source is 1v our deocupling capacitors also get charged to 1V.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/bbed32e9-eedb-4538-a1f3-65c3efadc531)

- Whenever our main circuit switches on it gets the power from the decoupling capacitors as its near or attached to the circuit giving the proper current to the circuit,therby decoupling the main circuit from the power supply.
- Whenever the main circuit switches on the decoupling capacitors start losing the charge and when ther is no switching activity with main circuit decoupling capacitors spends time to replenish its own charge.

Hence we surround the preplaced cells with the decoupling capacitors in order to keep the current flow as required without any problems of voltage drops.thereby ensuring each preplaced cells are getting the supply from the Decoupling capacitors.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/ccbf4b56-0224-4f3c-8353-bb7cf7088145)

### Power planning 

Power planning, in the context of integrated circuit (IC) design, is the process of strategically distributing power supplies (such as VDD and VSS) and ground connections across the chip to ensure proper and reliable operation of the electronic components.Effective power planning is crucial for achieving reliable and high-performance integrated circuits.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/06416b6b-7635-4a51-99a5-85d3eb36385d)

- Consider the above circuit which we used for decoupling capacitors and convert it into a Macro,now this Macro is repeated multiple times on the chip creating a current Demand for each and every element of the particular macro.Now suppose one is driver and other is loader each macro have a decoupling capacitors and we need to send signal from driver to load, we need to make sure the particular line between the driver to load maintains the same particular signal.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/41e9a3d7-c39a-46cb-a307-cdfd9a8a2f31)

- The line between the driver and load should get the necessary power from the power supply as decoupling capacitors cannot be placed in between therfore having a possibility of voltage drop as the power supply is far from the signal line.
- Hence we consider a 16 bit bus connected to an inverter when we pass the logic to the inverter the output will be inverted value of the input therfore all the capacitors charged to logic 1 are now dischraged to Logic 0 and vise versa.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/a8c57105-979f-4a5e-9799-6697ee33a806)
 
- But since we have a common ground line for all the capacitors as they all discharge to logic 0 to gnd it gives a **Ground bounce**, and the size of this bounce exceeds the noise margin level it might enter into the undefined state or the danger region.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/5dcdeef8-49f1-42a8-a487-343556d49934)

- now when all the other capacitors charging from Logic 0 to logic 1 in that case all these capacitors are demanding for supply from the main power supply at the same time and we have a single voltage line for all the capacitors hence we get a **Voltage Droop**

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/b0d9fcfc-0000-48ef-bed4-3d4dee8d1fb0)

- Therefore the problem is that the supply is from a One source power supply creating multiple power supply wud reduce this problem, hence we do **"POWER PLANNING"**.
- We put multiple power supplies instead of single one by creating multiple vdd and vss lines,therby giving any power supply demand to the circuit.
- The power planning structure is shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/aa922e04-bc35-4e15-a2ba-e172d6e51429)

### Pin placement and logical cell placement blockage

**What is Pin placement?**

- Pin placement refers to the process of determining the physical locations where the input and output pins of a digital IC will be placed on the chip. These pins are the electrical interfaces through which the IC communicates with the external world, connecting to other components on a printed circuit board (PCB) or another IC. Proper pin placement is crucial for efficient routing of signals and minimizing signal integrity issues.pin placement blockage restricts the placement of input and output pins.

- Logical cell placement involves determining the physical locations of the individual functional blocks or cells within an IC. Each cell represents a specific digital logic function (e.g., gates, flip-flops, multiplexers) and their arrangement on the chip affects factors like signal propagation delays, power consumption, and ease of routing.This specifically refers to the restriction of placement of logic cells. Certain areas of the chip may be designated as off-limits for placing logic cells. This could be due to predefined constraints like reserving space for custom analog blocks, clock distribution networks, or other critical components.

- lets consider the given designs to be implmented along with some pre placed cells as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/510cd2d7-ce52-4c59-88c2-72d0e69e90d3)

- From above image we currently have 4 input ports and 3 outpur ports
- lets take 2 more designs but both are driven using different clocks with a common pre placed cell as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/2b6e8e8c-768b-4b4a-b262-8bd5944fa601)

- Now we we try implement this complete design into the chip:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/42e53547-8112-4c98-8fcf-31a936052b10)

- While placement of pins it depends and varies on design structure, we can have input on top and output on bottom or input at left and output at right.
- Clock 1 and clock 2 drive the complete chip.
- Bigger the size lower is the resistance hence we need the clock signal to move In and out of the chip as fast as possible as its driven continously hence we need least resistance path for the clock therfore we create bigger boxes for clock 1 and clock 2 and clock out.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/713c2968-a3fc-4656-a3e4-abf16198dd0a)

- After Pin placement we make sure that none of the automated placement and routine tool doesnt place any cells in the particular area that the gaps between each clock ports,the area should be blocked for placement and routine tool,hence we do logical cell placement blockage.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/c6813abb-f30d-439c-9df8-cb99b76da601)

###  Steps to run floorplan using OpenLANE

- To run the floorplan on OpenLANE type `run_floorplan` in OpenLANE directory after run_synthesis and basic steps to run OpneLANE.
- to see the actual Layout after floorplan it is first done in magic.
- to access magic type `magic -T` with the directory of our deisgn file we want to access using the magic application.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/8f4e8927-ab8f-4fb0-8aea-ee1ddc3a8be5)

### Review floorplan layout in Magic

- after opening the required file in Magic we can now try to see and reivew the floorplan.
- to keep the the design floorplan in the middle of the screen press V.
- to zoom in we select the part of the design we want using left to select the portion of the region we want to access and then left click to to select that region and press Z to soom inside that region.
- we see the selected region as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/d1868204-5ab2-4ff9-80b9-1c68716da681)

- we want to zoom inside this and we can see the cells used inside this as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/8d9965f6-6416-41aa-9278-2278914f7da6)

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/c10ad79e-a303-4306-a499-7a3f5962e8f1)

- apart from the deisgn view window we also have a tkcon window where we open and type `what` inside the tkcon window it shows us which layer is connected to which pin and it also shows the selected metal layers.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/349fb9a8-93d9-42f2-bccf-917b0744c77b)

</details>

<details>
<summary> Library Binding and Placement </summary>

### Netlist binding and initial place design

**Placement and Routing**

- The  most important step in placement and routing is to bind the netlist with the physical cells.
- Consider the particular netlist with all these Gates and the shape of the gates represent the functionality of the Gates,but in reality we dont have shapes like the ones shown below we have it in box type with width and height of the particular cell.
- so at the end we will be having each logic gate with a shape and the pre placed blocks and we will be left out with wires.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/bf88ba01-e400-4812-8e46-80847b86c555)

- Now these all blocks of shape are now present in a shelf known as Library, Library contains various types of blocks including these(ex flip flops,AND gate, Or gate ,etc)

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/f72ed276-1089-402d-992a-d57654d3caee)

- The library also holds the information of each logic gate like delays and etc,the library can be classified into either 2 types one that holds the shapes and one that holds the information of each logic gate.
- The library will have the information of the shape the width and height,the delay information of each and every cell and the required condition of the particular cells.
- The library also holds different flavours of the cell it tries to store(ex if the 2 block is an and gate the library also shows another same AND gate but a bit bigger in size,least resistance path than the normal one as its bigger in size therby being faster compared to the normal one),therfore it has flavours of each and every cell we try to store it in.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/57bb1a47-9e68-49cb-a7bd-e963c3b8df1c)

- we can pick what we want to use based on our available space on floorplan.
- Therefore in summary library consists of everything it consists of cells,shapes and size of the cells,various flavours of the same cells and the timing and delay information.

Once we have given proper shape and size and delay information of our cell using the library the next step is to take this cell and place it onto the floorplan,so we have the floorplan,we have the netlist,we have the physical design view of the netlist in form of logic gates.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/798ac757-a04e-458b-813b-337e6137dc14)

- The netlist wont come into picture as we will be using the Physical view of logic gates though we will be following the connectivity information from the netlist itself.

**How this is done?**

- The placement stage will make sure that the pre placed cell locations are not are not affected and kept as it is and there will be no cells that can be placed in that area.
- we now place each of the shape cells from the physical deisgn view of logic gates in a proper manner such that ther are no delay contraints,we place them in such a way that they are close to thier respective input and ouput port pins, we place them close because if FF2 was placed somewhere below and the distance from FF2 to dout1 wud be higher therby having more timing delay to communicate with the output pin.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/37a6fc52-71d7-4533-b0b8-b0edfd7e407e)

- From above we clearly see that FF1 is placed near to Din1 and FF2 is placed near to Dout1 as they are connected close to each other using the Netlist as refrence we fill all the other the same way.

- we place the 2nd stage of the netlist in the way shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/6ec6568e-572e-45ab-9d3c-edacb0f916f1)

- Here we see that in 2nd stage FF1 is not close to Din2 for and FF1,cell 1,cell 2, FF2 the delay between FF1 and 1 will be very minimal and similarly the delay between 1 and 2 is also minimal, we do this beacuse of some reasons given ahead.

### Optimize placement using estimated wire-length and capacitance

- The 3rd stage to be placed we see that FF1 needs to be connected to Din3 and FF2 to Dout 3 but the distance between them is huge hence we try to place them diagonally as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/82fc8dd9-2f0a-45de-9ef1-52139c26b9a9)

- Similarly implementing stage 4 in quite tricky as we have pre placed cells and we cant give FF1 close to Din4 therefore the distance is huge again in stage 4 as there is again a diagonally opposite I/O ports for stage 4 on the chip.
- we place the stage as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/7629e9c8-d647-4f0a-a8a3-d3364188d0c0)

Now we try to solve the Problems that we encountered while placing these cells, the Solution for these Problems is known as **Optimize placement**.
  
- This is the stage where we do estimations where we estimate the wire length,capacitance and based on that insert **repeaters**.
- lets consider FF1 of 2nd stage and din2 we see that capacitances between them is very huge as its huge length of wire and even the resistance as it depends on length and lenght is huge.Therefore the signal delay is high from din2 to FF1 of 2nd stage due to the distance.
- we fix this problem by placing a **Repeater** in between Din2 and FF1 of 2nd stage to pass on the signal thereby reducing delay and buy having loss of data,therfore whatever is told to Din2 is succesfully retained by FF1 of 2nd stage and This is called **Signal Integrity**.

**REPEATER**

- are basically buffers that will recondition the old signal and make a new signal which replicates the original signal and sends it again, in this way many repaeters are placed and signal integrity is mmaintained But at a loss of area as more and more repeaters will be used for long distances which is a trade off.
- In the 1st stage we dont need any repeaters, Signel Integrity is based on the wire length estimation and calculation.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/bd7558cb-33ee-4381-8e80-fa9573be547b)

- **SLEW** is basically depended upon the value of the capacitor,higher the value of capacitor the amount of charge required to charge the capacitor will be high resulting in BAD slew.
- In stage 2 we see that the distance was far from Din2 and FF1 of stage 2,slew is basically transmission and it goes beyond the limit in the 2nd stage and resulting it in more difficulty in reaching the FF1,therfore we add some repeaters to it as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/b662d633-1f14-4c95-b884-6df0e3c7bab1)

- In stage 2 we have abetted certain logic so that there is no time delay between 1 and 2 and recreating signel will not be a problem as all are close to each each.
- The reason we have done abetment to FF1 ,1,2,FF2 of 2nd stage is because we have assumed our 2nd stage to work at a very high speed in that case we bring these set of logic very close to each other thereby having 0 delay from sending signal from FF1 to FF2 hence we have kept them together but far from thier respective ports.
- The stage 3 is placed as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/277c4ff6-39a0-4e1e-9576-c6ba941fc9b7)

-The stage 4 is placed as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/933e7d7d-15a0-4849-8c5b-f597343b8be1)

### Need for libraries and characterization

Typical IC design flow that every design needs to go through if it wants to be implemented on a chip are:

1) 1st step is the Logic synthesis,output of logic synthesis is arrangment of gates in thier original functionality that we have described using RTL.
2) 2nd step is the Floorplanning,in this step we import the Netlist that we get out of logic synthesis and decide the size of the core and die.
3) 3rd step is the Placement step we take the logic cells present from the logic synthesis and place it on the chip in such a manner that the initial timing is met.(ie we place the fast ones together and the ones with different functionality we keep them depending on that).
4) 4th step is the CTS(Clock tree Synthesis), if we want the clock to be spread across the logic cells at equal time (ie: all flip flops sitting far or close apart should recieve clock at the same time) therfore in CTS we attack a tree which controls the clock for each logic cells.
5) 5th step is the Routing stage , if we want to route each cells there are certain flow routing has to go through and it is depended on the characteristics of the cell.
6) 6th step is the STA(static timing analysis) we do static timing analysis to find out what the setup time, hold time,maximum achivable frequency of circuit.

From all these steps we see that there is one thing common and that is the **Gates oR cells** ,this is where **Library characterization** plays and imporatant role,the collection of these cells is known as library when placed in some area. we introduce these gates in a manner such that the tools understand what these gates are, we need to model them in a way that the EDA tools can understand it.

### Congestion aware placement using RePlAce 

Placement in OpenLANE occurs in two stages:
1) Global placement: one of cores placement where legalisation does not happen.Legalisation means the standard cells are placed in standard cell rows and should be abetted with each other and should have no overlaps.The main objective of global placement is to reduce the wire length.
3) Detailed placement

we have different tool to do both the functionalities.

- since global placement is to reduce the wire length in openlane we use concept of hpwl(Half permitable wire length),therefore it focuses on reducing this hpwl.
- once we `run_placement` in openlane we see that the hpwl values converge basically the length is reducing.
- to see the placement in openlane type `magic -T` with the required file location of the placement file.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/05a96562-7ebd-4099-a554-63718209177a)

- For detailed view of the cells we zoom inside as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/9baae53c-3a26-4481-9371-ed3ea55a5c0d)

- from rhe image shown below we can clearly see how the cells are arranged in row wise and how the placement looks like:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/9f0c8087-b039-4d68-9328-7d1c22da48dd)

</details>


<details>
<summary> Cell design and characterization flows</summary>

### Inputs for Cell design Flow

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/c18e62cc-d646-4ba0-9ad7-71f14bb7cd11)

- The above shown image is a placement and routed version of the logic synthesis step.
- all the cells from this they are also known as standard cells, Standard cells in the context of digital integrated circuit (IC) design, are pre-designed, pre-characterized, and pre-verified electronic building blocks that represent fundamental logic functions like gates, flip-flops, and latches. These cells are typically provided by semiconductor foundries or third-party IP (intellectual property) vendors and serve as the basic building blocks for designing complex digital circuits.
- These standard cells are placed and kept in a place called the libraries,library not only hold cells but they also hold differrent cells with different functionality and different sizes.
- library not only holds the different shapes of the cell but also thier characteristics like threshold voltage and other variations.

- lets consider a cell an inverter for us its nothing but a single input cell but for an inverter it goes through a typical steps of cell design flow.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/cd8fe8f9-d6b9-4ac4-bbdf-082fd9a4bc1c)

**CELL DESIGN FLOW**

- The cell design flow is devided into 3 steps:
1) inputs : these are the inputs needed to design the inverter.
2) design steps: designing of the inverter
3) output: outputs of this inverter are the ones that are used by the EDA tools.

##### INPUTS

- To the design the inverter we start with PDK's(Process design kits),PDK stands for Process Design Kit. It is a set of files and documents that contain information essential for the design and manufacture of integrated circuits (ICs) or semiconductor chips. A PDK is provided by semiconductor foundries to their customers, typically IC designers, to assist in the development of ICs using their specific manufacturing processes.These kits contain the DRC and LVC rules,SPICE modules,libraries and user defined specs.
- The DRC and LVC rules provides us a tech file with some rules of designing the layout or it gives design rules.
- There are many rules the foundaries give us thousands of rules before we start designing out the cell as shown below.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/38a1b5bb-b044-44d7-92b5-0485386ce332)

- SPICE modules give us the parameters while we design ex threshold voltage,linear region and saturation region formulas,etc.
- These are the parameters that are provided by the foundry as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/e0ec8069-a7c6-4ba6-9490-2d6be2a599b2)

- The library and user defined specifications tells us the cell height and the power rail and the ground rails used inside the core of the chip as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/2b64b3ac-7b63-49eb-b9a4-9abff3a9ce45)

- The seperation between the power rail and ground rails decides the **Cell Height** and it is responsible by the library developer that the cell height cell is maintained.

### Circuit Design step

##### DESIGN

Circuit design is then divided into 3 steps:

1) circuit design 

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/a5c365a1-3959-4048-b145-f92fd8e21a4e)

- mostly depended upon the SPICE simulations where the standard objective is to maintain the drain current of PMOS and NMOS should be equal to 0 
- the ouput that we get out of the circuit design is known as the CDL(Cricuit Design Language) File.

2) Layout design

- in layout design we try to get the PMOS and NMOS network graphs out of the implemented design.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/4f9030f2-2dd8-46c9-818f-47f7b578d730)

- The next step is to use eulers path and draw stick diagram out of it.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/db145dbf-0f6a-4915-a208-df9d78f2856a)
  
- The output of the layout Design will be GDS2,LEF(defines width and height of the cell),extracted spice netlist.

3) charecterisation

- lets take example of the layout of a buffer as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/e98dc2f0-98f4-4482-a0d9-91690e7f02d5)

- we have the description of this buffer as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/e152b496-74eb-4c22-864e-9551cf60a5da)

- for this we have spice extracted Netlist basically whatever we have in the Layout buffer the contacts the metal layers and everything for each element will have a resistance and capacitances we have extracted them all in terms of a spiced Netlist as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/34682b40-10f2-4c5c-8ea3-e97099383c27)

- for this we have the sub circuit file loaded, it contains the actual PMOS and NMOS models as shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/8fb79374-51b5-46f6-8b83-f8f6ad17b454)

- we have then read the models of PMOS and NMOS as shown above:
  
- The characterisation flow that is followed in the industry are:

1) 1st step is to read the Models as it is the first file that comes out of the foundry
2) 2nd step is to read the extracted spice netlist .
3) 3rd step is to recognise the behaviour of the buffer in above case or the Logic gate we have implmented.
4) 4th step is to read the loaded sub circuit file.
5) 5th step is to attach the necessary power sources.
6) 6th step is to apply the stimulus.
7) 7th step is to provide necessary output capacitances and should be varied in a range.
8) 8th step is to provide the necessary simulation commands.

- The next procedure is to feed in all these 1 to 8 steps in form of a configuration file into the charecterisation software called as **GUNA**.
- This software generates timing,noise,power,.lib,fucntions files.
- therefore the characterisation gets sub divided here into timing,power,noise characterisation.

</details>

<details>
<summary> General timing characterization parameters </summary>

### Timing Threshold definitions

**TIMING CHARACTERISATION**

- From the descriptive image of the buffer shown during the characterisation comes to an understanding of different threshold points of a waveform itself which is called as **Timing Threshold Definitions** .The timing threshold of the above image is shown below:


- The output of the waveform looks like this shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/acfe83fd-a021-4120-8971-2826dd394042)

- The above waveform is to understand the Slews of thee waveform slew rise shows the red rising graph and slew fall is shown by the blue falling graph each of ther graph have high and low values.
- similarly we have it for input rise and fall and output rise and fall, the input rise and fall is shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/6e70990b-bab6-4f63-95f4-1d7e6919c771)

- The output rise and fall is shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/61d3ab57-d5cf-4a77-ac29-ae1dcec594cb)

- NOTE that we are finding the waveforms only for timing stimulus that was provided in step 6

### Propagation delay and transition time

**PROPAGATION DELAY**

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/e3b7556e-b97b-4efd-a61e-1a08bb1349f9)

- To get Delay we subtract out - In of the above graph.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/62870a82-f5c3-4b6c-93bf-f1611668900f)

- From above graph we see that our output blue comes before our input red thats why we see negative delay here which are not expected, The reasaon behind the negative delay is poor choice of threshold points,therfore choosing the threshold points are very important.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/f52df3fd-c572-4469-84d8-cf19f3ed2b50)

- From above diagram we have choosed the correct threshold point but still get negative delay but still our output comes before our input as shown below for the above diagram when we zoom in at the threshold point choosed.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/6bef4bb1-35b5-451f-9731-da127328b42f)

**TRANSITION TIME**

- to find slew of a waveform we need to do time(Slew_high_rise_thr) - time(Slew_low_rise_thr)
- The transtion is showed below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/2f60375d-8c3a-4abc-b1b7-2d1aa9932b42)

</details>

# DAY-3

**Design library cell using Magic Layout and ngspice characterization**

<details>
<summary> Labs for CMOS inverter ngspice simulations </summary>

### I/O placer revision

- 4 strategies supported by the I/O placer.
- I/O placer is one of the open source EDA tools that is used to place around the core.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/18b8b0e0-cb5d-46d7-880c-825361760e78)

- Here in above image we see that all the I/O pins are located at output equidistant of each other.
- to view the floorplan mode we can go to `floorplan.tcl`

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/0d7cbed0-21e0-4109-b871-ea59c395efae)

- we try to modify the floorplan by changing the mode to 2.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/efb14c69-2f2a-483f-a1fa-ed58d62b3a32)

- after modify run floorplan, we get a structure of I/O pins which are stacked on top of each other.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/a9f83086-7163-4848-b959-83a44b76ba11)

### SPICE deck creation for CMOS inverter

### SPICE simulation lab for CMOS inverter 

### Switching Threshold Vm

### Static and dynamic simulation of CMOS inverter

### Lab steps to git clone vsdstdcelldesign 

- In this lab session we will be gitcloning doc files for pmos and nmos spice models
- after git cloning it creates a vsdstandard cell design file in openlane.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/10fb0419-3696-4d50-8baa-57d2c4ea0fc7)

- we copy the sky130A.tech file to the vsdstddesign file to make it easier to run and type magic -T sky130A.tech and the mag file which is inverter.

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/04a06c22-2110-48b7-bb8a-ef446f4a80ac)

- when seen in magic it looks like something shown below:

![image](https://github.com/Tawfeeq2507/pes_pd/assets/142083027/9856913e-b400-4f18-b4ef-3715ff6fef22)

</details>






































