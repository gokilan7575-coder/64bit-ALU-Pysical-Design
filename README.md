# 64-bit ALU RTL-to-GDSII Physical Design Flow

## Project Overview

This project implements a complete RTL-to-GDSII ASIC physical design flow for a 64-bit Arithmetic Logic Unit (ALU) using Cadence Genus and Innovus tools in 45nm technology.

The project includes RTL synthesis, floorplanning, IO pin placement, placement, routing, verification, and final GDSII generation.

---

## Tools Used

- Cadence Genus
- Cadence Innovus

---

## Technology

- 45nm Standard Cell Library

---

## ALU Operations Supported

The 64-bit ALU supports:

- Addition
- Subtraction
- AND
- OR
- XOR
- NOT
- Left Shift
- Right Shift
- Set Less Than (SLT)

---

## RTL Design

```verilog
module alu64 (
    input  wire [63:0] a,
    input  wire [63:0] b,
    input  wire [3:0]  alu_ctrl,
    output reg  [63:0] result,
    output wire        zero,
    output wire        carry,
    output wire        overflow
);
```

---

## RTL-to-GDSII Flow

```text
Genus → Netlist
       ↓
Innovus
Import Design
       ↓
Floorplan
       ↓
IO Pin Placement
       ↓
Placement
       ↓
NanoRoute (routeDesign)
       ↓
Verification (DRC + Connectivity)
       ↓
GDSII
```

---

## Genus Synthesis Flow

### Synthesis Steps

- RTL Read
- Library Read
- Elaboration
- Generic Synthesis
- Technology Mapping
- Optimization
- Timing, Area, and Power Reports

### Genus TCL Script

```tcl
read_hdl {alu64.v
}

read_lib {<library path>/45nm/lib/fast.lib}

elaborate

syn_generic
syn_map
syn_opt

report_qor    > work/alu64.rpt
report_timing -max_paths 100 -unconstrained > gkproject5/work/alu4.rpt
report_area   > work/alu64area.rpt
report_power  > work/alu64power.rpt
report_cell   > work/alu64cell.rpt
report_clocks > work/alu64clocks.rpt

write_hdl  > work/alu64netlist.v
write_sdf  > work/alu64.sdf
```

---

## Innovus Physical Design Flow

### Physical Design Steps

- Import Design
- Floorplanning
- Instance Grouping
- Fence Creation
- Placement
- Routing
- DRC Verification
- Connectivity Check
- Geometry Verification

### Innovus TCL Script

```tcl
set init_design_uniquify 1
set init_no_new_assigns 1

set init_lef_file {<library path>/ 45nm/lef/45_tech.lef 
}

set init_verilog { alu64netlist.v
}

set init_pwr_net VDD
set init_gnd_net VSS

init_design

floorPlan -site CoreSite -r 1.0 0.7 20 20 20 20
place_design
checkPlace

placeDesign -incremental

routeDesign

verifyConnectivity > top_connectivity.rpt

verify_drc > top_drc.rpt

verifyGeometry > top_geometry.rpt

report_area > top_area.rpt

editDelete -type Regular
editDelete -type special

ecoRoute -fix_drc

report_place_status
```

---

## Project Structure

```text
64bit-ALU-RTL-to-GDSII/
│
├── RTL/
│   └── alu64.v
│
├── TCL/
│   ├── genus.tcl
│   └── innovus.tcl
│
├── Reports/
│   ├── timing_report.txt
│   ├── area_report.txt
│   ├── power_report.txt
│   └── drc_report.txt
│
├── Screenshots/
│   ├── floorplan.png
│   ├── placement.png
│   ├── routing.png
│   └── final_layout.png
│
└── README.md
```

---

## Screenshots

# Synthesis (Genus)
schematic
<img width="1250" height="698" alt="WhatsApp Image 2026-05-10 at 6 08 53 PM" src="https://github.com/user-attachments/assets/477387c7-5e1d-4cba-a4eb-897d03429f77" />

## Physical Design (Innovus)

##Power Planning: Creating Power Rings and Straps (VDD/GND) to ensure robust power delivery.

<img width="738" height="646" alt="WhatsApp Image 2026-05-10 at 6 08 52 PM" src="https://github.com/user-attachments/assets/636ebac7-da27-4a8c-b283-15bc9fbeff2c" />


### Final Layout
<img width="1280" height="731" alt="WhatsApp Image 2026-05-10 at 6 07 45 PM" src="https://github.com/user-attachments/assets/608db3b3-2f21-443e-ad54-33f985545737" />


---

## Verification

- DRC Verification
  <img width="644" height="309" alt="WhatsApp Image 2026-05-10 at 6 08 52 PM" src="https://github.com/user-attachments/assets/694c8498-28e8-4947-9669-461c1b904927" />

- Connectivity Verification
- Geometry Verification

---

## Results

- Successful RTL-to-GDSII implementation
- Physical design flow completed successfully
- Timing, area, and power reports generated
- Final routed layout generated

---

## Author

Gokilan.T

```
