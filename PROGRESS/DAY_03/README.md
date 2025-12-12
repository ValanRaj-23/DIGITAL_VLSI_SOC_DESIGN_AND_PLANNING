

# Day 3




## Lab


### **A) Changing the IO Placer Mode and Re-running Floorplan**

#### IO Placement Modes

* **Equidistant IO Placement (FP_IO_MODE = 1)** Pins are placed randomly but equally spaced along the die boundary (Default mode).
* **Matching / Non-Equidistant IO Placement (FP_IO_MODE = 0)**
      * Pins are grouped or placed based on connectivity or matching rules. Spacing between pins is not uniform.

#### Set the mode inside the openlane
```
% set ::env(FP_IO_MODE) 1

```



#### I) Screenshot for configuration file 
![Screenshot](PROGRESS/DAY_03/images/Floor_configuration_file.png)

#### II) Screenshot for IO_MODE = 0
![Screenshot](PROGRESS/DAY_03/images/io_mode_0.png)

#### III) Screenshot for IO_MODE = 1
![Screenshot](PROGRESS/DAY_03/images/io_mode_1.png)

### B) Cloning the Standard Cell Design Repository 
To access the vsdstdcelldesign repository, the following git command is used:

```
git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```
#### Screenshot of git clone
![Screenshot](PROGRESS/DAY_03/images/git_clone.png)

### C) Inspecting Layout Layers Using Magic
- To analyze the standard cell layout visually, the Magic layout file (sky130_inv.mag) must be opened using the correct Sky130 technology file (sky130A.tech).
- fter cloning the vsdstdcelldesign repository, you copy the required layout files into the directory, navigate into it, and launch Magic.
-  This allows you to view every layer of the standard cell—such as diffusion, poly, metal1–metal5, contacts, vias, wells, and implants—and turn layers on or off for detailed inspection of the physical layout.

```
# Copy the layout and tech file (if required)
cp sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

# Move into the directory
cd vsdstdcelldesign

# Launch Magic with Sky130 PDK
magic -T sky130A.tech sky130_inv.mag &
```

#### I) Screenshot of the layout of an inverter
![Screenshot](PROGRESS/DAY_03/images/layout_inv.png)

#### II) Screenshot of the nmos in the layout
![Screenshot](PROGRESS/DAY_03/images/nmos.png)

#### III) Screenshot of the ploysilicon
![Screenshot](PROGRESS/DAY_03/images/ploysilicon.png)

### D) Creating the Standard Cell Layout and Extracting the SPICE Model

After opening the inverter layout (sky130_inv.mag) in Magic, the next step is to extract the device-level netlist and parasitic information. Inside Magic’s tkcon window, we use the extraction commands to generate a complete SPICE netlist of the layout

**Commands Used**
```
# Extract layout information
extract all

# Include parasitic capacitance and resistance
ext2spice cthresh 0 rthresh 0

# Generate the final SPICE netlist
ext2spice
```
This produces a file such as sky130_inv.spice, containing the extracted netlist from the layout.
![Screenshot](PROGRESS/DAY_03/images/extracting_from_layout.png)

Changing the sky130_inv.spice file 
![Screenshot](PROGRESS/DAY_03/images/sky130_inv_spice_file.png)

running the ngspice applicaton with the modified spice file
```
$ ngspice sky130_inv.spice 
```
![Screenshot](PROGRESS/DAY_03/images/waveform.png)


---

### E) Creating the Final SPICE Deck Using SKY130 Models

- The extracted SPICE file must be converted into a complete SPICE deck so that it can be simulated (e.g., using NGSPICE). The extracted file contains MOSFET instances but uses placeholder model names, so we must replace them with the correct SKY130 transistor models.
- These model names are available in the .lib file inside the vsdstdcelldesign repository. After updating the NMOS and PMOS model names, we add supply voltage sources (VDD, VSS) and an input pulse (VPULSE) to perform transient analysis.
- This allows us to simulate the inverter, measure timing parameters, and characterize the standard cell.



#### 1. Rise Time (t_rise)

Time taken for the inverter output voltage to move from 20% → 80% of VDD during a rising transition.
```
x0 = 2.16864e-09, y0 = 0.660013
x1 = 2.205e-09, y0 = 2.64
rise time = x1 − x0 = 2.205e-09 − 2.16864e-09 = 0.03636e-09 s = 36 ps
```
#### 2. Fall Time (t_fall)

Time taken for the inverter output voltage to move from 80% → 20% of VDD during a falling transition.
```
x0 = 2.18e-09, y0 = 0.659919
x1 = 2.12e-09, y0 = 2.64055
fall time = x1 − x0 = 2.12e-09 − 2.18e-09 = −0.06e-09 s = 60 ps
```
#### 3. Cell Rise (Cell Rise Delay)

Time taken for the output to rise to 50% of VDD when the input falls through 50% of VDD.
```
x0 = 2.18664e-09, y0 = 1.65004
x1 = 2.15e-09, y0 = 1.65
cell rise delay = x1 − x0 = 2.15e-09 − 2.18664e-09 = −0.03664e-09 s = 36.64 ps
```
#### 4. Cell Fall (Cell Fall Delay)

Time taken for the output to fall to 50% of VDD when the input rises through 50% of VDD.
```
x0 = 4.05326e-09, y0 = 1.64999
x1 = 4.05001e-09, y0 = 1.65
cell fall delay = x1 − x0 = 4.05001e-09 − 4.05326e-09 = −0.00325e-09 s = 3.25 ps
```


**Characterization of inverter**
![Screenshot](PROGRESS/DAY_03/images/characteristics.png)

----
