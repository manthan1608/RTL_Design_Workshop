![Verilog-flyer](https://www.vlsisystemdesign.com/wp-content/uploads/2021/05/Verilog-flyer.png)
# RTL_Design_Using_Verilog_with_SKY130_Technology_Workshop
This is a repository on RTL Design using Verilog with Sky130 Technology workshop conducted by VSD.It is a day by day log including examples from Lab sessions and Theory sessions
## Table of Content
- [Introduction](#Introduction)
- [Day 1 Introduction to Verilog RTL Design and Synthesis](#Day-1-Introduction-to-Verilog-RTL-Design-and-Synthesis)
- [Day 2 Timing libs , Hierarical vs Flat Synthesis and efficient Flop Coding Styles](#Day-2-Timing-libs-,-Hierarical-vs-Flat-Synthesis-and-efficient-Flop-Coding-Styles)
- [Day 3 Combinational and sequential optimization](#Day-3-Combinational-and-sequential-optimization)


### Introduction
This Workshop intends to teach basics of digital design using verilog language, various RTL coding styles,synthesis and problems faced by industry and how to solve them in Verilog using SKY130 Technology.\
Some of the open source eda tools used in this Workshop are
1. [iverilog](http://iverilog.icarus.com/) : It is Simulator which used for RTL Simulations and Gate Level Simulations.
2. [yosys](http://www.clifford.at/yosys/) : It is a open source Synthesis tool used for synthesis of RTL Design.
3. [Skywater](https://skywater-pdk.readthedocs.io/en/latest/rules/background.html): It is used as 130nm Standard Cell Libraries.

### Day 1 Introduction to Verilog RTL Design and Synthesis 
#### Introduction to open source simulator iverilog
***Simulator***\
RTL design is checked adherence to the spec by simulating the design.Simulator is the tool used for simulating this design.\
We have used iverilog in this workshop for simulation.\
***Design***\
Design is the actual Verilog Code or set of verilog codes which has the intended functionality to meet with the required specifications.
***TestBench***\
TestBench is setup toapply stimulus (test vectors) to the design to check its Functionality.\
How the Simulator Works ?\
- Simulator looks for chenges in input signal.
- Upon change in input the output is observed.
   * If there is no change to the input there will be no change to the output.
- Simultor looks for changes in the Value of input.\
The TestBench looks something like this 
![day1_1](https://user-images.githubusercontent.com/84860957/119926834-c1f4a480-bf95-11eb-956d-3e86a5884e3b.JPG)

***Note***: 
> *Design may have 1 or more primary inputs,1 or more primary outputs.
> TestBench doesn't have a Primary inputs or a primary outputs.*

**iverilog Based Simulation** 
- Design and Testbench are given to iverilog.(*As we know any simulator looks for the changes in input*.)
- The output of iverilog simulator is ***vcd file***.
  * Here ***vcd*** stands for *Value Change Dump* format.
- To view the *vcd file* we use another tool called ***GTKWave*** which is used for viewing the waveforms.

***LAB Session***
*Lab 1*\
It is introductory lab session in which we look at environment setup  for running the labs.\
Before cloning we can create directory *vsd* and inside it another directory *VLSI* using the `mkdir` command as demonstrated below
![day1_3](https://user-images.githubusercontent.com/84860957/119930591-59112a80-bf9d-11eb-9219-c4e22f6ac0a0.JPG)\
After creating the *VLSI* directory. We have to clone a repository named [sky130RTLDesignAndSynthesisWorkshop](https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop) which contains the required library files and verilog design files to perform the simulations and logic synthesis parts of the workshop. It can be done using by using basic linux command `git clone` `git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git` as demonstrated 
![day1_4](https://user-images.githubusercontent.com/84860957/119931069-4f3bf700-bf9e-11eb-8704-5f452f013bb3.JPG)\
After succesfully cloning it creates a sky130RTLDesignAndSynthesisWorkshop directory inside the VLSI directory. It contains various files required for this workshop you can explore the files such as my_lib which contains the library files lib and verilog_model.verilog_files which contains various verilog codes and its testbench files for all the lab expirements.\
\
![day1_5](https://user-images.githubusercontent.com/84860957/119931749-97a7e480-bf9f-11eb-8251-bbacd731393d.JPG)\
\
![day1_6](https://user-images.githubusercontent.com/84860957/119931833-c625bf80-bf9f-11eb-9828-cc3df4326cd1.JPG)\
\
![day1_7](https://user-images.githubusercontent.com/84860957/119932080-44826180-bfa0-11eb-91d3-32974ad790c7.JPG)\

*Lab2*\
In this Lab we are introduced to ***iverilog*** and ***GTKwave***.\
We use the command `iverilog` to load the simulator follwed by the verilog file and testbench name. `a.out` file is created in the verilog_files folder
![day1_8](https://user-images.githubusercontent.com/84860957/119934359-47328600-bfa3-11eb-8da5-bf84c6665f7c.JPG)\
\
After creating a.out file we execute it using `./a.out` it is going to dump the *vcd* file \
![day1_9](https://user-images.githubusercontent.com/84860957/119934727-e0619c80-bfa3-11eb-86db-e8d59ee90e4d.JPG)\
\
Then we write the command `gtkwave` followed by the vcd file name like `gtkwave tb_good_mux.vcd` \
![day1_10](https://user-images.githubusercontent.com/84860957/119935621-692d0800-bfa5-11eb-9e0f-52e7fa85a15e.JPG)\
\
We get the following waveform of a 2:1 Multiplexer when sel = 0 output follows i0 and when sel = 1 output follows i1  in the GTKWaveform viewer\

![day1_11](https://user-images.githubusercontent.com/84860957/119935442-22d7a900-bfa5-11eb-99fe-d9c360cdff42.JPG)\
\
We also observe the verilog code of the good_mux.v and tb_good_mux.v by using the command `gedit` (also `vim` and `gvim` can be used in systems where gvim it is installed)
![day1_12](https://user-images.githubusercontent.com/84860957/119936874-76e38d00-bfa7-11eb-9922-7bd95a6df536.JPG)\
\
We get the good_mux.v verilog code .There are various codes to generate a MUX we have used the `if` statement for it \
\
![day1_13](https://user-images.githubusercontent.com/84860957/119936876-777c2380-bfa7-11eb-93dc-73665b2bb025.JPG)\
\
and the testbench tb_good_mux.v we can see the instantiation of design and the stimulus generator and it runs for 300ns. \
\
![day1_14](https://user-images.githubusercontent.com/84860957/119937709-e7d77480-bfa8-11eb-80e8-cba55d46780a.JPG)!

**Introduction to yosys**\
***Sythesizer*** \
It is a tool used for converting RTL to Netlist . We use ***Yosys*** synthsizer used in this course.\
How does Yosys work ?\
We have a design and .lib file and it is given to the yosys tool which gives out the netlist.
Commands for using Yosys
- `read_verilog` --> To read the design.
- `read_liberty` --> To read the .lib.
- `write_verilog` --> To write out the netlist file.

*Note*
> *Netlist file  is the representation of design in the form of Standard Cells present in the .lib .*

*Verification of Synthesis*\
\
The GTKwave should be as we found in the case of design simulation as Netlist is just the cell representation of design they both are basically the same.\
\
![day1_15](https://user-images.githubusercontent.com/84860957/119940129-62ee5a00-bfac-11eb-9369-ad7f104fd2da.JPG)\
\
*Note*
>*The set of Primary inputs/primary outputs will remain same between RTL design and Synthesized Netlist --> Same Testbench can be used*.

***Logic Synthesis*** \
*RTL design* \
It is the Behavioral Representation of required specification like the verilog HDL code below\
\
![day1_16](https://user-images.githubusercontent.com/84860957/119940902-7bab3f80-bfad-11eb-83ad-90866f7e3696.JPG)\
\
*Synthesis*
- RTL to Gate Level Translation
- The design is converted into the gates and connections are made between the gates.
- This is given out as the file called Netlist.
- ![day1_17](https://user-images.githubusercontent.com/84860957/119941556-6387f000-bfae-11eb-9024-ff5dd412efb8.JPG)\

*.lib* 
- It is the collection of the logical modules
- Includes basic gates such as And,Or,Not etc.
- Different flavour of same gate 
  * 2 input And Gat
    * slow
    * medium  
    * fast
  * 3 input And Gate
    * slow
    * medium  
    * fast 
  * 4 input And Gate ....


 *Why Do we need different flavours of gates*\
 As the combinational delay in the logic path determinesthe maximum speed of operation of digital logic circuit i.e\
 T<sub>clk</sub> > T<sub>CQ_A</sub> + T<sub>combi</sub>+T<sub>Setup_B</sub> we need cells to work fast \
 And to ensure that there is no hold issue in the next flop we need slow cell and as hold time is given by\
 T<sub>Hold_B</sub> < T<sub>CQ_A + T<sub>combi</sub></sub>\ we need cell that work slowly.\
 We need cells that work fast to meet performance and cells that work slow to meet Hold time. So we need different flavours of gate to meet this requirement.The Collection forms .lib.\
 \
 *Note*
 >*Load in Digital Logic Circuit --> Capacitance*\
 >*Faster the charging/discharging --> Lesser the cell delay*
 > * *To Charge/Discharge the capacitance fast, we need Transistors capable of sourcing more current*.
 > * *Wider Transistor --> Low Delay --> More Area and Power*.

*Selection of Cells*
- Need to guide the synthesizer to select he cells that are optimum for implementation of logic design.
- More use of Faster cells results in bad circuit in terms of area and power and also might voilet the Hold time.
- More use of slower cells results in sluggish circuit.
- The Guidance given to the synthesizer is known as 'contraints'.

*Lab 3*\
Synthsis using Yosys\
First we Invoke the Yosys by simply typing the command `yosys`.\
\
![day1_18](https://user-images.githubusercontent.com/84860957/119946990-d8f6bf00-bfb4-11eb-87b9-2a511ae6cdc2.JPG)\
\
We are in the yosys prompt now, then we read the library using `read_liberty -lib` followed by path of the library it will import the library\
\
![day1_19](https://user-images.githubusercontent.com/84860957/119947527-7651f300-bfb5-11eb-9e68-25308a2abea8.JPG)\
\
Then read the design using the command `read_verilog`.\
\
![day1_20](https://user-images.githubusercontent.com/84860957/119947880-dea0d480-bfb5-11eb-8bdb-9e23a1f5d161.JPG)\
\
after the design is read successfully we use use the command `synth -top` for telling what module to synthsize\
\
![day1_21](https://user-images.githubusercontent.com/84860957/119948465-7e5e6280-bfb6-11eb-9489-3f069303602f.JPG)\
\
![day1_22](https://user-images.githubusercontent.com/84860957/119948468-7ef6f900-bfb6-11eb-9544-d86ed4deb9e2.JPG)\
\
Now to generate the netlist we use the command `abc -liberty` followed by the location of .lib file.\
\
![day1_23](https://user-images.githubusercontent.com/84860957/119949006-0e041100-bfb7-11eb-9b4d-a40df7d0e208.JPG)\
\
We can see what all internal signals ,input and output cells it has inferred\
\
![day1_24](https://user-images.githubusercontent.com/84860957/119949268-56bbca00-bfb7-11eb-8315-b4045f66a099.JPG)\
\
Now to see the Logic it has realised type the command `show`\
\
![day1_25](https://user-images.githubusercontent.com/84860957/119949813-e1042e00-bfb7-11eb-8198-a36287935281.JPG)\
\
We get the following Logic Implementation.\
![day1_26](https://user-images.githubusercontent.com/84860957/119950105-27598d00-bfb8-11eb-9ee4-ac1e96c5f384.JPG)\
\
In the Logic Circuit generated we observe\
- nand2_1 --> 2 input Nand gate
- o21ai_0  --> or and inverter
- clkinv_1 --> inverter

We get the Output from the logic circuit as y = i<sub>1</sub> sel +i<sub>0</sub> sel' which is the logic for 2 input Multiplexer.


### Day 2 Timing libs , Hierarical vs Flat Synthesis and efficient Flop Coding Styles

![day2_1](https://user-images.githubusercontent.com/84860957/119871869-824f9d80-bf40-11eb-9bf1-272686ab0873.JPG)
