
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

![8](https://github.com/user-attachments/assets/b5370bc4-1ea9-46bd-9e17-9c39bd73ffdf)
![9](https://github.com/user-attachments/assets/750bc2e9-7f4d-4a1d-8895-ece57687fbf2)
![10](https://github.com/user-attachments/assets/983e02f7-37d5-4449-ad0e-b8255166eea6)

  Fast route guide present in openlane/designs/picorv32a/runs/26-03_08-45/tmp/routing directory
  ![Screenshot from 2025-02-19 18-42-21](https://github.com/user-attachments/assets/6bb161d6-d6b9-45ad-8705-20e13b3962f3)

  Final step is routing . to run routing use below command :

```
run_routing
```

To view the final design  with routing in Magic tool , go to the results/routing/ in  the runs folder of openlane directory by using below command :

```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &

```



## Acknowledgement


I want to give a big thank you to Mr. Kunal Ghosh, one of the founders of VLSI System Design (VSD) Corp. Pvt. Ltd., and Mr. Nickson Jose for their amazing help during the DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING workshop. They really know their stuff when it comes to chip design using OpenLANE software and other cool techniques. Their guidance has been super valuable in teaching me all about it.
