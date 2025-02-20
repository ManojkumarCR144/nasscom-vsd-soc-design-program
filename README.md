
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

## DAY 3

  1.Git clone vsdstdcelldesign    
  Commands to open inverter layout in magic  
  
```bash
  #Change directory to openlane
  cd Desktop/work/tools/openlane_working_dir/openlane

  #Clone the repository with custom inverter design
  git clone https://github.com/nickson-jose/vsdstdcelldesign

  #Change into repository directory
  cd vsdstdcelldesign

  #Copy magic tech file to the repo directory for easy access
  cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

  #Check contents whether everything is present
  ls

  #Command to open custom inverter layout in magic
  magic -T sky130A.tech sky130_inv.mag &
```
![Screenshot from 2025-02-11 17-59-47](https://github.com/user-attachments/assets/e215a120-f5d8-40b3-96bf-5780845bb61e)
![Screenshot from 2025-02-11 18-14-13](https://github.com/user-attachments/assets/4fcf3e00-d5f1-4046-aef6-646a6cd6fb11)
CMOS inverter layout
![Screenshot from 2025-02-11 18-13-33](https://github.com/user-attachments/assets/99fb518b-5e1f-4079-9f0b-9996aa039e41)
NMOS
![Screenshot from 2025-02-11 20-11-49](https://github.com/user-attachments/assets/c58490c4-4907-424f-824c-99662d47a3d0
PMOS
![Screenshot from 2025-02-11 20-12-38](https://github.com/user-attachments/assets/f8e0e6b4-df90-47dc-bad2-85a687003cc7)
Y(output) connecting the drain of nmos and pmos
![Screenshot from 2025-02-11 20-14-46](https://github.com/user-attachments/assets/c9a8e9d7-2582-451f-b2d5-a0f82da440b5)
Poly
![Screenshot from 2025-02-11 21-05-33](https://github.com/user-attachments/assets/e52c3d81-e176-4eb3-a7fa-5c3a51f8ab5b)
Spice file
![Screenshot from 2025-02-11 21-27-09](https://github.com/user-attachments/assets/ade380ef-7256-4d25-b451-040556db0666)

2.Editing the spice model file for analysis through simulation. Edited spice file
![Screenshot from 2025-02-13 20-25-03](https://github.com/user-attachments/assets/644240de-8de2-4eb2-ac8e-7be5dc1d92b7)

  3.Spice extraction of inverter in magic.  

```bash
  #Check current directory
  pwd

  #Extraction command to extract to .ext format
  extract all

  #Before converting ext to spice this command enable the parasitic extraction also
  ext2spice cthresh 0 rthresh 0

  #Converting to ext to spice
  ext2spice
```

![Screenshot from 2025-02-12 18-49-11](https://github.com/user-attachments/assets/34ba8037-ebc6-4c14-9135-dc2b271c4645)
  4.Ngspice simulation
  Commands for ngspice simulation
  
```bash
  #Command to directly load spice file for simulation to ngspice
  ngspice sky130_inv.spice

  #Now that we have entered ngspice with the simulation spice file loaded we just have to  load the plot
  plot y vs time a
```
![Screenshot from 2025-02-13 20-27-34](https://github.com/user-attachments/assets/d853404a-871c-410a-8a16-a7684042cf03)
When C=0.07fF
![Screenshot from 2025-02-13 20-24-05](https://github.com/user-attachments/assets/f2a4c53c-a8a8-4c3d-bd80-87c688decc69)
when C=2fF
![Screenshot from 2025-02-13 20-26-28](https://github.com/user-attachments/assets/bdc46569-22cb-44bf-a5d5-2a7c0ce60313)



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

Now the next step is to edit the config.tcl of picorv32a design.

config.tcl file is present in picorv32a folder


NOW go to the openlane directory run docker command to go to bash terminal

next commands are given below:


![Screenshot from 2025-02-19 21-30-38](https://github.com/user-attachments/assets/7de84003-2f36-4310-a85b-2c1d1e21b284)

![Screenshot from 2025-02-19 21-36-12](https://github.com/user-attachments/assets/36d722aa-a598-4b98-876f-e821641c197a)

![Screenshot from 2025-02-19 21-42-11](https://github.com/user-attachments/assets/8197b336-0a38-4e8a-b5fd-456dac23bb41)
![Screenshot from 2025-02-19 21-42-28](https://github.com/user-attachments/assets/3fc0f32e-56ee-4976-af08-07e1e7e530f6)
![Screenshot from 2025-02-19 21-42-58](https://github.com/user-attachments/assets/04fb8e6b-ac96-4d0f-98e8-20b17ab55dc3)

![Screenshot from 2025-02-19 21-43-22](https://github.com/user-attachments/assets/87b86adc-0815-4f3c-9be7-df62fa51b075)
![Screenshot from 2025-02-16 13-09-57](https://github.com/user-attachments/assets/8f5171f6-533a-4255-94a4-9ab223c38b96)
![Screenshot from 2025-02-16 13-14-07](https://github.com/user-attachments/assets/28059584-1605-4972-883e-e7fae4cfb3d3)

![Screenshot from 2025-02-16 13-14-15](https://github.com/user-attachments/assets/fd37a774-9c2a-413c-a3e3-285a2d395ebf)
![Screenshot from 2025-02-17 18-09-17](https://github.com/user-attachments/assets/39fcf3f3-d2ae-4d4c-897a-a6eac3598dea)

![Screenshot from 2025-02-19 23-18-48](https://github.com/user-attachments/assets/e93e6114-5fc2-4be6-baf2-67ce7e424983)
![Screenshot from 2025-02-19 23-19-36](https://github.com/user-attachments/assets/e1cfd5de-92ed-47bb-
![Screenshot from 2025-02-16 15-12-05](https://github.com/user-attachments/assets/931dabde-1ab4-4631-8ec4-4af44e7a2cda)

![Screenshot from 2025-02-16 15-12-09](https://github.com/user-attachments/assets/c466bd9f-f718-43fa-82d4-9b6b5c26465d)

![Screenshot from 2025-02-16 17-08-21](https://github.com/user-attachments/assets/0b02a770-96c7-4db8-8c8c-528d0fff9012)

![Screenshot from 2025-02-16 17-12-36](https://github.com/user-attachments/assets/6bd987b2-9e48-4c9c-89c2-d544ede67c08)

![Screenshot from 2025-02-16 18-48-07](https://github.com/user-attachments/assets/2af07a93-bcf0-4f11-8ae9-7a4bcbfab694)
![Screenshot from 2025-02-16 18-51-19](https://github.com/user-attachments/assets/8f184dc9-05eb-4f13-9afb-aacde9807e96)




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
