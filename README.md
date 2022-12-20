# FPGA-workshop
## Day1

## Introduction to FPGA

FPGA (Field Programmable Gate Array) are intergated circuits which have a complex arrangement of configurable logic blocks (CLBs) and programmable interconnects.

## FPGA vs ASIC Comparison

|FPGA|ASIC|
|:---:|:---:|
|Field Programmable Gate Array|Application Specific Integrated Circuit|
|:---:|:---:|
|RTL to Bitstream|Permanent Circuit|
|:---:|:---:|
|Less Energy Efficient. Required more power for same task as compared to ASIC|More energy efficient|
|:---:|:---:|
|Useful for prototyping or validating a design|Used for final product design after validation|
# FPGA Architecture
The FPGA Architecture primarily consists of : - Configurable Logic Blocks - Programmable Interconnects - I/O Cells - Memory / Block RAM
![fpga_arch_1](https://user-images.githubusercontent.com/120499567/208094795-c7714e05-9cfe-47f4-8a1b-7c5b5a097643.png)
## Configurable Logic Block
Configurable Logic Block (CLB) is responsible for the combinational or sequential logic implementation. CLB consists of :
- Look-up Table (LUT) - Logic function implementation
- Carry and Control Logic - Arithmetic Operations
- Flip-flops and/or latches
![image](https://user-images.githubusercontent.com/120499567/208095849-ba631d7a-d2fc-430c-8462-a5fede5b51ba.png)
# Basys FPGA Board
The FPGA used in this repository is Basys 3 Artix-7 FPGA. Below is the snippet of Basys3 FPGA board and some major elements on the board.
![image](https://user-images.githubusercontent.com/120499567/208096167-dbca734b-b301-4553-95b2-3bc191be0fdd.png)
## 4 bit counter run in xilinx vivado 
A 4-bit up counter is being used for exploring the Vivado tool and OpenFPGA. Below mentioned the RTL for the counter modules that is being used
### 'code'
``` js
`timescale 1ns / 1ps
 //////////////////////////////////////////////////////////////////////////////////
 // Description: 4 bit counter with source clock (100MHz) division.

 module counter_clk_div(clk,rst,counter_out);
     
     input clk,rst;
     reg div_clk;
     reg [25:0] delay_count;
     output reg [3:0] counter_out;

     //////////clock division block////////////////////
     always @(posedge clk) begin

         if(rst) begin
             delay_count<=26'd0;
             div_clk <= 1'b0; //initialise div_clk
         end
         else
             if(delay_count==26'd212) begin
                 delay_count<=26'd0; //reset upon reaching the max value
                 div_clk <= ~div_clk;  //generating a slow clock
             end
             else begin
                 delay_count<=delay_count+1;
             end
         end
     end

     /////////////4 bit counter block///////////////////
     always @(posedge div_clk) begin

         if(rst) begin
             counter_out<=4'b0000;
         end
         else begin
             counter_out<= counter_out+1;
         end
     end

 endmodule
 
 \\\\\test bench\\\\\\\*******
 
 `timescale 1ns / 1ps

module test_counter();
reg clk, reset;
wire [3:0] out;

//create an instance of the design
counter_clk_div dut(clk, reset, out);  

initial begin

//note that these statements are sequential.. execute one after the other 

//$dumpfile ("count.vcd"); 
//$dumpvars(0,upcounter_testbench);

clk=0;  //at time=0

reset=1;//at time=0

#20; //delay 20 units
reset=0; //after 20 units of time, reset becomes 0


end


always 
#5 clk=~clk;  // toggle or negate the clk input every 5 units of time


endmodule 

 
 
 ```
 
 ## Counter Simulation and Elaboration
 The snippet below shows the behavioural simulation for the up counter.
 
 ![Screenshot 2022-12-16 184808](https://user-images.githubusercontent.com/120499567/208107350-b6d90214-85c0-4765-a8d3-1ff3aa6c650d.png)
 
The snippet below is the schematic of the counter design after elaboration.

![12](https://user-images.githubusercontent.com/120499567/208107553-9804c3ba-c935-4dbb-9177-eabd19d34ab7.png)

In I/O planning, the ports for modules are assigned respective FPGA pins. The snippet below shows the details about I/O planning

![13](https://user-images.githubusercontent.com/120499567/208107813-b60a87f7-74fb-427a-ab1c-547c6ae074c7.png)
![21](https://user-images.githubusercontent.com/120499567/208597066-73433b62-e3dc-4413-b55f-6f45370192be.png)


### after synthesis

![15](https://user-images.githubusercontent.com/120499567/208596631-6e21d78f-1a60-4858-9c02-0773e8006673.png)


## Counter Synthesis
synthesis is the process that converts RTL into a technologyspecific gate-level netlist, optimized for a set of pre-defined constraints.

The below snippet show sthe schematic generated by the Vivado synthesis tool.

![18](https://user-images.githubusercontent.com/120499567/208597232-df59aa88-509e-41fd-a879-94a81c7d13b6.png)

## Constraints

Constraints in simple are the specifications of your design like timing specifications, ports declaration, input/output delays, etc

![14](https://user-images.githubusercontent.com/120499567/208597431-19a0b044-8436-412d-87c9-d7d4fd59f688.png)

# Bitstream

A bitstream is a binary sequence that comprises a sequence of bits. These are used in FPGA applications for programming purposes and to establish communication channels. FPGA bitstream is a file containing the programming data associated with your FPGA chip.

# Counter Timing, Power and Area

Implementation of a design also gives details like the timing summary, device utilization, power analysis, etc. The below snippets show the brief timing sumary, implementation and power analysis of the up-counter desi

![19](https://user-images.githubusercontent.com/120499567/208598610-8a39dec5-f2d8-40b6-b0bc-f795e6846d62.png)

![16](https://user-images.githubusercontent.com/120499567/208598846-e41e5228-6fa7-4a56-8020-da0c99b9b030.png)

![17](https://user-images.githubusercontent.com/120499567/208599017-e2a997e3-e251-433e-92b8-581047260bc1.png)

![20](https://user-images.githubusercontent.com/120499567/208599541-3c51cda2-f6c3-4148-a547-29e0e0a2e5fe.png)

# Introduction To VIO

Virtual Input/Output (VIO) core is a customizable core that can both monitor and drive internal FPGA signals in real time. The number and width of the input and output ports are customizable in size to interface with the FPGA design

![download](https://user-images.githubusercontent.com/120499567/208612228-001debbf-7465-47f5-bd7c-dd7fda823d1a.png)








# Day 2 Exploring OpenFPGA, VPR and VTR

## Introduction To OpenFPGA

The OpenFPGA framework is the first open-source FPGA IP generator which supports highly-customizable homogeneous FPGA architectures. OpenFPGA provides a full set of EDA support for customized FPGAs, including Verilog-to-bitstream generation and self-testing verification. OpenFPGA targets to democratizing FPGA technology and EDA techniques, with agile prototyping approaches and constantly evolving EDA tools for chip designers and researchers.

Some key features of OpenFPGA are: - Use of Automation Techniques - Reduction of FPGA developement cycle to few days - Provides open source design tools

![image](https://user-images.githubusercontent.com/120499567/208614000-206356ab-0755-47c1-898c-ecdbacc8d015.png)



# VPR

VPR (Versatile Place and Route) is an open source academic CAD tool designed for the exploration of new FPGA architectures and CAD algorithms, at the packing, placement and routing phases of the CAD flow. As input, VPR takes a description of an FPGA architecture along with a technology-mapped user circuit. It then performs packing, placement, and routing to map the circuit onto the FPGA. The output of VPR includes the FPGA configuration needed to implement the circuit and statistics about the final mapped design (eg. critical path delay, area, etc).

![1](https://user-images.githubusercontent.com/120499567/208111957-8559ee0c-55da-4cc5-9ccc-8035fc3cbf63.png)

To invoke VPR from terminal:

``` js
$VTR_ROOT/vpr/vpr \
 $VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
 <blif-file-path \
 --route_chan_width 100 \
 --disp on
 ```
 
### The basic VPR flow involves below mentioned steps:

- Packing - combinines primitives into complex blocks
- Placment - places complex blocks within the FPGA grid
- Routing - determines interconnections between blocks
- Analysis - analyzes the implementation

# VTR

The Verilog to Routing (VTR) project provides open-source CAD tools for FPGA architecture and CAD research. The VTR design flow takes as input a Verilog description of a digital circuit, and a description of the target FPGA architecture

### To invoke VTR from command-line:

``` js
$VTR_ROOT/vtr_flow/scripts/run_vtr_flow.py \ 
  $VTR_ROOT/doc/src/<verilog-file-path>
  $VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
  -temp_dir . \
  --route_chan_width 100 
  ```
  
  # VTR Flow
  
  The basic VTR flow perfoms:
  
  - Elaboration & Synthesis (ODIN II)
  - Logic Optimization & Technology Mapping (ABC)
  - Packing, Placement, Routing & Timing Analysis (VPR)
  
  The snippets mentioned below show some of the stages from the VTR flow
  
 |![3](https://user-images.githubusercontent.com/120499567/208122727-21d023a3-5ac6-41f0-9157-3def64172de6.png)|![4](https://user-images.githubusercontent.com/120499567/208123121-e440bc63-31a3-4cbf-9b4a-3e8696970c21.png)|
 
 Nets
 ![Screenshot 2022-12-16 225338](https://user-images.githubusercontent.com/120499567/208615244-0096178e-1b63-4eaa-b8a7-0d2ed3b4ab6d.png)

## Timing Analysis VTR Flow

In order to perform timing analysis, a constraint file needs to be created. This constraint file is provided as an input to tool. To perform timing analysis from command-line, below mentioned switch should be enabled.

``` js
 ## .sdc constraint file is required
  --sdc_file <sdc-file-path>
  
  ```
  
  The snippets below show the setup and hold timing reports generated by the tool.
  
  ![hold](https://user-images.githubusercontent.com/120499567/208617740-57d9a862-2c25-49d1-9e59-ba366c3dd985.png)

![4](https://user-images.githubusercontent.com/120499567/208618171-6da19c2d-65cb-43f9-aa1f-837685df5baf.png)

## Post Synthesis Simulation

Post Synthesis simulation in VTR flow is same as Post Implementation simulations in general. To generate the post synthesis netlist for simulation, the below mentioned switch should be enabled in during the VPR stage in VTR flow.

 ## To generate post synthesis netlist
  --gen_post_synthesis_netlist on
  
  ![Screenshot 2022-12-19 000932](https://user-images.githubusercontent.com/120499567/208619481-5660eea7-1895-4972-8ad3-9b4768308039.png)


  
  
  

