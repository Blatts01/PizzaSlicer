The tcl script build the FPGA project for the Ultra96-V2 Board. 
The script use two other IPs from outside the project: 

Xilinx DPU from https://github.com/Xilinx/Vitis-AI.git 
Resizer from https://github.com/Xilinx/PYNQ-HelloWorld.git

To build the tcl file, update the ip_repo_path in line 170 to your local paths of PYNQ-HelloWorld and the Vitis-AI repo. 
