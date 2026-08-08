# UART Transmitter using Verilog HDL

A synthesizable **8-bit UART Transmitter** designed using Verilog HDL. The design implements standard **8-N-1 UART serial communication**, including a start bit, eight data bits transmitted LSB first, and a stop bit.

The design is verified through a Verilog testbench using simulation waveforms.

---

## 📌 Project Overview

UART (Universal Asynchronous Receiver/Transmitter) is a widely used serial communication protocol for transferring data between digital systems.

This project implements the **transmitter section of a UART interface**. When a transmission request is received, the transmitter loads an 8-bit data value and serially transmits it through the `tx` output.

The transmitter uses an FSM to control the transmission sequence:

```text
IDLE → START → DATA → STOP → IDLE
```

---

## 🎯 Objectives

* Design an 8-bit UART transmitter using Verilog HDL
* Implement 8-N-1 UART frame format
* Transmit data LSB first
* Implement start and stop bits
* Generate baud-rate timing using a clock counter
* Implement FSM-based transmission control
* Provide `busy` and `tx_done` status signals
* Verify the RTL design using a simulation testbench

---

## 🧩 Block Diagram

<img width="1536" height="1024" alt="blockdiagran_uart" src="https://github.com/user-attachments/assets/ac27cecf-c4c2-4ff9-98ce-428cbf14fa8f" />



---

## 🔌 Input and Output Signals

| Signal     | Direction | Width | Description                          |
| ---------- | --------- | ----: | ------------------------------------ |
| `clk`      | Input     |     1 | System clock                         |
| `reset`    | Input     |     1 | Active-high synchronous reset        |
| `tx_start` | Input     |     1 | Starts a new transmission            |
| `tx_data`  | Input     |     8 | 8-bit parallel data to transmit      |
| `tx`       | Output    |     1 | Serial UART output                   |
| `busy`     | Output    |     1 | Indicates active transmission        |
| `tx_done`  | Output    |     1 | Indicates completion of transmission |

---

## 📡 UART Frame Format

The design follows the standard **8-N-1** UART format:

```text
        START       DATA BITS                    STOP
          ↓              ↓                         ↓

Idle ──── 0 ─── D0 ─ D1 ─ D2 ─ D3 ─ D4 ─ D5 ─ D6 ─ D7 ─── 1 ─── Idle
                 └──────── LSB FIRST ────────────┘
```

### Frame structure

```text
1 Start Bit
8 Data Bits
0 Parity Bits
1 Stop Bit
```

Therefore:

```text
8-N-1
```

---

## ⚙️ Internal Architecture

The UART transmitter consists of the following major blocks:

### 1. 8-bit Data Register

Stores the parallel input data before transmission.

```text
tx_data[7:0]
     ↓
Data Register
```

### 2. Baud-Rate Counter

Generates the timing required to transmit each UART bit for the required number of clock cycles.

### 3. Bit Index Counter

Keeps track of which data bit is currently being transmitted.

```text
D0 → D1 → D2 → D3 → D4 → D5 → D6 → D7
```

### 4. Control FSM

Controls the complete transmission sequence:

```text
IDLE
  ↓
START
  ↓
DATA
  ↓
STOP
  ↓
IDLE
```

### 5. Serial Output

The selected start, data, or stop bit is driven onto the `tx` output.

---

## 🧠 State Machine

### IDLE

* `tx = 1`
* `busy = 0`
* Waits for `tx_start`

### START

* `tx = 0`
* Sends the UART start bit
* Transmission becomes active

### DATA

* Sends 8 data bits
* Data is transmitted **LSB first**
* `bit_index` increments from 0 to 7

### STOP

* `tx = 1`
* Sends the stop bit
* `tx_done` is asserted
* Returns to IDLE

---

## 🔄 Transmission Flow

```text
tx_start = 1
     ↓
Load tx_data
     ↓
Set busy = 1
     ↓
Send START bit
     ↓
Send D0
     ↓
Send D1
     ↓
Send D2
     ↓
Send D3
     ↓
Send D4
     ↓
Send D5
     ↓
Send D6
     ↓
Send D7
     ↓
Send STOP bit
     ↓
tx_done = 1
     ↓
busy = 0
     ↓
Return to IDLE
```

---

## 💻 Verilog Files

### Design

`uart_tx.v`

Contains the synthesizable UART transmitter RTL implementation.

### Testbench

`tb_uart_tx.v`

Provides:

* Clock generation
* Reset generation
* Transmission requests
* Test data
* VCD waveform generation
* Simulation control

---

## 🧪 Simulation

The testbench transmits two example bytes:

```text
0xA5
0x3C
```

### First transmission

```text
tx_data = 8'hA5
```

Binary representation:

```text
1010_0101
```

UART transmits the data **LSB first**:

```text
1 0 1 0 0 1 0 1
```

### Second transmission

```text
tx_data = 8'h3C
```

Binary representation:

```text
0011_1100
```

LSB-first transmission:

```text
0 0 1 1 1 1 0 0
```

---

## 📈 Expected Waveform

The simulation waveform should show:

```text
reset
tx_start
tx_data
busy
tx
tx_done
```

The `tx` signal should remain HIGH when idle and follow the UART frame during transmission:

```text
IDLE → START → DATA → STOP → IDLE
  1      0      DATA     1      1
```

> **Add your simulation waveform screenshot here.**

```text
[INSERT WAVEFORM IMAGE HERE]
```

---

## 🛠️ Tools Used

* **Verilog HDL**
* **Icarus Verilog**
* **EDA Playground**
* **EPWave**
* **GitHub**

---

## 📚 Concepts Demonstrated

* RTL Design
* Sequential Logic
* Finite State Machines
* UART Protocol
* Serial Communication
* Baud-Rate Timing
* Counters
* Registers
* LSB-First Data Transmission
* Verilog Testbench Development
* RTL Simulation
* Waveform Analysis

---

## 🚀 Future Improvements

The design can be extended with:

* Parameterized baud-rate configuration
* Configurable clock frequency
* UART Receiver
* Full UART Transceiver
* Parity-bit support
* 7/8/9-bit configurable data
* Configurable stop bits
* FIFO-based UART buffering

---

## 📌 Conclusion

This project demonstrates the RTL implementation and simulation of an **8-bit UART Transmitter** using Verilog HDL. The design uses an FSM, counters, and registers to generate a standard 8-N-1 UART serial frame and provides status signals for transmission control.

It provides a foundation for developing more advanced UART-based communication systems and SoC peripheral interfaces.
## 👩‍💻 Author

**Dikshitha M.**

Electronics and Communication Engineering | RTL/VLSI Enthusiast

🔗 **LinkedIn:** [Dikshitha M.](https://www.linkedin.com/in/dikshitha-m-34355b308/)

---

## 📄 License

This project is created for educational and portfolio purposes.


