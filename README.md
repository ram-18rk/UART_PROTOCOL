# UART Protocol – 8-Bit RTL Design (Verilog | Vivado | FPGA)

This repository contains a complete, synthesizable **8-bit UART** implementation using Verilog RTL.  
The design includes a **baud-rate generator**, **transmitter**, **receiver**, and a **loopback testbench**.  
It is fully verified through simulation, synthesized in Vivado, implemented on FPGA, and **meets all timing constraints at 100 MHz**.

---

## 📁 Repository Structure
/src → RTL source files (baud_rate, uart_tx, uart_rx, uart_top)


/tb → Loopback testbench


/docs → Waveforms, timing reports, power analysis, schematics


/constraints → XDC constraint file (clock rule)


---

## 🧩 UART Design Overview

### 🔹 Baud-Rate Generator  
- Parameterized system clock & baud rate  
- Generates a single-cycle `tick` for TX/RX timing  

### 🔹 UART Transmitter (TX)
- Converts 8-bit parallel data into a UART frame  
- Sends: **Start bit → 8 Data bits → Stop bit**  
- Shift-register based  
- `tx_busy` flag indicates active transmission  

### 🔹 UART Receiver (RX)
- Detects start bit  
- Uses **half-bit alignment** for mid-bit sampling  
- Reconstructs 8-bit data  
- Outputs `rx_valid` pulse when a byte is received  

### 🔹 Top Module
- Integrates baud generator, TX, and RX  
- Used for simulation and FPGA deployment  

---

# 1️⃣ Simulation (Loopback Verification)

### ✔ Testbench Behavior
- TX sends `0xA5`
- RX receives **the same byte**
- `rx_valid` pulses high after frame completion

### ✔ Simulation Confirms
- Correct start/data/stop bit timing  
- Accurate mid-bit sampling  
- Full end-to-end UART functionality  

📌 Waveform images:  
`docs/waveforms/`

---

# 2️⃣ Synthesis (Vivado)

### ✔ Clock Constraint  


create_clock -name sys_clk -period 10.000 [get_ports clk] # 100 MHz


### ✔ Synthesis Result Summary
- **LUTs:** 25  
- **Flip-Flops:** 46  
- **DSP/BRAM:** 0  
- Clean RTL schematic generated  
- No critical warnings  

📌 Synthesis report available in:  
`docs/synthesis/`

---

# 3️⃣ Implementation (Place & Route)

### ✔ Post-Implementation Timing Summary
| Metric | Value |
|--------|--------|
| Worst Setup Slack (WNS) | **+8.769 ns** |
| Worst Hold Slack (WHS) | **+0.053 ns** |
| Pulse Width Slack | **+4.725 ns** |
| Violations | **0** |

➡️ **ALL timing constraints met at 100 MHz**

### ✔ Power Summary

Total Power : 0.225 W
Static Power: 0.221 W
Dynamic Power: 0.004 W


📌 Implementation reports:  
`docs/implementation/`

---

# 4️⃣ Bitstream Generation

After implementation, Vivado generates:

uart_top.bit


This bitstream is ready to be flashed on FPGA hardware.

---

# 5️⃣ Hardware Testing Guide

### ✔ Connections
- **FPGA TX → USB-TTL RX**  
- **FPGA RX ← USB-TTL TX**

### ✔ Serial Settings
- Baud: depends on your baud generator parameter  
- Data bits: 8  
- Parity: None  
- Stop bits: 1  

### ✔ Result
FPGA sends/receives UART frames correctly.

---

# 🔮 Future Enhancements
- Parity bit support  
- Multiple stop bits  
- TX/RX FIFO buffers  
- AXI/Wishbone interface wrapper  
- Framing & parity error detection  

---




**RK**  
Electronics & Communication Engineering  
RTL | Verilog | FPGA | Digital Design  


