# EXP 4: 4Bit Up-Down Counter Asynchronous Reset Counter-Synthesize the Gate Level Netlist and tabulate Area, Power and Timing reports.

## Aim:

Synthesize 4Bit-Up-Down-Asynchronous-Reset-Counter design using Constraints and analyse reports, Timing, area and Power.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,

◦ Liberty Files (.lib)

◦ Verilog/VHDL Files (.v or .vhdl or .vhd)

◦ SDC (Synopsis Design Constraint) File (.sdc)

 ### Step 2 : Creating an SDC , .v File

•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.

•	The SDC File must contain the following commands:

```
create_clock -name clk -period 2 -waveform {0 1} [get_ports "clk"]

set_clock_transition -rise 0.1 [get_clocks "clk"]

set_clock_transition -fall 0.1 [get_clocks "clk"]

set_clock_uncertainty 0.01 [get_ports "clk"]

set_input_delay -max 0.8 [get_ports "rst"] -clock [get_clocks "clk"]

set_output_delay -max 0.8 [get_ports "count"] -clock [get_clocks "clk"]

i→ Creates a Clock named “clk” with Time Period 2ns and On Time from t=0 to t=1.

ii, iii → Sets Clock Rise and Fall time to 100ps.

iv → Sets Clock Uncertainty to 10ps.

v, vi → Sets the maximum limit for I/O port delay to 1ps.
```
•	The .v File must contain the following commands:

```
`timescale 1ns / 1 ns
module counter(clk,m,rst,count);
input clk,m,rst;
output reg [3:0] count;
always@(posedge clk or negedge rst)
begin
if (!rst)
count=0;
else if(m)
count=count+1;
else
count=count-1;
end
endmodule
```
•	The Run.tcl File must contain the following commands:

```
read_libs /cadence/install/FOUNDRY-01/digital/90nm/dig/lib/slow.lib
read_hdl counter.v
elaborate
read_sdc input_constraints.sdc 

syn_generic
report_area
syn_map
report_area
syn_opt
report_area 

report_area > counter_area.txt
report_power > counter_power.txt
report_timing > counter_timing.txt
report_area > counter_cell.txt
report_gates > counter_gates.txt

write_hdl > counter_netlist.v
write_sdc > output_constraints.sdc 

gui_show

```

### Step 3 : Performing Synthesis

The Liberty files are present in the library path,

• The Available technology nodes are 180nm ,90nm and 45nm.

• In the terminal, initialise the tools with the following commands if a new terminal is being
used.

◦ csh

◦ source /cadence/install/cshrc

• The tool used for Synthesis is “Genus”. Hence, type “genus -gui” to open the tool.

• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

#### Synthesis RTL Schematic :

![Screenshot 2024-11-11 111631](https://github.com/user-attachments/assets/387667b9-2b3b-437a-921f-13d3a8d027a4)

#### Area report:

![Screenshot 2024-11-11 111935](https://github.com/user-attachments/assets/ee623d4e-a3b5-4d4c-89fc-58a9f8e9d36c)

#### Power Report:

![Screenshot 2024-11-11 111946](https://github.com/user-attachments/assets/6fc19761-8801-422e-91d0-d4dfa0a3134b)

#### Timing Report: 

![Screenshot 2024-11-11 111959](https://github.com/user-attachments/assets/a5b8760e-cec0-4ec1-82d0-af00d22b4c21)

#### Result: 

The generic netlist has been created, and area, power, and timing reports have been tabulated and generated using Genus.





