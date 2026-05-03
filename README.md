# PC-on-PCPU-nano-1-t4

**Building a modular computer based on the 1-bit PCPU-NANO1 T4 processor**

This project is an implementation of the idea of building a fully-fledged, modular computer starting from the very foundation: a 1-bit processor made from discrete transistors. Inspired by the `PCPU-NANO1` architecture (ppc-8051), this project focuses on practical assembly, debugging, and scaling.

**Key goal:** To create not just a transistor blinker, but a fully functional, expandable computer capable of executing programs and interacting with users and peripherals.

---

## 📦 Repository Structure

The project is divided into three independent PCBs, following a modular philosophy.

*   **`PCB_PCPU`** : The processor board. This is where the `PCPU-NANO1 T4` resides — the 1-bit core built using an optimized schematic (4 transistors, 7 resistors).
*   **`PCB_MotherBoard`** : The motherboard that ties all components together. It contains a socket for the CPU, program memory (EEPROM `AT28C256`), a program counter (`SN74HC163N`), and a clock generator.
*   **`PCB_Debugging_Module`** : A debug module for manual program entry (in the style of the "Micro-80"). Allows you to set the address and data, write them to memory, and visually monitor the buses.

---

## 🖥️ System Architecture

The system is built on a modular principle, allowing each part to be developed and debugged independently.

1.  **Processor (`PCPU-NANO1 T4`)** : A 1-bit core. It performs a single `NAND(DATA, IN)` operation while also calculating the conditional jump signal `JMP = INS AND DATA AND IN`.
2.  **Program Counter (`SN74HC163N`)** : Generates the address of the current instruction. It can increment and load a new value (the jump address) when it receives the `JMP` signal.
3.  **Program Memory (EEPROM `AT28C256`)** : Stores the firmware. Organized as `32K x 8`. For the `PCPU`'s 8-bit addressing, only the lower 8 address lines (`A0-A7`) are used; the higher lines (`A8-A14`) are tied to `GND`.

---

## 🔌 How It Works

1.  The program counter (`SN74HC163N`) generates an address.
2.  This address goes to the EEPROM (`AT28C256`), which outputs an 8-bit word from the selected cell: one bit is `INS`, another is `DATA`.
3.  The processor (`PCPU`) receives `INS` and `DATA` and calculates the result.
4.  If the jump condition is met (`JMP = 1`), the program counter loads a new address (either from a separate "Address Memory" or, by default, 0).
5.  The cycle repeats.

---

## 🛠️ System Components

### Main System (Minimum Configuration)

| Component | Model | Purpose |
|-----------|-------|---------|
| **Processor Core** | `PCPU-NANO1 T4` | Performs NAND operation and generates `JMP` |
| **Program Counter** | `SN74HC163N` (2x for 8-bit) | Generates the instruction address |
| **Program Memory** | `AT28C256` (DIP-28) | Stores the program code (`INS`, `DATA`) |
| **Power Supply** | +5V, regulated | Powers the entire system |
| **Debug Module** | Module for writing data to the eeprom |


---

## 🧪 Assembly and Debugging Steps

1.  **Build the `PCPU-NANO1 T4` core** : Verify the truth table on a breadboard (apply signals with jumpers, check `OUT` and `JMP`).
2.  **Build the motherboard** : Assemble the program counter, clock generator, and CPU socket.
3.  **Verify memory operation** : Program the EEPROM with a simple test program (e.g., blinking an LED on `OUT0` via `NADJZ`).
4.  **Integrate the CPU and memory** : Program the EEPROM with alternating instructions (`INS=1/DATA=1` and `INS=0`) and observe the address increment.
5.  **Connect the debug module** : Verify manual input functionality and bus isolation.

---

## 🔮 Development Roadmap

*   ✅ **1-bit core** (PCPU-NANO1 T4) — stable on a breadboard.
*   ✅ **Program counter and EEPROM** — design proven.
*   ✅ **Debug module** — designed.
*   ❌ **8-bit processor** (`PCPU-micro 8`) — under development (scaling by using 8 parallel cores).
*   ❌ **ATtiny-based video card** — text output to a UART TFT display (planned).
*   ❌ **PS/2 keyboard support** — via a converter (e.g., `CH9328` or `FT260`).

---

## 🤝 Acknowledgements

*   **ppc-8051** — for the original `PCPU-NANO1` architecture that inspired this project.

---

## 📜 License

MIT License — free to use, modify, and distribute.

---

## ⚠️ Important Note

All information is provided "as is" for educational purposes. The author is not responsible for any potential errors.

**Happy building!**
