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
