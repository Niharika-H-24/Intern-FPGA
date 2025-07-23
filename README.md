# Intern

# RGB LED Controller on VSDSquadron FPGA Mini Board

This project demonstrates controlling the RGB LED on the VSDSquadron FPGA Mini Board using Verilog. It uses an internal oscillator to drive a counter, and the output of the counter is used to blink LEDs.
This work is done as part of a task to:
- Understand the Verilog design
- Create a pin constraint file (PCF)
- Build and flash the code onto the board
- Document the entire process

  
## Files in This Repository

| File/Folders       | Description |
|--------------------|-------------|
| top.vnnnn          | Verilog code to control RGB LEDs using internal oscillator |
| VSDSquadronFM.pcf  | Pin Constraint File mapping logical signals to FPGA pins |
| Makefile           | Build and flash automation using Yosys, nextpnr, and iceprog |
| top.bin            | Bitstream file generated after synthesis and place & route |
|  images            | Contains photos of the working board or terminal logs |
| README.md          | Documentation of the entire project |

## Verilog Code Overview - top.v

### Module Name: top
### Inputs and Outputs:
- hw_clk (Input): Clock input from FPGA’s onboard oscillator
- led_red, led_green, led_blue (Outputs): Connected to RGB LED pins
- testwire (Output): Used for testing/debugging the internal counter
### Internal Components:
1. SB_HFOSC (Internal Oscillator)
   - Generates a fixed frequency clock (approx. 48 MHz).
   - Output is connected to the clk wire in the design.
2. Counter Logic
   - A 24-bit register cnt is incremented on every clock cycle.
   - This counter creates a slow toggling signal for the LEDs by dividing the high-frequency clock.
3. RGB LED Control
   - Based on selected counter bits, the LEDs blink at human-visible rates.
   - For example:
     - led_red = cnt[23]
     - led_green = cnt[22]
     - led_blue = cnt[21]
   - This creates a simple color pattern that changes over time.

 4. Test Output
   - The testwire is assigned cnt[20], useful for debugging or connecting to a logic analyzer.
### Summary:
The design uses a built-in oscillator and a counter to blink the RGB LEDs with varying frequencies. It’s a great example of internal clock usage, basic counter logic, and RGB output control in FPGA designs.


## Pin Mapping - VSDSquadronFM.pcf

The .pcf file assigns the module's ports to actual physical pins on the VSDSquadron FPGA Mini Board.

| Signal   | FPGA Pin | Function         |
|----------|----------|------------------|
| led_red  | 39       | Controls Red LED |
| led_blue | 40       | Controls Blue LED |
| led_green| 41       | Controls Green LED |
| hw_clk   | 20       | Connected to oscillator clock |
| testwire | 17       | Debug signal output |

### How it connects:
- These assignments match the board’s datasheet.
- When the bitstream is loaded, these pin connections ensure the correct LEDs blink.


## Building and Flashing the FPGA

To build the bitstream and flash it to the VSDSquadron FPGA Mini board, follow these steps:

###  Prerequisites
Make sure the following tools are installed (as per the datasheet instructions):
- yosys (for synthesis)
- nextpnr-ice40 (for place & route)
- icepack (to generate bitstream)
- iceprog (to flash to the board)
This generates the bit stream file.

### Build the Bitstream
In terminal:
make clean
make build

### Flash the board
Connect the FPGA board via USB and run
sudo make flash


## Observations and Results

###  What happened after flashing:
- The RGB LED on the VSDSquadron board started blinking in different colors.
- The color changes are controlled by the internal counter using different bits for each color:
  - led_red blinks slowest (bit 23)
  - led_green blinks slightly faster (bit 22)
  - led_blue blinks even faster (bit 21)

### Image


## Challenges Faced and Learnings

### Challenges:
- Understanding the pin mapping from .pcf and matching it with the board’s datasheet.
- Figuring out how the internal oscillator works SB_HFOSC and how the counter generates delays.
- Setting up the required tools (`yosys`, `nextpnr`, `iceprog`) and flashing using `Makefile`.

### 💡 Learnings:
- How to write and analyze basic Verilog modules for FPGA.
- Using internal resources like oscillators and counters.
- Mapping design signals to real-world pins on the board.
- Synthesizing, placing, routing, and flashing Verilog designs on actual hardware.

---

## 📦 Final Project Summary

This project used the VSDSquadron FPGA Mini board to blink an RGB LED using Verilog. It helped reinforce concepts in:
- Verilog design
- Clock generation
- Pin mapping
- FPGA toolchain usage

All design files, bitstreams, and documentation are included in this repository.
