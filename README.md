![Verilog-flyer](https://www.vlsisystemdesign.com/wp-content/uploads/2021/05/Verilog-flyer.png)
# RTL_Design_Using_Verilog_with_SKY130_Technology_Workshop
This is a repository on RTL Design using Verilog with Sky130 Technology workshop conducted by VSD.It is a day by day log including examples from Lab sessions and Theory sessions
## Table of Content
- [Introduction](#Introduction)
- [Day 1 Introduction to Verilog RTL Design and Synthesis](#Day-1-Introduction-to-Verilog-RTL-Design-and-Synthesis)
- [Day 2 Timing libs , Hierarical vs Flat Synthesis and efficient Flop Coding Styles](#Day-2-Timing-libs-,-Hierarical-vs-Flat-Synthesis-and-efficient-Flop-Coding-Styles)
- [Day 3 Combinational and sequential optimization](#Day-3-Combinational-and-sequential-optimization)
- [Day 4 GLS,blocking vs non blocking and Synthesis Simulation mismatch](#Day-4-GLS-blocking-vs-non-blocking-and-Synthesis-Simulation-mismatch)
- [Day 5 Optimization in synthesis](


## Introduction
This Workshop intends to teach basics of digital design using verilog language, various RTL coding styles,synthesis and problems faced by industry and how to solve them in Verilog using SKY130 Technology.\
Some of the open source eda tools used in this Workshop are
1. [iverilog](http://iverilog.icarus.com/) : It is Simulator which used for RTL Simulations and Gate Level Simulations.
2. [yosys](http://www.clifford.at/yosys/) : It is a open source Synthesis tool used for synthesis of RTL Design.
3. [Skywater](https://skywater-pdk.readthedocs.io/en/latest/rules/background.html): It is used as 130nm Standard Cell Libraries.

## Day 1 Introduction to Verilog RTL Design and Synthesis 
### Introduction to open source simulator iverilog
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

***LAB Session***\
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
```verilog
module sample_code(
input clk,rst,
output result,done);

always @ (posedge clk,posedge rst)
if(rst)
---
else
---
endmodule
--------------
--------------
```
*Synthesis*
- RTL to Gate Level Translation
- The design is converted into the gates and connections are made between the gates.
- This is given out as the file called Netlist.

*.lib file* 
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
In the Logic Circuit generated we observe
- nand2_1 --> 2 input Nand gate
- o21ai_0  --> or and inverter
- clkinv_1 --> inverter

We get the Output from the logic circuit as y = i<sub>1</sub> sel +i<sub>0</sub> sel' which is the logic for 2 input Multiplexer.


## Day 2 Timing libs , Hierarical vs Flat Synthesis and efficient Flop Coding Styles
### Introduction to .lib
We have used sky130_fd_sc_hd__tt_025C_1v80 for which meaning of each abbreviation is given below
- fd - the skywater foundary
- sc - digital standard cell
- hd - high density
- tt - typical process
- 025C - temperature
- 1v - voltage
*Note*
> library in the sky130 pdk are named using the following scheme:

> process name _ library source _ library type _ library name

Here is sample of .lib file\
\
![day2_25](https://user-images.githubusercontent.com/84860957/120077764-a12d6b80-c0c9-11eb-8289-9913e206d24b.JPG)\
\
![day2_26](https://user-images.githubusercontent.com/84860957/120077777-ab4f6a00-c0c9-11eb-93e7-d448b1fbf73c.JPG)\
\
Some details which are provided in the .lib file are 
- Power 
  * Internal Power 
  * Leakage Power
- Area 
- Pin details
- Timings
- Capacitance

#### Comparison between different Cells in terms of there Area
\
![day2_24](https://user-images.githubusercontent.com/84860957/120078089-5d3b6600-c0cb-11eb-9b3f-f154cff3b464.png)\
\
*Note*
> Here we observe that 
> area(and2_4) >area(and2_2)>area(and2_0) 
> power(and2_4)>power(and2_2)>power(and2_0)

### Hierarchical vs Flat Synthesis
In this section we explore through the Hierarical and Flat synthesis.\
Let us go through one of the examples through a Lab session to understand the difference between hierarchical and Flat Synthesis.\
\
***LAB Session***\
*Lab 1*\
For this we are going to consider the multiple_modules.v code.\
```verilog
module sub_module2 (input a, input b, output y);
	assign y = a | b;
endmodule

module sub_module1 (input a, input b, output y);
	assign y = a&b;
endmodule


module multiple_modules (input a, input b, input c , output y);
	wire net1;
	sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
	sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
endmodule
```
Step 1 : Invoke yosys using `yosys`\
\
![day4_5](https://user-images.githubusercontent.com/84860957/120079016-ae4d5900-c0cf-11eb-87e8-a014b104fcfe.JPG)\
\
Step 2 : Read liberty file using `read_liberty`\
\
![day4_6](https://user-images.githubusercontent.com/84860957/120079082-02f0d400-c0d0-11eb-86d8-5edac4bdf2dc.JPG)\
\
Step 3 : Read verilog file using `read_verilog`\
\
![day2_27](https://user-images.githubusercontent.com/84860957/120079195-772b7780-c0d0-11eb-8103-c874ebcd1392.JPG)\
\
Step 4 : For starting the synthsis process use `synth -top` and also observe the inferences\
\
![day2_28](https://user-images.githubusercontent.com/84860957/120079307-e1441c80-c0d0-11eb-81df-d4b10ce988aa.JPG)\
\
![day2_29](https://user-images.githubusercontent.com/84860957/120079415-47c93a80-c0d1-11eb-9607-fd963204ef65.JPG)\
\
Step 5 : Now to map to standard cell we use `abc -liberty`\
\
![day2_30](https://user-images.githubusercontent.com/84860957/120082243-764e1200-c0df-11eb-82ab-12213391b5f2.JPG)\
\
Step 6 : Now to see the Logic design we give the command `show`\
\
![day2_31](https://user-images.githubusercontent.com/84860957/120082301-c4631580-c0df-11eb-93fc-d7264a869f87.JPG)\
\
![day2_32](https://user-images.githubusercontent.com/84860957/120082318-e0ff4d80-c0df-11eb-8e55-d167fa8fb90d.JPG)\
\
We observe that it does not show 'and' gate or 'or' gate but instead shows the 2 sub modules u1 and u2.This is what we call the **Hierarical design**.\
\
Step 7 : Now to get the netlist we give the `write_verilog -attr` command\
\
![day2_33](https://user-images.githubusercontent.com/84860957/120082456-d98c7400-c0e0-11eb-8457-bede10f72779.JPG)\
\
![day2_34](https://user-images.githubusercontent.com/84860957/120082773-67b52a00-c0e2-11eb-8668-bee95d7ea581.JPG)
\
\
We see that all the hierarchies are preserved and to avoid stacked PMOS the logic is implemented using NAND gates.\
\
*Note*
> *Stacked PMOS is always bad because PMOS has poor mobility and to improve this we have to make the cell alot wide cell to get good logical effort.

Step 8 : We use `flatten` to write out he flat netlist.And again use `write_verilog -attr` to get the flattened netlist.\
\
![day2_35](https://user-images.githubusercontent.com/84860957/120082814-aba82f00-c0e2-11eb-9830-4515c10033bc.JPG)\
\
In this case the hierarchy is not preserved but are flattened out. We can see the logic diagram flattened as below
\
![day2_36](https://user-images.githubusercontent.com/84860957/120082884-16f20100-c0e3-11eb-8e9f-f2e96955816a.JPG)\
\
We do not see u1 and u2 but the sub-modules completely flattened.\
\
Now to do a sub module level synthesis we follow the same procedure until Step 3\
\
Step 4 : Instead of `synth -top` to get sub module level syntheies we give the command `synth -top sub_module1`\
\
![day2_37](https://user-images.githubusercontent.com/84860957/120083122-3f2e2f80-c0e4-11eb-8df0-de240a0a184d.JPG)\
\
again follow the Step 5 and Step 6.We get the logic diagram of sub_module1 \
\
![day2_38](https://user-images.githubusercontent.com/84860957/120083215-b6fc5a00-c0e4-11eb-9c48-5a194548106a.JPG)\
\
![day2_39](https://user-images.githubusercontent.com/84860957/120083230-d1cece80-c0e4-11eb-95f1-78485de5fcbb.JPG)\
\
##### Why we use Sub Module level synthesis 
- When we want to use multiple instances of the same module.
- We want to do divide and conquer approch if we have a massive design.

*Note*
> *`Synth -top` controls which module to synthesis*.\
> *`flatten` is used for flattening the netlist*.

### Various Flop Coding Styles and Optimization.








 



















## Day 3 Combinational and sequential optimization
### Introduction to Logic Optimisation
Logic Optimisation contains mainly 2 parts 
1. Combination Logic Optimisation
2. Sequential Logic Optimisation
#### Combinational Logic optimisation
-We need combinational logic optimisation to squeeze the logic to get the most optimised design 
   * Mainly for Area and Power Saving
- Constant Propogation is one of the technique used for Combinational Logic Optimisation 
    * It is a Direct Optimisation in which the value of one input is propogated to the next stage  to get the most optimised logic.
- Another Technique Used is the Boolean Logic Optimisation in which we use and other tools to get the most optimised logic.

#### Sequential Logic optimisation
In Sequential Logic Optimisation there are 2 Techniques 
1. Basic
 * Sequential Constant Propogation
   * Some of the Sequential design in which D input is tied off the Squential Constant is propogated to give Q pin as a Constant and gives the most optmised design of the Squential Circuits.
   * The Sequential Design in which Q does not remains as constant cannot be optimised and flop needs to be retained in the circuit.
   * *Note*
    >*Every flop in which the D input is tied off is a sequential constant for the flop to become sequential constant the Q pin should always take a constant value*.
2. Advanced (not a part of the workshop) 
 * State Optimisation
   * Optimisation of unused states.
 * Retiming
   * It is a Technique to improve the Performance of the Circuit.
 * Sequential Logic Cloning (Floor Plan Aware Synthesis)
   * It is done when we are doing a Physical Aware Synthesis.

***LAB Session***\

*Lab 1*\
**Combinational Logic Optimisation**\
In this Lab we are going to use all the 'opt_check' files which can be found by typing the command`ls *opt_check*`\
Let us first see the code for opt_check.v and opt_check2.v by opening it using `gedit`\
\
![day3_1](https://user-images.githubusercontent.com/84860957/119980860-816b4a00-bfda-11eb-9135-a9d51c409319.JPG)\
\
The opt_check is simplified as y = ab (And Logic gate)\
The opt_check2 is simplified as y = a+b (Or logic gate)\
\
Now we invoke Yosys using the command `yosys`\
Load the Libraries using `read_liberty -lib`\
Now  read the verilog file using `read_verilog`\
\
![day3_2](https://user-images.githubusercontent.com/84860957/119981687-8aa8e680-bfdb-11eb-9c62-59c34f971e51.JPG)\
\
Now synthesizing the device using `synth -top`\
\
![day3_3](https://user-images.githubusercontent.com/84860957/119982058-fdb25d00-bfdb-11eb-9882-fd6c04144920.JPG)\
\
![day3_4](https://user-images.githubusercontent.com/84860957/119982061-fee38a00-bfdb-11eb-9772-66b434029a71.JPG)\
\
The  Command to do the constant propogation and all the optimisation is `opt_clean -purge`\
\
![day3_5](https://user-images.githubusercontent.com/84860957/119982501-8df0a200-bfdc-11eb-96ad-5c62cab322fa.JPG)\
\
Now Link it to the liberty using `abc -liberty` and then `show`\
\
![day3_6](https://user-images.githubusercontent.com/84860957/119982897-10796180-bfdd-11eb-8371-0986b9b1acf4.JPG)\
\
As we already concluded earlier for opt_check.v we are getting an And gate we see the same being implemented.\
Similarly we check the implementation for opt_check2.v.\
\
![day3_7](https://user-images.githubusercontent.com/84860957/119983378-b2994980-bfdd-11eb-834d-7df54a2d48bf.JPG)\
\
As we already concluded earlier for opt_check2.v it should be a 2 input Or gate we get the same being implemented.\
As Or gate involves Nor gate and it uses a stacked PMOS which is bad due its mobility and various other reasons the so the Tool does a Nand Realisation of Or gate.It a inverter -inverter followed by a Nand.\
\
![day3_8](https://user-images.githubusercontent.com/84860957/119984127-a792e900-bfde-11eb-84ab-f8cbeca67c4e.JPG)\
\
Similarly we observe opt_check3.v. \
\
![day3_9](https://user-images.githubusercontent.com/84860957/119985046-c6de4600-bfdf-11eb-9733-034818543a54.JPG)\
\
We evaluate that y = abc (3 input And gate).We apply the same process as above for opt_check.v and opt_check2.v to get the optimised output.\
\
![day3_10](https://user-images.githubusercontent.com/84860957/119986050-06f1f880-bfe1-11eb-8920-2e227a5c8695.JPG)\
\
As we already concluded earlier for opt_check3.v it should be a 3 input And gate we get the same being implemented.\
\
Similary We observe multiple_modules_opt.v.\
\
![day3_11](https://user-images.githubusercontent.com/84860957/119987069-3ce3ac80-bfe2-11eb-87bf-e9e223096f9d.JPG)\
\
We get the following result after we flatten the multiple_module_opt\
\
![day3_12](https://user-images.githubusercontent.com/84860957/119988192-78cb4180-bfe3-11eb-9b06-6707cdf99650.JPG)\
\
The resulting Circuit is Enlarged below\
![day3_13](https://user-images.githubusercontent.com/84860957/119988187-779a1480-bfe3-11eb-9573-9a56633bda5c.JPG)\
\
\
**Sequential Logic Optimisation**\
*Lab 2*\
In this Lab we are going to use all the 'dff_const' files which can be found by typing the command`ls *dff_const*`\
Let us take the dff_const1.v and dff-const2.v and open it \
\
Here is the Verilog code for both\
![day3_14](https://user-images.githubusercontent.com/84860957/119991856-8a164d00-bfe7-11eb-821e-562d110c704a.JPG)\
\
![image](https://user-images.githubusercontent.com/84860957/119992133-d95c7d80-bfe7-11eb-8bf5-7d32b4e6b79c.png)\
\
Let us simulate  before synthesizing for that we use iverilog and then excute the a.out file to get vcd file.\
\
![image](https://user-images.githubusercontent.com/84860957/119993467-31e04a80-bfe9-11eb-8652-49a86e48e592.png)\
\ 
Use `gtkwave` to get the waveform\
\
![image](https://user-images.githubusercontent.com/84860957/119994081-e1b5b800-bfe9-11eb-889b-2d5e21358af0.png)\
\
We observe that even after rst has become low Q waits for rising edge of the clock to go high. This circuit will infer a flop.\
\
![image](https://user-images.githubusercontent.com/84860957/119994537-4ec94d80-bfea-11eb-86f3-f98abdd893ec.png)\
\
Now we Compare it with dff_const2.v\
\
We see that Q does not depend upon clk unlike in the case of dff_const1.v\
![image](https://user-images.githubusercontent.com/84860957/119995765-8f759680-bfeb-11eb-9d32-db43bbeec350.png)\
\
Now let us Synthesis both the files\
As this is a sequential circuit while synthesis we have to give one more command `dfflipmap` for proper mapping of sequential cells in the library.\
\
For dff_const1.v\
![image](https://user-images.githubusercontent.com/84860957/119997392-4de5eb00-bfed-11eb-80a5-4dca530144c8.png)\
\
![image](https://user-images.githubusercontent.com/84860957/119997652-930a1d00-bfed-11eb-808d-a17e9f61a2e4.png)\
\
![image](https://user-images.githubusercontent.com/84860957/119997841-c64cac00-bfed-11eb-89b6-2580597a786e.png)\
\
We get the following logic design\
![image](https://user-images.githubusercontent.com/84860957/119997995-ebd9b580-bfed-11eb-902f-df06a98134ad.png)\
\
The standard cell library is expecting the reset to be active low but we have coded a active high reset so the tool is inferring a inverter.\
\
Now we synthesis  dff_const2.v file\
\
We observe that it has not inferred any flop in this case contradictory to dff_const2.v
![day3_22](https://user-images.githubusercontent.com/84860957/120008671-d0c07300-bff8-11eb-892e-b4f28c7b90a7.JPG)\
\
![image](https://user-images.githubusercontent.com/84860957/120007529-a3bf9080-bff7-11eb-89e1-6aff77fa364c.png)\
\
Now let us take dff_const.v file\
\
![day3_23](https://user-images.githubusercontent.com/84860957/120013530-7aeec980-bffe-11eb-836b-37b080a20c91.JPG)\
\
By observing the code we see that Q1 and Q both cannot be optimized so both the flops will be present in the Circuit.\
So first let us simulate our design \
\
![day3_24](https://user-images.githubusercontent.com/84860957/120014737-0d439d00-c000-11eb-8806-08fa7f0448c8.JPG)\
\
We can clearly see that both q and q1 do not remain constant and both change on the rising edge of the clock so the cannot be optimized
![day3_25](https://user-images.githubusercontent.com/84860957/120015132-95c23d80-c000-11eb-8ea6-164df52c10c4.JPG)\
\
Now let us go ahead and do the synthesis of the circuit\
\
![day3_26](https://user-images.githubusercontent.com/84860957/120015830-711a9580-c001-11eb-8cc2-53e8368d19f7.JPG)\
\
We can clearly see it infers 2 flops\
![day3_27](https://user-images.githubusercontent.com/84860957/120015915-90192780-c001-11eb-964e-eb486b88d96b.JPG)\
\
![day3_28](https://user-images.githubusercontent.com/84860957/120016413-35340000-c002-11eb-99a4-1714313a0572.JPG)\
\
From the Logic diagram we have confirmed that 2 flops and also the 1st flop is a reset flop and the 2nd flop is a set flop.\
![day3_29](https://user-images.githubusercontent.com/84860957/120016523-5694ec00-c002-11eb-905a-1cde4e794048.JPG)\
\
Not every flop which is having constant at the input gets optimized we also need to look at set and the reset connections and see if q is constant or q values is going to change.\

*Lab3*
Sequential Optimisation using Unused Output optimisation.\
So let us consider the code counter_opt.v\
\
![day3_30](https://user-images.githubusercontent.com/84860957/120017702-d079a500-c003-11eb-962a-afc7ce5c2a97.JPG)\
\
When we look at code we see that it is a up-counter whenever there is reset it reset otherwise its 3 bit up-counter.\
The output of the circuit is count[2:0] but q = count[0]. count[1] and count[2] are unused. So this are not required in the logic diagram.\
Let us synthesis this code.\
\
We clearly see only 1 flop is inferred even if it is 3 bit counter it should have inferred 3 flops \
![day3_31](https://user-images.githubusercontent.com/84860957/120018717-2c90f900-c005-11eb-91c8-baeee37db3bc.JPG)\
\
As it is a sequential circuit we should use `dfflibmap -liberty ../my_lib/lib/<location of .lib file>`\
![day3_32](https://user-images.githubusercontent.com/84860957/120019489-23545c00-c006-11eb-9e49-51344bcdfb11.JPG)\
\
We can clearly see that there is only 1 flop in the circuit design.We see count[0] is toggling so the output q is given to the inverter and feededback to the d pin.\
![day3_33](https://user-images.githubusercontent.com/84860957/120019609-4aab2900-c006-11eb-8200-8fc05076ce49.JPG)\
\
So we can conclude that any logic which is not resulting in any primary output is optimised away.\
Now let us consider count_opt2.v\
![day3_34](https://user-images.githubusercontent.com/84860957/120020725-bb067a00-c007-11eb-9451-f0f49585ebe4.JPG)\
\
We have tweaked the code slightly from count_opt.v as we have changed `assign q = (count[2:0] == 3'b100);`\
Now we expect that as all outputs are used it should have 3 flops to be present in the logic circuit.\
Let us synthesis the code.\
\
![day3_35](https://user-images.githubusercontent.com/84860957/120021888-46343f80-c009-11eb-8caf-6c988831633c.JPG)\
\
From the Figure above we clearly see that 3 flops are inferred here.\
\
![day3_36](https://user-images.githubusercontent.com/84860957/120022280-d07ca380-c009-11eb-91af-12cfc02884fe.JPG)\
\
The logic other than the flops is the incremental logic.\
The given Logic diagram contains 3 flops .We get the expression of q = count[2] . count[1]'.count[0]' so it is using all the primary outputs so it uses all the flops.\
So we conclude that all the outputs that have no role in determining the primary output are optimized away using Unused Output Optimisation.

## Day 4 GLS,blocking vs non blocking and Synthesis Simulation mismatch
### Introduction to GLS
#### What is GLS 
- It stands for Gate Level Simulation.
- Running the Test Bench With Netlist as design under Test.
- Netlist is logically same as the RTL Code.
  * Same testbench will align with the design.
#### Why GLS ?
- Verify the LOgical Correctness of the design after synthesis.
- Ensuring the Timing of design is met
  * For this GLS needs to be run with delay annotation. \
#### GLS using iverilog
- We give the Netlist,**Gate Level Verilog Models** and Testbench  to the iverilog.
- Netlist has all the standard cell instantiated and the meaning of standard cell is conveyed to the iverilog by **Gate Level Verilog Models**.
- After which it has the same flow giving the vcd file using which we can generate Waveform using GTKwave.

*Note* 
> *If the gate level models are delay annotated, then we can use GLS for timing validation.*

#### Synthesis Simulation Mismatch
-Suppose we have a Netlist 
  * and u_and(.a(a),.b(b)) 
  * or u_or (.a(a),.b(b)) 
- Now to understand this we Gate level Verilog Models.They can 
  * Timing Aware -->validate functionality + timing both 
  * Functional --> validate functionality
##### Why Synthesis Simulation Mismatch happens ?
- Missing Sensitivity List
- Blocking Vs Non-Blocking Assignments
- Non Standard verilog Coding

##### Missing Sensitivity List
Let us consider a verilog Code
```verilog 
module mux(
input i0,input i1
input sel,
output reg y
);
always @ (sel)
begin
   if (sel)
            y = i1;
   else 
            y = i0;
            
end
endmodule
```
- Simulator works based on activity i.e the output will change only when there is a change in the input.
- In the Code given the always block is evaluating only when the sel is changing. As the always block is not sensitive to the changes in i0 and i1.
- Only when there is a change in sel the always block gets evaluated. 
- When  we simulate it acts as a latch.But when we synthesis it will act as a Mux.
- This is called missing elements of sensitivity list.
- Now let us consider another code 
```verilog 
module mux(
input i0,input i1
input sel,
output reg y
);
always @ (*)
begin
   if (sel)
            y = i1;
   else 
            y = i0;
            
end
endmodule
```
- In this case always is evaluated when any signal changes.
- So now always block will be evaluated for i1 as well as i0
- This will simulate as Mux and also be synthesised as Mux.

##### Blocking and Non Blocking Statements
- Inside always block
  * If we are using '=' to make assignments
    * Executes the Statement in the order it is written.
    * So the first statement is evaluated before the second statement.
  * If we are using '<=' to make assignments
    * Executes all the RHS when always block is enteredand assigns to LHS
    * Parallel Evaluation

##### Caveats with Blocking Statements 
- Let try to create a shift register

```verilog
module code (input clk,input reset,
input d,
output reg q);
always @ (posedge clk,posedge reset)
begin
if(reset)
begin
        q0 = 1'b0;
        q = 1'b0;
end
else
        q = q0;
        q0 = d;
        
end
endmodule
```
- In this case q0 is assigned to q and then d gets assigned to q0.So we get 2 values.
- So it has 2 flops. 
- Now let change the code slightly


```verilog
module code (input clk,input reset,
input d,
output reg q);
always @ (posedge clk,posedge reset)
begin
if(reset)
begin
        q0 = 1'b0;
        q = 1'b0;
end
else
        q0 = d;
        q = q0;
                
end
endmodule
```
- In this case q0 is assigned d the q0 gets assigned to q
- So it is as if q is getting the value of d which means it only has 1 flop
- But if we write it with non blocking statements
```
module code (input clk,input reset,
input d,
output reg q);
always @ (posedge clk,posedge reset)
begin
if(reset)
begin
        q0 <= 1'b0;
        q <= 1'b0;
end
else
        q0 <= d;
        q <= q0;
                
end
endmodule
```
- So we conclude that non blocking statements should be used to write Sequential Circuit Code.

Now Let us look at another code.\
Our aim is to create a circuit with output as y = (a + b).c
```verilog
module code (input a,b,c
output reg y);
reg q0;
always @ (*)
begin
        y = q0 & c;
        q0 = a|b ;
        
end 
endmodule
```
- We assign the value of y with q0 and with c first and then assign q0 as a or b 
- So we see that q0 value is the old q0 value and is not updated.
- When we simulate old q0 value will mimic a delay or flop but when we synthesis there will not be a flop.
- Now let us change the order of block statement
```verilog
module code (input a,b,c
output reg y);
reg q0;
always @ (*)
begin
        q0 = a|b ;
        y = q0 & c;
        
        
end 
endmodule
```
- In this case there will not be any delay as the q0 is updated first then used.
- When we simulate it will work as the given circuit.
- In both cases above when we synthesis we get the desired circuit but in first case the simulation result differ with synthesis results.

Because of these kind of issues it becomes very important to run GLS on the Netlist and match with the expected output and see if there are no Synthesis Simulation Mismatches.\
\
***LAB Session***\
*Lab 1*\
In this Session we are going to see how to invoke GLS and run some basic experiments with GLS.
To Run GLS we need
- Netlist
- Verilog Models
- Testbench

Let us consider the file ternary_operator_mux.v
- Ternary Operator
  * ` <cond> ? <T>:<F>`
  * If the condition is true the 'T' part is executed and if it is false 'F' part is executed.


Now let us have a look at the code of  ternary_operator_mux.v\
![day4_1](https://user-images.githubusercontent.com/84860957/120068523-b640d500-c09e-11eb-8ece-967e608c51ae.JPG)\
The code given is basically a Mux implemented using a ternary operator.\
Now let us first do the RTL simulation.\
For that we first invoke `iverilog` and then execute the a.out file using `./a.out`\
![day4_2](https://user-images.githubusercontent.com/84860957/120068627-39fac180-c09f-11eb-8212-4350f6fbde0a.JPG)\
After getting the vcd file use `gtkwave <file name>` to see the simulation.\
![day4_3](https://user-images.githubusercontent.com/84860957/120068653-6c0c2380-c09f-11eb-81d2-3b08a8f11e42.JPG)

![day4_4](https://user-images.githubusercontent.com/84860957/120068728-cdcc8d80-c09f-11eb-974d-8cc9c2df06a0.JPG)\
\
Here we observe it is clearly following the behaviour of 2:1 MUX.\
Now let us systhesis for which we invoke `yosys`\
![day4_5](https://user-images.githubusercontent.com/84860957/120068803-387dc900-c0a0-11eb-80d7-de215d0b64cc.JPG)\
\
To Load the .lib file we use the command `read_liberty -lib ../my_lib/lib/...`\
![day4_6](https://user-images.githubusercontent.com/84860957/120068847-70850c00-c0a0-11eb-8782-3e39a5be8f15.JPG)\
Then we read the verilog file using `read_verilog <file name>`\
![day4_7](https://user-images.githubusercontent.com/84860957/120068898-a4f8c800-c0a0-11eb-8eed-100ab20a6d36.JPG)\
Now let us start the synthesis process using `synth -top <file name>`\
![day4_8](https://user-images.githubusercontent.com/84860957/120068924-c78ae100-c0a0-11eb-8821-e602a0b716aa.JPG)\
\
![day4_9](https://user-images.githubusercontent.com/84860957/120068944-e2f5ec00-c0a0-11eb-97d5-7e72d1be7317.JPG)\
We can clearly see that 1 Mux has been infered.\
Now let us use the `abc -liberty ../my_lib/lib/<.lib file>`\
\
![day4_12](https://user-images.githubusercontent.com/84860957/120069067-93fc8680-c0a1-11eb-84b5-cf08d4465ca8.JPG)\
\
and then use `show` to get the logic diagram generated.\
\
![day4_10](https://user-images.githubusercontent.com/84860957/120069034-64e61500-c0a1-11eb-86c3-744e7607d836.JPG)\
\
This is the Logic Diagram generated\
\
![day4_11](https://user-images.githubusercontent.com/84860957/120069039-70d1d700-c0a1-11eb-9f12-d0b57d404990.JPG)\
\
If we Solve the logic diagram we get the result as y = sel .i1 + sel'.i0.Exactly \
\
Now let us do the GLS.\
For that we need to open iverilog with verilog models,netlist,  and testbench using the command `iverilog`. In the lib folder you will find the verilog models of the standard cells.Execute a.out file using ./a.out \
\
![day4_13](https://user-images.githubusercontent.com/84860957/120069303-d83c5680-c0a2-11eb-9798-d2fe66abbef8.JPG)\
\
Now as the vcd file is generated.Now we use GTKwave to observe the simulation by giving the command `gtkwave`\
\
![day4_14](https://user-images.githubusercontent.com/84860957/120069400-7e885c00-c0a3-11eb-90f8-a878492882e5.JPG)\
\
We know the difference GLS and RTL as in GLS under uut there no files but in GLS we _6_ , _7_ , _8_.\
\
![day4_15](https://user-images.githubusercontent.com/84860957/120069486-c909d880-c0a3-11eb-8c5d-29f22da6c195.JPG)\
\
Now let us observe the simulation. It is clearly following the MUX output.\
\
![day4_16](https://user-images.githubusercontent.com/84860957/120069528-feaec180-c0a3-11eb-8cac-ac7cfc4869e5.JPG)\
\
\
Now Let us take another example of bad_mux.v\
\
![day4_17](https://user-images.githubusercontent.com/84860957/120069650-ab893e80-c0a4-11eb-9f6c-4acc0f76753f.JPG)\
\
We can observe that in this case the always block going to be evaluated only upon changes in sel.\
So in simulation it we work as some kind of flop.\
Now let us carry out the RTL simulation.\
\
For simulation we first invoke the iverilog using `iverilog` command.\
\
![day4_18](https://user-images.githubusercontent.com/84860957/120069804-703b3f80-c0a5-11eb-84e4-9ab930e19506.JPG)\
\
Then we execute the a.out file using the `./a.out` command and get the vcd file.\
\
![day4_19](https://user-images.githubusercontent.com/84860957/120069865-bbede900-c0a5-11eb-9ebf-5b54e677a229.JPG)\
\
Now we use GTKwave to observe the simulation by giving the command `gtkwave`\
\
![day4_20](https://user-images.githubusercontent.com/84860957/120069952-2bfc6f00-c0a6-11eb-9026-d1a49a0c1a4c.JPG)\
\
![day4_21](https://user-images.githubusercontent.com/84860957/120069991-5ea66780-c0a6-11eb-8374-d70324ff0092.JPG)\
\
So we can observe that if there is no activity on the output does not change.\
This simulation shows as if the Mux is acting like a flop .\
\
Now let us synthesis and see what happens.
\
For synthesis we invoke `yosys`\
\
![day4_5](https://user-images.githubusercontent.com/84860957/120070340-a37ece00-c0a7-11eb-8574-e0fa81a9f8e2.JPG)\
\
To Load the .lib file we use the command `read_liberty -lib ../my_lib/lib/...`\
![day4_6](https://user-images.githubusercontent.com/84860957/120068847-70850c00-c0a0-11eb-8782-3e39a5be8f15.JPG)\
\
Then we read the verilog file using `read_verilog <file name>`\
\
![day4_22](https://user-images.githubusercontent.com/84860957/120070407-01131a80-c0a8-11eb-8031-5d4d3ded68ac.JPG)\
\
Now let us start the synthesis process using `synth -top <file name>`\
\
![day4_23](https://user-images.githubusercontent.com/84860957/120070465-4a636a00-c0a8-11eb-9f7a-f26f896ec6b8.JPG)\
\
Here are some details of which gates are inferred\
\
![day4_24](https://user-images.githubusercontent.com/84860957/120070502-7da5f900-c0a8-11eb-9d9a-387a17f18933.JPG)\
\
So we see it infers MUX not a latch.\
\
Now let us use the `abc -liberty ../my_lib/lib/<.lib file>`\
\
![day4_25](https://user-images.githubusercontent.com/84860957/120070588-eab98e80-c0a8-11eb-89a6-efca6f81ae7c.JPG)\
\
Now to get the netlist file we use `write_verilog`\
\
![day4_26](https://user-images.githubusercontent.com/84860957/120070746-a7135480-c0a9-11eb-89cb-1d0abfd9cafe.JPG)
\
Now let us do the GLS.\
For that we need to open iverilog with verilog models,netlist,  and testbench using the command `iverilog`. In the lib folder you will find the verilog models of the standard cells.Execute a.out file using `./a.out` \
\
![day4_27](https://user-images.githubusercontent.com/84860957/120070876-3e78a780-c0aa-11eb-807f-dacf1babe9e9.JPG)\
\
Now we use GTKwave to observe the simulation by giving the command `gtkwave`\
\
![day4_28](https://user-images.githubusercontent.com/84860957/120071008-d2e30a00-c0aa-11eb-8cb4-d0057556c67a.JPG)\
\
![day4_29](https://user-images.githubusercontent.com/84860957/120071044-fefe8b00-c0aa-11eb-8d55-56c7ff900301.JPG)\
\
Now we compare the simultion with the RTL design and after synthesis Netlist simulation.\
\
![day4_30](https://user-images.githubusercontent.com/84860957/120071293-3de11080-c0ac-11eb-8c47-544e6370ab1b.jpg)\
\
In the GLS simulation the activity on i1, i0 has been reflected on output whereas in the RTL simulation the output was effected by only the sel.\
This is called Synthesis Simulation Mismatch.\
\
\
*Lab2*\
In this Lab we will continue to see the Synthesis Simulation Mismatch.\
Now Let us take a blocking code example.\
\
![image](https://user-images.githubusercontent.com/84860957/120071466-353d0a00-c0ad-11eb-9610-d2129a965624.png)\
\
The aim of this code is to get the d = (a+b).c \
\
Now because of the blocking statement first d will be assigned (x and c) then x = a or b will be executed.\
So by the time the statement for d is getting evaluated we have the previous value of x .\
So when we simulate it , it will look as if x is a flopped output.\
\
Now let us do the RTL simulation.\
\
For simulation we first invoke the iverilog using `iverilog` command.\
\
Then we execute the a.out file using the `./a.out` command and get the vcd file.\
\
![day4_33](https://user-images.githubusercontent.com/84860957/120072156-f3619300-c0af-11eb-9e3d-d66690e44af8.JPG)\
\
Now we use GTKwave to observe the simulation by giving the command `gtkwave`\
\
![day4_34](https://user-images.githubusercontent.com/84860957/120072245-77b41600-c0b0-11eb-8bfc-7fbf955cbe2b.JPG)\
\
![day4_35](https://user-images.githubusercontent.com/84860957/120072318-ebeeb980-c0b0-11eb-87b0-b6705fd5c8e0.JPG)\
\
We see from the simulation that the value of d = 0 but it is coming out to be 1 because the past value of (a or b) is 1 in the cycle before that value is getting anded with the present value of c.\
\
Now let us synthesis and see what happens.
\
For synthesis we invoke `yosys`\
\
![day4_5](https://user-images.githubusercontent.com/84860957/120070340-a37ece00-c0a7-11eb-8574-e0fa81a9f8e2.JPG)\
\
To Load the .lib file we use the command `read_liberty -lib ../my_lib/lib/...`\
![day4_6](https://user-images.githubusercontent.com/84860957/120068847-70850c00-c0a0-11eb-8782-3e39a5be8f15.JPG)\
\
Then we read the verilog file using `read_verilog <file name>`\
\
![day4_36](https://user-images.githubusercontent.com/84860957/120072503-df1e9580-c0b1-11eb-8be7-9bff69bf2070.JPG)\
\
Now let us start the synthesis process using `synth -top <file name>`\
\
![day4_37](https://user-images.githubusercontent.com/84860957/120072570-20af4080-c0b2-11eb-976a-742383e89d8d.JPG)\
\
Here are some details of which gates are inferred\
\
![day4_38](https://user-images.githubusercontent.com/84860957/120072657-90bdc680-c0b2-11eb-9bf8-660e200cd324.JPG)\
\
Now let us use the `abc -liberty ../my_lib/lib/<.lib file>`\
\
![day4_25](https://user-images.githubusercontent.com/84860957/120070588-eab98e80-c0a8-11eb-89a6-efca6f81ae7c.JPG)\
\
Now to get the netlist file we use `write_verilog`\
\
![day4_39](https://user-images.githubusercontent.com/84860957/120072843-54d73100-c0b3-11eb-9ee4-7310def47998.JPG)\
\
Before going to the GLS let us look at what logic we got by giving the command `show`.\
\
![day4_40](https://user-images.githubusercontent.com/84860957/120072905-a54e8e80-c0b3-11eb-9699-60eb02b22f5a.JPG)\
\
We clearly see that it is very straight forward with or 2 and gate used and no latches in the design.\
\
Now let us do the GLS.\
For that we need to open iverilog with verilog models,netlist,  and testbench using the command `iverilog`. In the lib folder you will find the verilog models of the standard cells.Execute a.out file using `./a.out` \
\
![day4_41](https://user-images.githubusercontent.com/84860957/120073206-12165880-c0b5-11eb-89a8-db54c4fcc4d2.JPG)\
\
Now we use GTKwave to observe the simulation by giving the command `gtkwave`\
\
![day4_42](https://user-images.githubusercontent.com/84860957/120073346-83eea200-c0b5-11eb-832f-b39ed0968f2b.JPG)\
\
![day4_43](https://user-images.githubusercontent.com/84860957/120073728-3ecb6f80-c0b7-11eb-9cdd-6285657c7e7e.JPG)\
\
Now we compare the simultion with the RTL design and after synthesis Netlist simulation.\
\
![day4_44](https://user-images.githubusercontent.com/84860957/120073766-5acf1100-c0b7-11eb-886b-6b8f6fcaa08e.JPG)\
\
We can clearly see that the value of d = 0 for the same case where a = 0,b = 0 and c = 1.\
This is Synthesis Simulation Mismatch caused by Blocking statement.\
*Note*
> *Please be very carefull before using blocking statements for Sequential Circuits*.


##


















































































































