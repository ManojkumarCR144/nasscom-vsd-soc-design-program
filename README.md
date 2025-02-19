
# NASSCOM-VSD-SoC-Design-Program  
Exploring the ASIC design flow using OpenLane and the SkyWater 130nm PDK, a collaboration between Google and SkyWater. This program, led by industry expert Kunal Ghosh, provides hands-on experience in standard cell design, physical design, and GDSII generation.  

## 📌 About the Repository  
This repository documents my learnings from a 5-day intensive workshop on VLSI SoC design.  

### 🔹 **Note:**  
*All block diagrams and flowcharts in this repository are sourced from the VLSI System Design (VSD) SoC Design course.*  

[Visit VLSI System Design (VSD)](https://www.vlsisystemdesign.com/)  

## 📖 Table of Contents  
### 🟢 **Day 1: Introduction & Synthesis**  
- Introduction to ASIC Design Flow  
- Overview of OpenLane and STRIVE Chipsets  
- Commands for OpenLane Flow  
- Tool Invocation & Operation  
- Running Synthesis on a Design  

### 🟢 **Day 2: Floorplanning & Placement**  
- Chip Floorplanning Considerations  
- Steps for Running Floorplan & Reviewing Files  
- Placement Execution and Analysis  

### 🟢 **Day 3: Standard Cell Design & Characterization**  
- Standard Cell Design using Magic & NGSPICE Characterization  
  - Creating a Standard Cell Layout & Extracting Spice Netlist  
  - CMOS Inverter Layout in Magic  
  - Netlist Extraction & Transient Analysis  
- Introduction to Magic and Loading Sky130 Tech Rules  

### 🟢 **Day 4: Timing Analysis & Clock Tree Synthesis (CTS)**  
- Pre-Layout Timing Analysis & CTS  
- Converting Magic Layout to Standard Cell LEF  
- Optimizing Synthesis to Fix Slack  
- Timing Analysis using OpenSTA  
- Post-CTS OpenROAD Timing Analysis  

### 🟢 **Day 5: RTL-to-GDS Flow Completion**  
- Finalizing RTL-to-GDS Flow using TritonRoute & OpenSTA  

## 🏆 Acknowledgment  

---
## DAY 4
### PRELAYOUT TIMING ANALYSIS AND CLOCK TREE SYNTHESIS <ul>
The next step is to get the **.lef** file from the inverter design.

Open the tracks.info file 
directory is : 
```
openlane_working_dir/pdk/sky130/libs.tech /openlane/sky130_fd_sc_hd/track.info 
```
![Screenshot from 2025-02-16 11-17-50 (1)](https://github.com/user-attachments/assets/feec19de-d301-4c0c-80be-293e93c39969)

Change grid into tracks in console window as shown in below image:
![Screenshot from 2025-02-16 11-49-48](https://github.com/user-attachments/assets/cf4797e8-be36-45c4-8b3c-8567d64f7a50)
![Screenshot from 2025-02-16 11-57-01](https://github.com/user-attachments/assets/3dcc28d8-276e-4cee-b191-e14e6bfca109)
![Screenshot from 2025-02-16 11-57-21](https://github.com/user-attachments/assets/5769a86c-af2d-40dc-958d-48ddde636a1c)

Intersection of grids at the input and output os the inverter
![Screenshot from 2025-02-14 18-10-10](https://github.com/user-attachments/assets/52e5b982-3d2a-4c3a-85e5-bb8090cf8960)
![Screenshot from 2025-02-14 18-19-18](https://github.com/user-attachments/assets/3e3dcb42-2e8f-4195-864c-03991e8ed3e5)



*<li>Steps to convert magic layout to std cell LEF</li>*

for the extraction of lef file ,enter the given below command in the tckon window.


lef write

![Screenshot from 2025-02-14 18-25-43](https://github.com/user-attachments/assets/5ee9fbfd-38fb-4fbb-8c32-c1cc2949a796)
![Screenshot from 2025-02-14 18-26-20](https://github.com/user-attachments/assets/d2db057a-c944-4894-b920-b2b8b1475c60)

now the *sky130_inv.lef* file is created in *vsdstdcelldesign* folder in *openlane* directory.

![Screenshot from 2025-02-16 12-08-06](https://github.com/user-attachments/assets/973b2714-de98-44ed-a8dd-f1e672557434)

The next step is to copy the newly created *.lef* file to src folder of *picorv32a* directory along with some important libraries.

libraries are present in following directory
![Screenshot from 2025-02-19 22-43-33](https://github.com/user-attachments/assets/adc301d6-c452-4740-8c96-90cb40d68b33)
![Screenshot from 2025-02-19 22-44-51](https://github.com/user-attachments/assets/ada15ecb-4b4e-452a-b53e-362aaaa9c762)





## DAY 5 

###  Final step for RTL2GDS using tritinRoute and openSTA

first step is to build  the power distribution network by follwing command :
```
gen_pdn

```

![Screenshot from 2025-02-16 18-57-08](https://github.com/user-attachments/assets/319210c7-e6c8-4ab0-8b19-2da4ea31682a)
![Screenshot from 2025-02-16 18-57-37](https://github.com/user-attachments/assets/d22dfa7f-6417-4364-b771-9e6425a0bd4a)


To check PDN, open Magic tool go to /tmp/floorplan/ indside the run folder in openlane directory by using below commands :

```

magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```  Commands to load PDN def in magic in another terminal

```bash
  #Change directory to path containing generated PDN def
  cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/03-02_22-08/tmp/floorplan/

  #Command to load the PDN def in magic tool
  magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```

![3](https://github.com/user-attachments/assets/fe10cc3a-e698-4285-94ac-a2f22c2ad5bc)
![4](https://github.com/user-attachments/assets/9fd43f7a-7363-4929-bb47-2f5bd4de3534)
  Final step is routing to run routing use below command :

```
run_routing
```
![Screenshot from 2025-02-16 21-37-40](https://github.com/user-attachments/assets/cb45aa76-93e5-41ae-ab8c-01f3098aa726)
Routing is done.
![Screenshot from 2025-02-16 22-43-30](https://github.com/user-attachments/assets/0339cc25-b809-4d44-9ea9-9f8a563eb4cc)


To view the final design  with routing in Magic tool , go to the results/routing/ in  the runs folder of openlane directory by using below command :

```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &

```


![8](https://github.com/user-attachments/assets/b5370bc4-1ea9-46bd-9e17-9c39bd73ffdf)
![9](https://github.com/user-attachments/assets/750bc2e9-7f4d-4a1d-8895-ece57687fbf2)
![10](https://github.com/user-attachments/assets/983e02f7-37d5-4449-ad0e-b8255166eea6)

  Fast route guide present in openlane/designs/picorv32a/runs/26-03_08-45/tmp/routing directory
  ![Screenshot from 2025-02-19 18-42-21](https://github.com/user-attachments/assets/6bb161d6-d6b9-45ad-8705-20e13b3962f3)

## Acknowledgement


I want to give a big thank you to Mr. Kunal Ghosh, one of the founders of VLSI System Design (VSD) Corp. Pvt. Ltd., and Mr. Nickson Jose for their amazing help during the DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING workshop. They really know their stuff when it comes to chip design using OpenLANE software and other cool techniques. Their guidance has been super valuable in teaching me all about it.
