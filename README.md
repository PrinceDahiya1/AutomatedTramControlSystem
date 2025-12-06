# Airport Terminal Tram Controller

## Project Overview
This project is a digital logic design for an automated tram system for a circular airport terminal. The system controls a tram that navigates between 16 gates (0-F), managing movement direction, speed, and door safety mechanisms.

The design implements a **Synchronous Finite State Machine (FSM)** using a Moore Machine architecture to control an ALU-based datapath. It was designed and simulated using the [Digital](https://github.com/hneemann/Digital) logic simulator.

## Features
* **Circular Navigation:** Navigates 16 gates in a loop (0 -> F or F -> 0).
* **Direction Memory:** "Smart" logic tracks the prior direction of movement to handle complex commands.
* **Variable Speed:** Supports single-gate movement and double-gate "express" jumps.
* **Safety Interlocks:** Doors (Green LED) only open when the tram is fully stopped. Red LED indicates motion.
* **Visual Feedback:** 7-Segment display shows current gate location in Hexadecimal.

## Files Included
* `tram_controller.dig` - The main system schematic (FSM + Datapath).
* `alu.dig` - Arithmetic Logic Unit component (imported).
* `four_bit_reg.dig` - 4-bit Register component (imported).
* `README.md` - Project documentation.

## Requirements
* **Software:** [Digital (HNEemann)](https://github.com/hneemann/Digital)
* **Java:** Requires Java Runtime Environment (JRE) to run Digital.

## How to Simulate
1. **Launch Digital** and open `tram_controller.dig`. (To Launch Digital: `java -jar C:\iverilog\Digital\Digital.jar`)
2. Click the **Play (Start Simulation)** button in the toolbar.
3. **System Reset (Critical First Step):**
    * Turn the `Reset` switch **ON** (High).
    * Toggle the `Clk` (Clock) once.
    * Turn `Reset` **OFF** (Low).
    * *Result:* Display should show `0`, Green LED should be ON.

### Control Modes (Input C)
The system uses a 2-bit input `C` (Input Bits: 2) to control behavior:

| Input C (Binary) | Mode | Behavior |
| :--- | :--- | :--- |
| **00** | **Stop / Wait** | Tram stays at current gate. Doors Open (Green LED). |
| **01** | **Move Up** | Tram moves Forward (+1). Doors Close (Red LED). |
| **10** | **Move Down** | Tram moves Backward (-1). Doors Close (Red LED). |
| **11** | **Move 2 Gates** | Tram jumps 2 gates in the **prior direction**. |

## Design Architecture
* **Controller (FSM):**
    * Implemented using 3 **D-AS Flip-Flops** (Asynchronous Set/Reset).
    * Uses **Moore Machine** logic (Outputs depend only on current state).
    * **State Encoding:** 6 Active states (S0 to S5) to manage "Wait", "Move", and "Skip" timing.
* **Datapath:**
    * **ALU:** Handles incrementing/decrementing logic based on `invert` and `a0` signals.
    * **Register:** Stores the current gate value (4-bit).

## Logic Equations
The Finite State Machine is driven by the following optimized Boolean logic:

* **Move Control (a0):** Q2 + Q1
* **Direction Control (invert):** Q0
* **Door Control (green):** ~Q2 * ~Q1
* **Next State Logic:** Derived via 5-variable Karnaugh Maps.

## Author
**Prince Dahiya**
*Computer Science, Arizona State University*
*EEE 120 - Digital Design Fundamentals*
