# DIGITAL_VLSI_SOC_DESIGN_AND_PLANNING



This project demonstrates the complete RTL-to-GDSII physical design flow using the **OpenLANE** toolchain and the **SkyWater SKY130 PDK**, starting from Verilog RTL and ending with a manufacturable GDSII layout.


---


## Setup


- Downloaded the **openlane.vdi** file. 
- Imported the VDI into the Virtual Machine. 
- Navigated to the OpenLANE working directory:<br>
   ```bash
    vsduser@vsdsquadron:~/Desktop/work/tools/openlane_working_dir/openlane
   ```
## OpenLANE – SETUP


```bash
1. Enters the Docker container where the OpenLANE environment is installed.
-> docker


2. Runs the OpenLANE flow script in interactive mode.
-> ./flow.tcl -interactive


3. Loads the OpenLANE Tcl package (version 0.9) inside the interactive shell.
-> package require openlane 0.9


4. Initializes the setup for the design picorv32a.
-> prep -design picorv32a
```
Creates the required **runs/** directory and prepares all configuration files needed for the flow.






# Day 1


## Theory


Introduction to ASIC design flow and its fundamental elements:


- Package 
- Die 
- Core 
- IPs 
- Overview of the complete RTL → GDSII flow 


---


## Lab


### **a) Synthesis of PicoRV32A**

Inside the OpenLANE interactive window, the synthesis step was performed:
```
$ run_synthesis
```
This command executes Yosys + ABC to generate the synthesized netlist and cell statistics in the **run/** folder.


#### I) Screenshot for run_synthesis cmd

#### II) Screenshot of the files that is being generate after run_synthesis


### **b) Calculation for D flipflop ratio**

The synthesis report provides two key values:

   - Total number of cells in the design
   - Total number of D flip-flops (DFFs)

These values are used to compute the Flip-Flop Ratio, which indicates the percentage of sequential elements in the synthesized design.

#### Formula
   ``` 
   Flop Ratio = (Number of DFFs) / (Total Number of Cells) 

   ```

From the synthesis report:

- Number of DFFs = 1613
- Total number of cells = 14676

```
Flop Ratio = 1613 / 14676 ≈ 0.1084
Flip-Flop Ratio ≈ 0.1084 (or 10.84%)
```
#### Screenshot of synthesis report

---






