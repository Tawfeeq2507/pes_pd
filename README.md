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


- From above image we currently have 4 input ports and 3 outpur ports
- lets take 2 more designs but both are driven using different clocks with a common pre placed cell as shown below:

- Now we we try implement this complete design into the chip:

- While placement of pins it depends and varies on design structure, we can have input on top and output on bottom or input at left and output at right.
- Clock 1 and clock 2 drive the complete chip.
- Bigger the size lower is the resistance hence we need the clock signal to move In and out of the chip as fast as possible as its driven continously hence we need least resistance path for the clock therfore we create bigger boxes for clock 1 and clock 2 and clock out.


- After Pin placement we make sure that none of the automated placement and routine tool doesnt place any cells in the particular area that the gaps between each clock ports,the area should be blocked for placement and routine tool,hence we do logical cell placement blockage.

 
















































