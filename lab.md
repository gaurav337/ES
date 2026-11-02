# MNNIT Allahabad — CSN 15402(P): Embedded Systems Lab
## Complete Lab Exam, Practical, Viva & Quiz Survival Master Manual
**Department of Computer Science & Engineering | B.Tech 5th Semester**

---

# MODULE 0: The Complete Beginner's Practical Guide
*(Never used Proteus, Keil, or MCU 8051 IDE? Read this first — Step-by-Step zero-to-hero guide)*

---

## 1. Keil uVision 5 — From Code to HEX File
Keil uVision is the industry-standard IDE used to write, compile, and generate `.hex` machine files for the 8051 microcontroller (`AT89C51`). Proteus cannot run your `.c` or `.asm` code directly; it requires the compiled `.hex` file.

### Step-by-Step Keil Workflow:
1. **Create a New Project**:
   - Open Keil uVision.
   - Click `Project` -> `New uVision Project...`.
   - Choose a directory (e.g., `D:\LabExam\`), name your project (e.g., `LabTest`), and click **Save**.
2. **Select the Device**:
   - A dialog titled *Select Device for Target* will pop up.
   - In the search bar, type `AT89C51` (under Microchip / Atmel).
   - Select `AT89C51` and click **OK**.
   - Keil will ask: *"Copy STARTUP.A51 to Project Folder and Add File to Project?"* -> Click **Yes** (or No if writing pure assembly starting at 0000H; for C programs, always click **Yes**).
3. **CRITICAL STEP: Enable HEX File Generation**:
   - By default, Keil compiles code but **DOES NOT** create a `.hex` file!
   - Right-click on `Target 1` in the Project Workspace tree on the left, and select **Options for Target 'Target 1'...** (or press `Alt + F7`).
   - Go to the **Target** tab:
     - Set **Xtal (MHz)** to `11.0592` (for serial communication questions) or `12.0` (for timer square wave questions as specified in problem statements).
     - Memory Model: `Small: variables in DATA`.
   - Go to the **Output** tab:
     - Check the box **[X] Create HEX File**. (If you forget this, you won't have a file to load into Proteus!).
   - Click **OK**.
4. **Create and Add Your Source File**:
   - Click `File` -> `New` (or `Ctrl + N`).
   - Click `File` -> `Save As...`.
   - Save the file as `main.c` (for Embedded C) or `main.asm` (for Assembly). **Ensure you manually type the extension `.c` or `.asm`!**
   - Expand `Target 1` in the left pane, right-click on `Source Group 1`, and choose **Add Existing Files to Group 'Source Group 1'...**.
   - Select your `main.c` or `main.asm` file and click **Add**, then **Close**.
5. **Write and Compile (Build)**:
   - Type or paste your code.
   - Click `Project` -> `Build Target` (or press `F7`).
   - Look at the bottom **Build Output** window:
     ```text
     creating hex file from ".\Objects\LabTest"...
     ".\Objects\LabTest" - 0 Error(s), 0 Warning(s).
     ```
   - Note the path where the `.hex` file is generated (usually in the project folder or `Objects` subfolder).

---

## 2. Proteus 8 Professional — Complete Circuit Simulation Guide
Proteus is a Virtual System Modeling (VSM) simulator where you build the 8051 circuit and test it with virtual instruments (Oscilloscope, Logic Analyzer, Virtual Terminal, LEDs, LCDs).

### Step-by-Step Proteus Setup:
1. **Launch Proteus**:
   - Open **Proteus 8 Professional**. Run as Administrator if required.
   - Click **New Project** on the Start Page.
   - Name your project (e.g., `MNNIT_Lab`), choose path, click **Next**.
2. **Schematic & Firmware Selection**:
   - In *Schematic Template*, select **DEFAULT** and click **Next**.
   - In *PCB Layout*, select **Do not create a PCB layout** and click **Next**.
   - In *Firmware*, you can select **No Firmware Project** (since we use Keil to build `.hex`), click **Next**, then **Finish**.
3. **Placing Components (The 'P' Key)**:
   - On the left toolbar, click the **Component Mode** icon (icon with op-amp/triangle).
   - Click the **P** button (Pick Devices) or press keyboard shortcut `P`.
   - Type keywords in the search bar, double-click each item to add it to your device list:
     - `AT89C51` — The 8051 Microcontroller IC.
     - `LED-BARGRAPH-RED` — 10-segment LED bar (used for 8-bit port display).
     - `LED-RED` — Individual red LED.
     - `RESPACK-8` — 8-resistor SIP network (pull-up / current limiting).
     - `RES` — Standard 2-pin resistor (set value to 330 ohm for LEDs, 10k for reset).
     - `DIPSW_8` or `DIPSW-8` — 8-position DIP switch for digital inputs.
     - `LM016L` — 16x2 Alphanumeric LCD display (HD44780 compatible).
     - `POT-HG` — Interactive Potentiometer (for LCD contrast control).
     - `CRYSTAL` — Quartz crystal oscillator.
     - `CAP` / `CAP-ELEC` — Capacitors (33pF ceramic, 10uF electrolytic).
4. **Power & Ground Terminals**:
   - On the left toolbar, click **Terminals Mode** icon (two horizontal bars with pins).
   - Select **POWER** (provides +5V VCC).
   - Select **GROUND** (provides 0V GND).
   - *Note on AT89C51 in Proteus:* The VCC (Pin 40) and GND (Pin 20) of AT89C51 are hidden and internally connected to VCC and GND by default.
   - **MANDATORY PIN CONNECTION:** Pin 31 is `EA/VPP` (External Access). For AT89C51 to execute code from its internal 4KB Flash ROM, **Pin 31 (EA) MUST BE CONNECTED TO POWER (+5V)**! If left floating or connected to GND, the 8051 will attempt to fetch code from external ROM and will execute nothing!
   - Pin 9 is `RST` (Reset). Connect to a 10uF capacitor to VCC and an 8.2k/10k resistor to GND with a pushbutton for manual reset (or connect directly to GND for simulation if reset button is not needed).
5. **Loading the HEX File into AT89C51**:
   - Double-click on the `AT89C51` chip symbol on the schematic.
   - An *Edit Component* window opens.
   - Look for **Program File**: click the small folder icon and browse to your compiled `.hex` file generated by Keil.
   - Set **Clock Frequency**: Change to `11.0592MHz` or `12MHz` (match your Keil target setting!).
   - Click **OK**.
6. **Virtual Instruments Mode**:
   - Click on the **Virtual Instruments** icon on the left toolbar (looks like an oscilloscope / meter).
   - Available tools:
     - **OSCILLOSCOPE**: Has 4 channels (A, B, C, D). Connect Channel A pin to your output pin (e.g., `P1.5` or `P2.0`). Run simulation, and the oscilloscope window opens. Adjust time base (`Time/Div`) to `0.1ms` or `0.2ms` and voltage base (`Volts/Div`) to `2V` or `5V` to see square waves clearly.
     - **VIRTUAL TERMINAL**: Used for Serial UART communication (Assignments 10 Q4/Q5, D2 Q4).
       - Connect `TXD` of AT89C51 (Pin 11 / P3.1) to `RXD` of Virtual Terminal.
       - Connect `RXD` of AT89C51 (Pin 10 / P3.0) to `TXD` of Virtual Terminal.
       - Double-click the Virtual Terminal in schematic to set Baud Rate (e.g., `9600` or `4800`), Data Bits (8), Parity (None), Stop Bits (1).
     - **SIGNAL GENERATOR**: Used for generating external pulse trains (e.g., Assignment 11 Q3 vehicle sensor on Pin 15 / P3.5 / T1).
7. **Running Simulation**:
   - Click the green **Play (Run)** button at the bottom-left corner of the Proteus window.
   - The circuit runs in real time!
   - To pause/stop, click the yellow pause or blue stop square button at the bottom left.
   - *If an instrument window closes:* Right-click during simulation -> select *Digital Oscilloscope* or *Virtual Terminal* from the popup menu, or go to top menu `Debug` -> click the instrument name.

---

## 3. MCU 8051 IDE Virtual Hardware Configuration
MCU 8051 IDE is used for Assignments 2 through 8. It features built-in Virtual Hardware panels accessible from the top menu `Virtual HW`.

### 1. LED Panel (Assignment 6):
- Go to `Virtual HW` -> `LED Panel`.
- Click `Settings` (gear icon). Check **Window Always on Top**.
- Set **Port**: Choose `Port 1` (or whichever port is specified in code). The same port must be selected for all 8 LEDs.
- Set **Bit Number**: Assign Bit 0 to Bit 7 to LEDs 0 through 7.
- **CRITICAL CONVENTION:** In MCU 8051 LED Panel, **logic 0 turns the LED ON** (Active-Low / Sink current), and **logic 1 turns the LED OFF**.
  - All ON: Send `00H`.
  - All OFF: Send `FFH`.

### 2. LED Display — 7-Segment (Assignment 7):
- Go to `Virtual HW` -> `LED Display`.
- Click `Settings`:
  - **Common Electrode**: Select **Common Anode** (CA) or **Common Cathode** (CC).
    - Common Anode: Bit value `0` turns ON the segment.
    - Common Cathode: Bit value `1` turns ON the segment.
- Turn ON the display switch on the panel.
- Assign the selected Port and individual bit numbers (Bit 0 to Bit 7) to segments A, B, C, D, E, F, G, DP.

### 3. Multiplexed 4-Digit 7-Segment Display (Assignment 8 Q1, Q2):
- Go to `Virtual HW` -> `Multiplexed LED Display`.
- Configuration:
  - **Digit Select Lines**: Assign Port 2 bits (e.g., P2.0 for Digit 0, P2.1 for Digit 1, P2.2 for Digit 2, P2.3 for Digit 3).
  - **Segment Lines**: Assign Port 1 bits (P1.0 to P1.7 for segments A through DP).
  - In Common Anode: Active low on digit select enables the digit, and active low on segment line turns ON the segment.

### 4. 8x8 LED Matrix (Assignment 8 Q3-Q6):
- Go to `Virtual HW` -> `LED Matrix`.
- Select Matrix Dimensions: `8 rows` (A to H) and `8 columns` (0 to 7).
- Configuration:
  - Assign Row Port (e.g., `Port 1`) for Rows A-H.
  - Assign Column Port (e.g., `Port 2`) for Columns 0-7.
  - **Active-Low Matrix Logic:** An LED at row $R$ and column $C$ turns ON when its row bit and column bit provide the required forward bias (in MCU 8051 active-low matrix: Row = 0 and Column = 0 or as configured in settings).

---

## 4. EdSim51 Simulator Guide (Assignment 1)
- EdSim51 is a graphical 8051 simulator.
- Open EdSim51 (`java -jar edsim51di.jar`).
- Paste your Assembly code into the code pane on the left.
- Click **Assm** (Assemble) button.
- Memory windows:
  - `Data` tab: View internal RAM (00H to 7FH).
  - Observe locations `20H`, `21H`, `30H`, `40H` as requested by Assignment 1.
- Click **Step** to execute line-by-line, or **Run** to execute at full speed.



# MODULE 1: All Laboratory Assignments (Step-by-Step Complete Solutions)

---

# ASSIGNMENT 1: EdSim51 Basics & Fundamental Operations

---

## Assignment 1 — Question 1
### Proper Question Title:
**Determination of Positive and Negative Numbers Count in an Array of 10 Numbers Stored in Internal RAM**

### Short Name to Remember:
`A1Q1_Pos_Neg_Counter` (Count +ve/-ve numbers from 40H; +ve -> 20H, -ve -> 21H)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV R0, #40H
    MOV R2, #10
    MOV 20H, #00H
    MOV 21H, #00H
LOOP:
    MOV A, @R0
    JB ACC.7, NEGATIVE
    INC 20H
    SJMP NEXT
NEGATIVE:
    INC 21H
NEXT:
    INC R0
    DJNZ R2, LOOP
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin address for reset vector
    MOV R0, #40H        ; Initialize R0 as pointer to starting RAM address 40H
    MOV R2, #10         ; Counter register R2 loaded with 10 (size of array)
    MOV 20H, #00H       ; Clear location 20H (Positive numbers counter = 0)
    MOV 21H, #00H       ; Clear location 21H (Negative numbers counter = 0)
LOOP:
    MOV A, @R0          ; Read byte from RAM address pointed by R0 into Accumulator
    JB ACC.7, NEGATIVE  ; If MSB (bit 7) is 1, number is negative in 2's complement
    INC 20H             ; Bit 7 is 0, so number is positive -> increment 20H
    SJMP NEXT           ; Skip the negative increment
NEGATIVE:
    INC 21H             ; Increment negative counter at location 21H
NEXT:
    INC R0              ; Point R0 to the next RAM location
    DJNZ R2, LOOP       ; Decrement loop counter R2; repeat if R2 != 0
    SJMP $              ; Terminate execution (halt in infinite loop)
    END                 ; End of assembly source code
```

### How It Worked & Registers Used:
- **Sign Bit Principle:** In signed 8-bit arithmetic (2's complement), the Most Significant Bit (MSB, `ACC.7`) indicates the sign:
  - If `ACC.7 = 0`, the number is positive (0 to +127).
  - If `ACC.7 = 1`, the number is negative (-128 to -1).
- **Registers Used:**
  - `R0`: Indirect internal RAM pointer pointing to current element.
  - `R2`: Loop counter initialized to decimal 10.
  - `ACC`: Accumulator holding the current byte; inspected via bit-addressable bit `ACC.7`.
  - `20H`, `21H`: Direct RAM locations tracking positive and negative counts.
- **Key Instruction:** `JB ACC.7, label` tests bit 7 of the accumulator and branches if high.

### What It Does:
Iterates sequentially through 10 numbers located at internal RAM addresses `40H` through `49H`. Evaluates the sign of each number and tallies the total positive numbers at RAM `20H` and negative numbers at RAM `21H`.

### Examiner Viva Questions & Potential Modifications:
1. **"How do you separate zero as a third category?"**
   - *Modification:* Add `JZ IS_ZERO` before checking `ACC.7`. Increment a zero counter at `22H`.
2. **"Can we use `RLC A` instead of `JB ACC.7`?"**
   - *Modification:* Yes, `RLC A` rotates MSB into Carry flag (`CY`), then use `JC NEGATIVE`.
3. **"What if the numbers are stored in external RAM?"**
   - *Modification:* Replace `MOV R0, #40H` with `MOV DPTR, #40H` and `MOV A, @R0` with `MOVX A, @DPTR`.

---

## Assignment 1 — Question 2
### Proper Question Title:
**Addition of a Series of 10 Data Bytes from Internal RAM Storing a 16-Bit Sum**

### Short Name to Remember:
`A1Q2_10Byte_16Bit_Sum` (Sum 10 bytes starting at 30H -> MSB at 40H, LSB at 41H)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV R0, #30H
    MOV R2, #10
    CLR A
    MOV R3, A
LOOP:
    ADD A, @R0
    JNC SKIP_CARRY
    INC R3
SKIP_CARRY:
    INC R0
    DJNZ R2, LOOP
    MOV 41H, A
    MOV 40H, R3
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Reset vector
    MOV R0, #30H        ; Pointer R0 initialized to starting data location 30H
    MOV R2, #10         ; Counter R2 loaded with array length 10
    CLR A               ; Clear Accumulator (stores lower 8 bits of sum)
    MOV R3, A           ; Clear Register R3 (stores upper 8 bits / carries)
LOOP:
    ADD A, @R0          ; Add byte at @R0 to Accumulator; sets Carry if overflow > 255
    JNC SKIP_CARRY      ; If CY == 0, jump over carry increment
    INC R3              ; If CY == 1, increment higher-byte register R3
SKIP_CARRY:
    INC R0              ; Move pointer to next data byte
    DJNZ R2, LOOP       ; Repeat loop until all 10 bytes are added
    MOV 41H, A          ; Store lower 8 bits of sum at RAM 41H
    MOV 40H, R3         ; Store higher 8 bits of sum at RAM 40H
    SJMP $              ; Infinite loop to halt program
    END
```

### How It Worked & Registers Used:
- **16-bit Overflow Handling:** Adding ten 8-bit numbers (max $10 	imes 255 = 2550 = 09F6H$) exceeds an 8-bit accumulator ($FFH = 255$).
- Whenever an addition results in a carry, `CY` is set to 1. `JNC` checks this; if carry occurs, `INC R3` increments the MSB.
- **Registers Used:**
  - `A`: Accumulator holding running lower byte of the sum.
  - `R3`: Register holding higher byte of sum (cumulative carries).
  - `R0`: Memory pointer pointing to `30H..39H`.
  - `R2`: Loop counter (10 iterations).
  - `40H` & `41H`: Destination addresses for MSB and LSB respectively.

### What It Does:
Sums ten unsigned 8-bit numbers located from internal RAM `30H` to `39H`. Generates a full 16-bit result, saving the MSB into `40H` and LSB into `41H`.

### Examiner Viva Questions & Potential Modifications:
1. **"What if the question asks to store result in little-endian format?"**
   - *Modification:* In little-endian, LSB is stored at the lower address: `MOV 40H, A` and `MOV 41H, R3`.
2. **"Can we use `ADDC` instead of `INC R3`?"**
   - *Modification:* Yes:
     ```assembly
     ADD A, @R0
     MOV R4, A
     MOV A, R3
     ADDC A, #00H
     MOV R3, A
     MOV A, R4
     ```
     `INC R3` with `JNC` is much faster and cleaner since `INC` does not affect flags.

---

## Assignment 1 — Question 3
### Proper Question Title:
**Block Transfer of 10 Data Bytes Between Internal RAM Locations**

### Short Name to Remember:
`A1Q3_Block_Transfer_20_to_30` (Transfer 10 bytes from 20H..29H to 30H..39H)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV R0, #20H
    MOV R1, #30H
    MOV R2, #10
COPY_LOOP:
    MOV A, @R0
    MOV @R1, A
    INC R0
    INC R1
    DJNZ R2, COPY_LOOP
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Program Origin
    MOV R0, #20H        ; Source pointer: R0 points to starting address 20H
    MOV R1, #30H        ; Destination pointer: R1 points to starting address 30H
    MOV R2, #10         ; Counter: R2 stores block size (10 bytes)
COPY_LOOP:
    MOV A, @R0          ; Read byte from source RAM location into Accumulator
    MOV @R1, A          ; Write byte from Accumulator to destination RAM location
    INC R0              ; Advance source pointer to next byte
    INC R1              ; Advance destination pointer to next byte
    DJNZ R2, COPY_LOOP  ; Decrement counter R2; branch if not zero
    SJMP $              ; Halt execution
    END
```

### How It Worked & Registers Used:
- In 8051, internal RAM cannot be directly copied to internal RAM using `MOV @R1, @R0`. Data must be buffered through the Accumulator `A`.
- **Registers Used:**
  - `R0`: Source pointer (pointing to internal RAM `20H`).
  - `R1`: Destination pointer (pointing to internal RAM `30H`).
  - `R2`: Loop counter (value 10).
  - `A`: Intermediate transfer buffer.

### What It Does:
Duplicates a continuous block of 10 bytes from source memory `20H..29H` to target memory `30H..39H` without data corruption.

### Examiner Viva Questions & Potential Modifications:
1. **"What if the source and destination overlap (e.g. 20H to 24H)?"**
   - *Answer:* Forward copying will overwrite source bytes before they are read. You must copy in reverse order (from end to start). See Assignment 2 Question 4.
2. **"Can we use R3 as a pointer?"**
   - *Answer:* No! Only `R0` and `R1` can act as indirect internal RAM pointers (`@R0`, `@R1`).

---

## Assignment 1 — Question 4
### Proper Question Title:
**64-Bit Addition of Two Numbers in Internal RAM Using Little-Endian Format**

### Short Name to Remember:
`A1Q4_64Bit_Little_Endian_Add` (Add 8-byte numbers from 20H and 30H -> 40H, carry at 48H)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV R0, #20H
    MOV R1, #30H
    MOV R2, #40H
    MOV R3, #08H
    CLR C
ADD_LOOP:
    MOV A, @R0
    ADDC A, @R1
    MOV @R2, A
    INC R0
    INC R1
    INC R2
    DJNZ R3, ADD_LOOP
    MOV A, #00H
    ADDC A, #00H
    MOV 48H, A
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV R0, #20H        ; R0 points to Augend (Number 1) LSB at 20H
    MOV R1, #30H        ; R1 points to Addend (Number 2) LSB at 30H
    MOV R2, #40H        ; R2 points to Sum destination LSB at 40H
    MOV R3, #08H        ; 64 bits = 8 bytes; loop counter = 8
    CLR C               ; Clear Carry flag prior to first addition
ADD_LOOP:
    MOV A, @R0          ; Load byte from Number 1 into Accumulator
    ADDC A, @R1         ; Add with carry the corresponding byte from Number 2
    MOV @R2, A          ; Store 8-bit sum byte into destination RAM
    INC R0              ; Increment pointer for Number 1 (points to next higher byte)
    INC R1              ; Increment pointer for Number 2
    INC R2              ; Increment pointer for Sum
    DJNZ R3, ADD_LOOP   ; Loop 8 times; preserves CY flag across iterations
    MOV A, #00H         ; Clear Accumulator
    ADDC A, #00H        ; Capture final 64-bit overflow carry (00H or 01H)
    MOV 48H, A          ; Store final carry at RAM address 48H
    SJMP $              ; Halt
    END
```

### How It Worked & Registers Used:
- **Little-Endian Architecture:** The least significant byte (LSB) is stored at the lowest memory address (`20H`, `30H`, `40H`), and the most significant byte (MSB) is at the highest address (`27H`, `37H`, `47H`).
- `ADDC A, @R1` adds the byte plus whatever carry `CY` was generated from the preceding byte addition.
- Crucially, `INC` instructions (`INC R0`, `INC R1`, `INC R2`) and `DJNZ R3` do **NOT** affect the carry flag (`CY`) in 8051! This allows the carry to propagate seamlessly across all 8 loop iterations.
- **Registers Used:**
  - `R0`: Pointer to Augend (20H to 27H).
  - `R1`: Pointer to Addend (30H to 37H).
  - `R2`: Pointer to Result (40H to 47H).
  - `R3`: Counter (8 bytes).
  - `CY`: Arithmetic carry bit.
  - `48H`: Final overflow carry location.

### What It Does:
Adds two 64-bit hexadecimal numbers:
`0145 3BC0 56C7 05FE H` and `0239 F2A7 B13A 0679 H`.
Produces the 64-bit sum starting at address `40H`, and sets `48H` to `01H` if a 65th carry bit occurs, or `00H` otherwise.

### Examiner Viva Questions & Potential Modifications:
1. **"Why does `INC R0` not corrupt the Carry flag?"**
   - *Answer:* In 8051 architecture, `INC` and `DEC` affect NO flags in PSW (unlike 8085/x86 where ZF is modified). Thus, the Carry bit remains preserved for the next `ADDC`.
2. **"How would you do 64-bit subtraction?"**
   - *Modification:* Use `CLR C` initially, then replace `ADDC A, @R1` with `SUBB A, @R1`.

---

# ASSIGNMENT 2: Memory Interfacing & Block Operations (MCU 8051)

---

## Assignment 2 — Question 1
### Proper Question Title:
**Block Transfer of 10 Data Bytes from Internal RAM (20H) to External RAM (2000H)**

### Short Name to Remember:
`A2Q1_Int_to_Ext_RAM_Transfer` (10 bytes from 20H -> 2000H using `MOVX`)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV R0, #20H
    MOV DPTR, #2000H
    MOV R2, #10
TRANS_LOOP:
    MOV A, @R0
    MOVX @DPTR, A
    INC R0
    INC DPTR
    DJNZ R2, TRANS_LOOP
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV R0, #20H        ; R0 points to internal RAM starting at 20H
    MOV DPTR, #2000H    ; DPTR points to external RAM starting at 2000H
    MOV R2, #10         ; Counter initialized to 10
TRANS_LOOP:
    MOV A, @R0          ; Read byte from internal RAM location @R0
    MOVX @DPTR, A       ; Write byte to external RAM address @DPTR using /WR strobe
    INC R0              ; Increment internal RAM pointer
    INC DPTR            ; Increment 16-bit external data pointer
    DJNZ R2, TRANS_LOOP ; Loop until 10 bytes transferred
    SJMP $              ; Stop execution
    END
```

### How It Worked & Registers Used:
- External RAM requires the 16-bit Data Pointer (`DPTR`) and the `MOVX` (Move External) instruction.
- When `MOVX @DPTR, A` executes, the 8051 generates address multiplexing on Port 0 and Port 2, asserting the active-low `\WR` strobe pin (P3.6).
- **Registers Used:** `R0` (internal pointer), `DPTR` (external pointer), `R2` (counter), `A` (accumulator).

### What It Does:
Reads 10 continuous bytes from internal memory `20H..29H` and writes them into external RAM addresses `2000H..2009H`.

---

## Assignment 2 — Question 2
### Proper Question Title:
**Block Transfer of 10 Data Bytes from External RAM (2000H) to Internal RAM (20H)**

### Short Name to Remember:
`A2Q2_Ext_to_Int_RAM_Transfer` (10 bytes from 2000H -> 20H using `MOVX A, @DPTR`)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV DPTR, #2000H
    MOV R0, #20H
    MOV R2, #10
FETCH_LOOP:
    MOVX A, @DPTR
    MOV @R0, A
    INC DPTR
    INC R0
    DJNZ R2, FETCH_LOOP
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV DPTR, #2000H    ; DPTR points to source external RAM at 2000H
    MOV R0, #20H        ; R0 points to target internal RAM at 20H
    MOV R2, #10         ; Counter = 10 bytes
FETCH_LOOP:
    MOVX A, @DPTR       ; Read byte from external RAM into Accumulator using /RD strobe
    MOV @R0, A          ; Write byte into internal RAM at address pointed by R0
    INC DPTR            ; Increment external RAM pointer
    INC R0              ; Increment internal RAM pointer
    DJNZ R2, FETCH_LOOP ; Decrement counter and loop if not zero
    SJMP $              ; Halt
    END
```

### How It Worked & Registers Used:
- `MOVX A, @DPTR` asserts the external read strobe `\RD` (Pin P3.7) to fetch data into `A`.
- Then `MOV @R0, A` writes into the fast internal data RAM.

---

## Assignment 2 — Question 3
### Proper Question Title:
**Block Transfer of 10 Data Bytes Between Two External RAM Locations (1934H to 3048H)**

### Short Name to Remember:
`A2Q3_Ext_to_Ext_RAM_Transfer` (10 bytes from ext 1934H to ext 3048H)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV R2, #10
    MOV R3, #19H
    MOV R4, #34H
    MOV R5, #30H
    MOV R6, #48H
EXT_LOOP:
    MOV DPH, R3
    MOV DPL, R4
    MOVX A, @DPTR
    INC DPTR
    MOV R3, DPH
    MOV R4, DPL
    MOV B, A
    MOV DPH, R5
    MOV DPL, R6
    MOV A, B
    MOVX @DPTR, A
    INC DPTR
    MOV R5, DPH
    MOV R6, DPL
    DJNZ R2, EXT_LOOP
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV R2, #10         ; 10 bytes to transfer
    MOV R3, #19H        ; Source DPTR High byte (19H)
    MOV R4, #34H        ; Source DPTR Low byte (34H)
    MOV R5, #30H        ; Destination DPTR High byte (30H)
    MOV R6, #48H        ; Destination DPTR Low byte (48H)
EXT_LOOP:
    ;--- Read from Source (1934H onwards) ---
    MOV DPH, R3         ; Load Source DPH
    MOV DPL, R4         ; Load Source DPL
    MOVX A, @DPTR       ; Read data byte from source external RAM
    INC DPTR            ; Increment source pointer
    MOV R3, DPH         ; Save updated Source DPH back to R3
    MOV R4, DPL         ; Save updated Source DPL back to R4
    MOV B, A            ; Buffer data byte into B register
    ;--- Write to Destination (3048H onwards) ---
    MOV DPH, R5         ; Load Destination DPH
    MOV DPL, R6         ; Load Destination DPL
    MOV A, B            ; Restore data byte into Accumulator
    MOVX @DPTR, A       ; Write data byte to destination external RAM
    INC DPTR            ; Increment destination pointer
    MOV R5, DPH         ; Save updated Destination DPH back to R5
    MOV R6, DPL         ; Save updated Destination DPL back to R6
    DJNZ R2, EXT_LOOP   ; Loop until all 10 bytes transferred
    SJMP $              ; Program Halt
    END
```

### How It Worked & Registers Used:
- **The Single DPTR Bottleneck:** The standard 8051 has only ONE 16-bit Data Pointer register (`DPTR`).
- To transfer between two external memory locations, we use register pairs: `R3:R4` to save the source address and `R5:R6` to save the destination address.
- We alternate between swapping `DPH:DPL` and buffering the fetched byte in Register `B`.

### What It Does:
Copies 10 bytes from external RAM addresses `1934H..193DH` to external RAM addresses `3048H..3051H`.

---

## Assignment 2 — Question 4
### Proper Question Title:
**Overlapping Block Transfer of 10 Data Bytes from 30H to 34H in Internal RAM**

### Short Name to Remember:
`A2Q4_Overlapping_Block_Transfer` (10 bytes: 30H -> 34H, backward copy to prevent overwrite)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV R0, #39H
    MOV R1, #3DH
    MOV R2, #10
BACKWARD_COPY:
    MOV A, @R0
    MOV @R1, A
    DEC R0
    DEC R1
    DJNZ R2, BACKWARD_COPY
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    ; Source range: 30H to 39H (10 bytes)
    ; Destination range: 34H to 3DH (10 bytes)
    ; Overlap region: 34H to 39H!
    ; If we copy forward (30H -> 34H), original data at 34H will be destroyed before being read.
    ; Therefore, we MUST copy from the LAST byte downwards (backward copy):
    MOV R0, #39H        ; Source ends at 30H + 9 = 39H
    MOV R1, #3DH        ; Destination ends at 34H + 9 = 3DH
    MOV R2, #10         ; Counter = 10 bytes
BACKWARD_COPY:
    MOV A, @R0          ; Read byte from end of source block
    MOV @R1, A          ; Write byte to end of destination block
    DEC R0              ; Decrement source pointer
    DEC R1              ; Decrement destination pointer
    DJNZ R2, BACKWARD_COPY ; Repeat until all 10 bytes moved
    SJMP $              ; Halt
    END
```

### How It Worked & Registers Used:
- **Memory Collision Hazard:** Source is `30H..39H` and destination is `34H..3DH`. The block `34H..39H` overlaps!
- If forward copying is attempted, copying byte 0 into `34H` destroys byte 4 before it can be copied.
- By starting at the highest addresses (`39H` -> `3DH`, `38H` -> `3CH`, ..., `30H` -> `34H`), each byte is copied into unoccupied or already-relocated territory.

---

## Assignment 2 — Question 5
### Proper Question Title:
**Counting Positive and Negative Numbers in a Look-up Table Stored in Internal ROM**

### Short Name to Remember:
`A2Q5_ROM_Lookup_Pos_Neg_Count` (Count +ve/-ve in 10-byte ROM table using `MOVC`)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV DPTR, #TABLE
    MOV R2, #10
    MOV R3, #00H
    MOV R4, #00H
LOOKUP_LOOP:
    CLR A
    MOVC A, @A+DPTR
    JB ACC.7, IS_NEG
    INC R3
    SJMP CONT
IS_NEG:
    INC R4
CONT:
    INC DPTR
    DJNZ R2, LOOKUP_LOOP
    MOV 30H, R3
    MOV 31H, R4
    SJMP $

    ORG 0200H
TABLE:
    DB 12H, 0F5H, 34H, 88H, 0E1H, 7FH, 0AH, 0FEH, 55H, 90H
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV DPTR, #TABLE    ; Load DPTR with base address of ROM lookup table
    MOV R2, #10         ; Counter = 10 elements
    MOV R3, #00H        ; Clear R3 (Positive count)
    MOV R4, #00H        ; Clear R4 (Negative count)
LOOKUP_LOOP:
    CLR A               ; Clear A so index offset = 0
    MOVC A, @A+DPTR     ; Read byte from Program ROM at address (A + DPTR)
    JB ACC.7, IS_NEG    ; If bit 7 is set, number is negative
    INC R3              ; If bit 7 is 0, increment positive count
    SJMP CONT           ; Jump over negative count
IS_NEG:
    INC R4              ; Increment negative count
CONT:
    INC DPTR            ; Advance DPTR to next entry in ROM table
    DJNZ R2, LOOKUP_LOOP; Repeat for all 10 entries
    MOV 30H, R3         ; Store final positive count at RAM 30H
    MOV 31H, R4         ; Store final negative count at RAM 31H
    SJMP $              ; Halt

    ORG 0200H           ; Store table in program memory
TABLE:
    DB 12H, 0F5H, 34H, 88H, 0E1H, 7FH, 0AH, 0FEH, 55H, 90H
    END
```

### How It Worked & Registers Used:
- ROM lookup requires `MOVC A, @A+DPTR`.
- `CLR A` ensures the accumulator is `00H` before indexing `DPTR`.
- `INC DPTR` advances through the ROM array.
- Final counts are deposited into internal RAM locations `30H` and `31H`.

---

## Assignment 2 — Question 6
### Proper Question Title:
**Palindrome Verification of an Unknown Length String (FFH if Palindrome, 00H otherwise)**

### Short Name to Remember:
`A2Q6_String_Palindrome_Check` (Check if string is palindrome; A=FFH if yes, 00H if no)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV R0, #40H
FIND_LEN:
    MOV A, @R0
    JZ LEN_FOUND
    INC R0
    SJMP FIND_LEN
LEN_FOUND:
    DEC R0
    MOV R1, R0
    MOV R0, #40H
CHECK_LOOP:
    MOV A, R0
    CLR C
    SUBB A, R1
    JNC IS_PALIN
    MOV A, @R0
    MOV B, A
    MOV A, @R1
    CJNE A, B, NOT_PALIN
    INC R0
    DEC R1
    SJMP CHECK_LOOP
IS_PALIN:
    MOV A, #0FFH
    SJMP FINISH
NOT_PALIN:
    MOV A, #00H
FINISH:
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    ; Assume null-terminated string (ends with 00H) starts at RAM 40H
    MOV R0, #40H        ; Start pointer R0 at string start
FIND_LEN:
    MOV A, @R0          ; Read character
    JZ LEN_FOUND        ; If null terminator (00H) found, stop
    INC R0              ; Advance to next character
    SJMP FIND_LEN
LEN_FOUND:
    DEC R0              ; R0 was pointing to 00H; back up to last character
    MOV R1, R0          ; R1 now points to end of string (tail pointer)
    MOV R0, #40H        ; R0 reset to start of string (head pointer)
CHECK_LOOP:
    MOV A, R0           ; Compare head pointer (R0) and tail pointer (R1)
    CLR C
    SUBB A, R1          ; If R0 >= R1, all characters matched!
    JNC IS_PALIN        ; All matching -> string is a palindrome
    MOV A, @R0          ; Read head character
    MOV B, A            ; Store in register B
    MOV A, @R1          ; Read tail character
    CJNE A, B, NOT_PALIN; Compare head and tail: if not equal, not palindrome!
    INC R0              ; Advance head pointer forward
    DEC R1              ; Retreat tail pointer backward
    SJMP CHECK_LOOP     ; Repeat check for inner characters
IS_PALIN:
    MOV A, #0FFH        ; Palindrome verified: set Accumulator to FFH
    SJMP FINISH
NOT_PALIN:
    MOV A, #00H         ; Not a palindrome: set Accumulator to 00H
FINISH:
    SJMP $              ; Halt
    END
```

### How It Worked & Registers Used:
- Two-pointer technique: `R0` (head pointer) and `R1` (tail pointer).
- Traverses inward, comparing `@R0` with `@R1` via `CJNE`.
- If pointers cross without mismatch, `A = FFH`. On first mismatch, `A = 00H`.

---

# ASSIGNMENT 3: 16-Bit Arithmetic, Division & Code Conversion List Files

---

## Assignment 3 — Question 1
### Proper Question Title:
**16-Bit Unsigned Multiplication (FFFFH x FFFFH) Generating 32-Bit Product**

### Short Name to Remember:
`A3Q1_16Bit_Multiplication` (FFFFH x FFFFH -> 32-bit product in 50H, 51H, 52H, 53H)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV 30H, #0FFH
    MOV 31H, #0FFH
    MOV 40H, #0FFH
    MOV 41H, #0FFH

    MOV A, 31H
    MOV B, 41H
    MUL AB
    MOV 53H, A
    MOV 52H, B

    MOV A, 31H
    MOV B, 40H
    MUL AB
    ADD A, 52H
    MOV 52H, A
    MOV A, B
    ADDC A, #00H
    MOV 51H, A
    MOV 50H, #00H

    MOV A, 30H
    MOV B, 41H
    MUL AB
    ADD A, 52H
    MOV 52H, A
    MOV A, B
    ADDC A, 51H
    MOV 51H, A
    JNC SKIP1
    INC 50H
SKIP1:

    MOV A, 30H
    MOV B, 40H
    MUL AB
    ADD A, 51H
    MOV 51H, A
    MOV A, B
    ADDC A, 50H
    MOV 50H, A

    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    ; Input setup: First 16-bit number = FFFFH (30H=FFH [High], 31H=FFH [Low])
    ; Second 16-bit number = FFFFH (40H=FFH [High], 41H=FFH [Low])
    ; Let N1 = (H1:L1) and N2 = (H2:L2).
    ; Product = (L1 * L2) + (L1 * H2 << 8) + (H1 * L2 << 8) + (H1 * H2 << 16)
    MOV 30H, #0FFH      ; High byte of N1
    MOV 31H, #0FFH      ; Low byte of N1
    MOV 40H, #0FFH      ; High byte of N2
    MOV 41H, #0FFH      ; Low byte of N2

    ; Step 1: L1 * L2 -> Byte 3 (LSB at 53H) and Byte 2 (at 52H)
    MOV A, 31H          ; Load L1
    MOV B, 41H          ; Load L2
    MUL AB              ; A = low byte, B = high byte
    MOV 53H, A          ; Store Byte 0 (absolute LSB) at 53H
    MOV 52H, B          ; Store intermediate Byte 1 at 52H

    ; Step 2: L1 * H2 -> Add to Byte 2 and Byte 1
    MOV A, 31H          ; Load L1
    MOV B, 40H          ; Load H2
    MUL AB              ; Multiply
    ADD A, 52H          ; Add product low byte to previous Byte 1
    MOV 52H, A          ; Save updated Byte 1 at 52H
    MOV A, B            ; Product high byte
    ADDC A, #00H        ; Add carry from addition
    MOV 51H, A          ; Store intermediate Byte 2 at 51H
    MOV 50H, #00H       ; Initialize Byte 3 (MSB) to 00H

    ; Step 3: H1 * L2 -> Add to Byte 2 and Byte 1
    MOV A, 30H          ; Load H1
    MOV B, 41H          ; Load L2
    MUL AB              ; Multiply
    ADD A, 52H          ; Add product low byte to Byte 1 at 52H
    MOV 52H, A          ; Finalized Byte 1 (52H)
    MOV A, B            ; Product high byte
    ADDC A, 51H         ; Add product high byte + carry into Byte 2
    MOV 51H, A          ; Save updated Byte 2
    JNC SKIP1           ; If no carry into Byte 3, skip
    INC 50H             ; Propagate carry into Byte 3
SKIP1:

    ; Step 4: H1 * H2 -> Add to Byte 2 and Byte 3
    MOV A, 30H          ; Load H1
    MOV B, 40H          ; Load H2
    MUL AB              ; Multiply
    ADD A, 51H          ; Add low byte into Byte 2 at 51H
    MOV 51H, A          ; Finalized Byte 2 (51H)
    MOV A, B            ; High byte of product
    ADDC A, 50H         ; Add into Byte 3 + carry
    MOV 50H, A          ; Finalized Byte 3 (MSB at 50H)

    SJMP $              ; Result FFFE 0001 H stored in 50H..53H
    END
```

### How It Worked & Registers Used:
- Math Verification: $65535 	imes 65535 = 4,294,836,225 = 	ext{FFFE0001H}$.
  - Location `50H` = `FFH` (MSB)
  - Location `51H` = `FEH`
  - Location `52H` = `00H`
  - Location `53H` = `01H` (LSB)
- Four 8-bit `MUL AB` partial products combined with `ADD`/`ADDC`.

---

## Assignment 3 — Question 2
### Proper Question Title:
**16-Bit Unsigned Division (FFFFH / 000FH) Generating 16-Bit Quotient and Remainder**

### Short Name to Remember:
`A3Q2_16Bit_Division` (Divide 16-bit by 16-bit; Q in 50H:51H, Rem in 52H:53H)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV 30H, #0FFH
    MOV 31H, #0FFH
    MOV 40H, #00H
    MOV 41H, #0FH

    MOV 50H, 30H
    MOV 51H, 31H
    MOV 52H, #00H
    MOV 53H, #00H
    MOV R7, #16

DIV_LOOP:
    MOV A, 51H
    RLC A
    MOV 51H, A
    MOV A, 50H
    RLC A
    MOV 50H, A
    MOV A, 53H
    RLC A
    MOV 53H, A
    MOV A, 52H
    RLC A
    MOV 52H, A

    CLR C
    MOV A, 53H
    SUBB A, 41H
    MOV R2, A
    MOV A, 52H
    SUBB A, 40H
    JC NO_SUB

    MOV 52H, A
    MOV 53H, R2
    INC 51H

NO_SUB:
    DJNZ R7, DIV_LOOP
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    ; Dividend in 30H (High) and 31H (Low) = FFFFH
    ; Divisor in 40H (High) and 41H (Low) = 000FH
    MOV 30H, #0FFH
    MOV 31H, #0FFH
    MOV 40H, #00H
    MOV 41H, #0FH

    ; Initialize Quotient (50H:51H) with Dividend
    MOV 50H, 30H
    MOV 51H, 31H
    ; Initialize Remainder (52H:53H) with 0000H
    MOV 52H, #00H
    MOV 53H, #00H
    MOV R7, #16         ; 16-bit division loop counter

DIV_LOOP:
    ; Shift left Remainder:Quotient 32-bit register chain through Carry
    MOV A, 51H
    RLC A
    MOV 51H, A
    MOV A, 50H
    RLC A
    MOV 50H, A
    MOV A, 53H
    RLC A
    MOV 53H, A
    MOV A, 52H
    RLC A
    MOV 52H, A

    ; Trial subtraction: Remainder (52H:53H) - Divisor (40H:41H)
    CLR C
    MOV A, 53H
    SUBB A, 41H         ; Subtract low bytes
    MOV R2, A           ; Temporarily save low difference
    MOV A, 52H
    SUBB A, 40H         ; Subtract high bytes with borrow
    JC NO_SUB           ; If Borrow (CY=1), Remainder < Divisor -> don't subtract

    ; If Remainder >= Divisor, commit subtraction and set quotient LSB = 1
    MOV 52H, A          ; Update Remainder high byte
    MOV 53H, R2         ; Update Remainder low byte
    INC 51H             ; Set LSB of quotient to 1

NO_SUB:
    DJNZ R7, DIV_LOOP   ; Repeat for all 16 bits
    SJMP $              ; Result: Quotient in 50H:51H (1111H), Remainder in 52H:53H (0000H)
    END
```

### How It Worked & Registers Used:
- Standard **Restoring Division Algorithm** (16 shifts and trial subtractions).
- Quotient generated in `50H` (High) and `51H` (Low).
- Remainder stored in `52H` (High) and `53H` (Low).
- For $	ext{FFFFH} / 000	ext{FH} = 65535 / 15 = 4369$ remainder 0 ($	ext{1111H}$ remainder $0$).

---

## Assignment 3 — Question 3
### Proper Question Title:
**Packed BCD to ASCII Conversion on Ports P2/P3 with 0.5s Delay at XTAL = 22 MHz & Complete Assembler List File**

### Short Name to Remember:
`A3Q3_BCD_to_ASCII_List_File` (Convert 10 BCD numbers to ASCII on P2/P3; 0.5s delay at 22MHz)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
START:
    MOV R3, #10
    MOV DPTR, #0400H
BACK:
    CLR A
    MOVC A, @A+DPTR
    MOV R2, A
    ANL A, #0FH
    ORL A, #30H
    MOV P3, A
    MOV A, R2
    ANL A, #0F0H
    SWAP A
    ORL A, #30H
    MOV P2, A
    ACALL DELAY
    INC DPTR
    DJNZ R3, BACK
    SJMP START

    ORG 0300H
DELAY:
    MOV R6, #07H
HERE1:
    MOV R5, #0FFH
HERE2:
    MOV R4, #0FFH
HERE3:
    DJNZ R4, HERE3
    DJNZ R5, HERE2
    DJNZ R6, HERE1
    RET

    ORG 0400H
TABLE:
    DB 29H, 32H, 33H, 12H, 92H, 34H, 23H, 47H, 42H, 61H
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Reset vector
START:
    MOV R3, #10         ; Counter for 10 BCD numbers
    MOV DPTR, #0400H    ; DPTR points to lookup table at 0400H in ROM
BACK:
    CLR A               ; Clear index
    MOVC A, @A+DPTR     ; Fetch packed BCD byte from ROM
    MOV R2, A           ; Preserve original packed BCD in R2
    ;--- Unpack Lower BCD Digit -> Port P3 ---
    ANL A, #0FH         ; Mask off upper nibble (e.g. 29H -> 09H)
    ORL A, #30H         ; Convert BCD digit to ASCII (09H | 30H = 39H = '9')
    MOV P3, A           ; Output lower ASCII digit to Port P3
    ;--- Unpack Higher BCD Digit -> Port P2 ---
    MOV A, R2           ; Reload original packed BCD from R2
    ANL A, #0F0H        ; Mask off lower nibble (e.g. 29H -> 20H)
    SWAP A              ; Swap nibbles (20H -> 02H)
    ORL A, #30H         ; Convert to ASCII (02H | 30H = 32H = '2')
    MOV P2, A           ; Output higher ASCII digit to Port P2
    ACALL DELAY         ; Call 0.5-second software delay
    INC DPTR            ; Advance pointer to next packed BCD number
    DJNZ R3, BACK       ; Process next number
    SJMP START          ; Loop continuously from first number

    ORG 0300H           ; Delay subroutine placed at 0300H
DELAY:
    MOV R6, #07H        ; Outer loop count (7)
HERE1:
    MOV R5, #0FFH       ; Middle loop count (255)
HERE2:
    MOV R4, #0FFH       ; Inner loop count (255)
HERE3:
    DJNZ R4, HERE3      ; 2 cycles per inner iteration
    DJNZ R5, HERE2
    DJNZ R6, HERE1
    RET                 ; Return from delay

    ORG 0400H           ; Table stored at ROM 0400H
TABLE:
    DB 29H, 32H, 33H, 12H, 92H, 34H, 23H, 47H, 42H, 61H
    END
```

### Complete Filled Assembler List File Table:
| PC (Hex) | Opcode (Hex) | Line | Mnemonic | Operand | Machine Cycles |
|---|---|---|---|---|---|
| **0000** | **7B 0A** | 01 | ORG | 0000H | - |
| **0000** | **7B 0A** | 02 | START: MOV | R3, #10 | 1 |
| **0002** | **90 04 00** | 03 | MOV | DPTR, #0400H | 2 |
| **0005** | **E4** | 04 | BACK: CLR | A | 1 |
| **0006** | **93** | 05 | MOVC | A, @A+DPTR | 2 |
| **0007** | **FA** | 06 | MOV | R2, A | 1 |
| **0008** | **54 0F** | 07 | ANL | A, #0FH | 1 |
| **000A** | **44 30** | 08 | ORL | A, #30H | 1 |
| **000C** | **F5 B0** | 09 | MOV | P3, A | 2 |
| **000E** | **EA** | 10 | MOV | A, R2 | 1 |
| **000F** | **54 F0** | 11 | ANL | A, #0F0H | 1 |
| **0011** | **C4** | 12 | SWAP | A | 1 |
| **0012** | **44 30** | 13 | ORL | A, #30H | 1 |
| **0014** | **F5 A0** | 14 | MOV | P2, A | 2 |
| **0016** | **71 00** | 15 | ACALL | DELAY | 2 |
| **0018** | **A3** | 16 | INC | DPTR | 2 |
| **0019** | **DB EA** | 17 | DJNZ | R3, BACK | 2 |
| **001B** | **80 E3** | 18 | SJMP | START | 2 |
| **0300** | - | 19 | ORG | 0300H | - |
| **0300** | **7E 07** | 20 | DELAY: MOV | R6, #07H | 1 |
| **0302** | **7D FF** | 21 | HERE1: MOV | R5, #0FFH | 1 |
| **0304** | **7C FF** | 22 | HERE2: MOV | R4, #0FFH | 1 |
| **0306** | **DC FE** | 23 | HERE3: DJNZ | R4, HERE3 | 2 |
| **0308** | **DD FA** | 24 | DJNZ | R5, HERE2 | 2 |
| **030A** | **DE F6** | 25 | DJNZ | R6, HERE1 | 2 |
| **030C** | **22** | 26 | RET | | 2 |
| **0400** | - | 27 | ORG | 0400H | - |
| **0400** | **29 32 33 12...**| 28 | DB | 29H, 32H... | - |
| - | - | 29 | END | | - |

### Delay Math Calculation at XTAL = 22 MHz:
- Oscillator period: $T_{\text{osc}} = 1 / 22\text{ MHz}$.
- Machine cycle: $12 \times (1 / 22\text{ MHz}) = 0.54545\ \mu\text{s}$.
- Required delay: $0.5\text{ s} = 500,000\ \mu\text{s}$.
- Total machine cycles needed: $500,000 / 0.54545 \approx 916,667$ cycles.
- Triple loop formula: $2 \times R6 \times R5 \times R4$ cycles.
  $2 \times 7 \times 255 \times 255 \approx 910,350$ cycles $\approx 0.496\text{ s} \approx 0.5\text{ s}$.

---

# ASSIGNMENT 4: Delays, Factorials, Fibonacci & Mixed RAM Arithmetic

---

## Assignment 4 — Question 1
### Proper Question Title:
**Software Delay Using Nested Loops (R1=200, R2=250) and Exact Execution Time Calculation at 11.0592 MHz**

### Short Name to Remember:
`A4Q1_Nested_Delay_Calculation` (R1=200, R2=250; Cycle count & time at 11.0592 MHz)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
DELAY:
    MOV R1, #200
OUTER:
    MOV R2, #250
INNER:
    DJNZ R2, INNER
    DJNZ R1, OUTER
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
DELAY:
    MOV R1, #200        ; [1 Cycle] Outer loop counter initialized to 200
OUTER:
    MOV R2, #250        ; [1 Cycle] Inner loop counter initialized to 250 (runs 200 times)
INNER:
    DJNZ R2, INNER      ; [2 Cycles] Decrement R2, branch if not 0 (runs 250 * 200 times)
    DJNZ R1, OUTER      ; [2 Cycles] Decrement R1, branch to OUTER if not 0 (runs 200 times)
    SJMP $              ; Stop
    END
```

### Detailed Instruction & Execution Time Calculation:
1. **Clock & Machine Cycle Timing:**
   - $\text{XTAL} = 11.0592\text{ MHz}$.
   - Machine cycle time:
     $$T_{\text{mc}} = \frac{12}{11.0592\text{ MHz}} = 1.0850694\ \mu\text{s}$$
2. **Cycle Count Breakdown:**
   - `MOV R1, #200` executes **1 time** = $1 \times 1 = 1$ cycle.
   - `MOV R2, #250` executes **200 times** = $200 \times 1 = 200$ cycles.
   - `DJNZ R2, INNER` executes **$200 \times 250 = 50,000$ times** = $50,000 \times 2 = 100,000$ cycles.
   - `DJNZ R1, OUTER` executes **200 times** = $200 \times 2 = 400$ cycles.
3. **Total Machine Cycles:**
   $$\text{Total Cycles} = 1 + 200 + 100,000 + 400 = 100,601\text{ machine cycles}$$
4. **Total Execution Time:**
   $$T = 100,601 \times 1.0850694\ \mu\text{s} \approx 109,159\ \mu\text{s} = 109.16\text{ ms} \approx 0.11\text{ seconds}$$

---

## Assignment 4 — Question 2
### Proper Question Title:
**Factorial Calculation of N (N <= 6) Storing 16-Bit Result in R6:R5**

### Short Name to Remember:
`A4Q2_Factorial_N` (Factorial of N <= 6; Result in R6:R5)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV R0, #05H
    MOV A, #01H
    MOV B, #00H
    MOV R2, #01H
FACT_LOOP:
    MOV B, R2
    MUL AB
    INC R2
    MOV R3, A
    MOV A, R2
    CLR C
    SUBB A, R0
    JC CONTINUE
    JZ CONTINUE
    SJMP STORE_RESULT
CONTINUE:
    MOV A, R3
    SJMP FACT_LOOP
STORE_RESULT:
    MOV R5, R3
    MOV R6, #00H
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV R0, #05H        ; Input number N (e.g. N = 5; 5! = 120 = 0078H)
    MOV A, #01H         ; Accumulator initialized to 1 (multiplicative identity)
    MOV R2, #02H        ; Multiplier starting at 2
FACT_LOOP:
    MOV B, R2           ; Load multiplier into register B
    MUL AB              ; A = A * B (For N <= 6, max is 6! = 720, fitting in 16 bits)
    ; If product exceeds 8 bits, B holds higher byte
    MOV R5, A           ; Lower byte of result in R5
    MOV R6, B           ; Higher byte of result in R6
    INC R2              ; Increment multiplier
    MOV A, R2           ; Check if multiplier > N
    CLR C
    SUBB A, R0
    JC CONTINUE         ; If R2 < N, continue multiplying
    JZ CONTINUE         ; If R2 == N, continue multiplying
    SJMP DONE           ; If R2 > N, computation is complete!
CONTINUE:
    MOV A, R5           ; Reload current factorial value
    SJMP FACT_LOOP
DONE:
    SJMP $              ; Result stored in R6:R5 (For N=5: R6=00H, R5=78H; N=6: R6=02H, R5=D0H)
    END
```

---

## Assignment 4 — Question 3
### Proper Question Title:
**Fibonacci Sequence Generation for First N Terms Stored Starting from Internal RAM 20H**

### Short Name to Remember:
`A4Q3_Fibonacci_Sequence` (First N Fibonacci numbers in 20H, 21H, 22H...)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV R0, #20H
    MOV R2, #08H
    MOV A, #00H
    MOV @R0, A
    INC R0
    DEC R2
    MOV A, #01H
    MOV @R0, A
    INC R0
    DEC R2
FIB_LOOP:
    DEC R0
    MOV A, @R0
    DEC R0
    ADD A, @R0
    INC R0
    INC R0
    MOV @R0, A
    INC R0
    DJNZ R2, FIB_LOOP
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV R0, #20H        ; Pointer R0 initialized to starting address 20H
    MOV R2, #08H        ; Generate first N = 8 terms
    ; First term F(0) = 0
    MOV A, #00H
    MOV @R0, A          ; Store 00H at 20H
    INC R0              ; Point to 21H
    DEC R2              ; One term done
    ; Second term F(1) = 1
    MOV A, #01H
    MOV @R0, A          ; Store 01H at 21H
    INC R0              ; Point to 22H
    DEC R2              ; Second term done
FIB_LOOP:
    DEC R0              ; Point to previous term F(n-1)
    MOV A, @R0          ; Load F(n-1) into Accumulator
    DEC R0              ; Point to F(n-2)
    ADD A, @R0          ; A = F(n-1) + F(n-2)
    INC R0              ; Restore pointer
    INC R0              ; Point to current destination address
    MOV @R0, A          ; Store new Fibonacci term
    INC R0              ; Advance to next slot
    DJNZ R2, FIB_LOOP   ; Repeat for remaining terms
    SJMP $              ; Result: 00, 01, 01, 02, 03, 05, 08, 0D (H)
    END
```

---

## Assignment 4 — Question 4
### Proper Question Title:
**Addition of Two 8-Bit Numbers in External RAM (2050H and 2051H) Storing Sum at 2052H and Carry at 2053H**

### Short Name to Remember:
`A4Q4_Ext_RAM_8Bit_Add` (2050H + 2051H -> Sum at 2052H, Carry at 2053H)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV DPTR, #2050H
    MOVX A, @DPTR
    MOV R1, A
    INC DPTR
    MOVX A, @DPTR
    ADD A, R1
    MOV R2, #00H
    JNC NO_CY
    MOV R2, #01H
NO_CY:
    INC DPTR
    MOVX @DPTR, A
    INC DPTR
    MOV A, R2
    MOVX @DPTR, A
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV DPTR, #2050H    ; DPTR points to first number in external RAM
    MOVX A, @DPTR       ; Read first 8-bit number into A
    MOV R1, A           ; Buffer in R1
    INC DPTR            ; DPTR points to 2051H
    MOVX A, @DPTR       ; Read second 8-bit number into A
    ADD A, R1           ; Add both numbers; Carry flag set if sum > 255
    MOV R2, #00H        ; Clear carry flag holder in R2
    JNC NO_CY           ; If no carry, skip
    MOV R2, #01H        ; If carry occurred, set carry flag holder to 01H
NO_CY:
    INC DPTR            ; DPTR points to 2052H
    MOVX @DPTR, A       ; Store 8-bit sum at external RAM 2052H
    INC DPTR            ; DPTR points to 2053H
    MOV A, R2           ; Load carry status
    MOVX @DPTR, A       ; Store carry (00H or 01H) at external RAM 2053H
    SJMP $              ; Halt
    END
```

---

## Assignment 4 — Question 5
### Proper Question Title:
**Conditional Addition of Numbers from Internal RAM (30H) and External RAM (2000H)**

### Short Name to Remember:
`A4Q5_Conditional_Mixed_RAM_Add` (Add 30H & 2000H; No carry -> 31H, Carry -> Ext 2001H)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV A, 30H
    MOV R1, A
    MOV DPTR, #2000H
    MOVX A, @DPTR
    ADD A, R1
    JC CARRY_GEN
    MOV 31H, A
    SJMP DONE
CARRY_GEN:
    MOV DPTR, #2001H
    MOVX @DPTR, A
DONE:
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV A, 30H          ; Read first 8-bit number from internal RAM 30H
    MOV R1, A           ; Save into register R1
    MOV DPTR, #2000H    ; DPTR points to external RAM 2000H
    MOVX A, @DPTR       ; Read second 8-bit number from external RAM
    ADD A, R1           ; Add both numbers
    JC CARRY_GEN        ; If Carry generated (CY=1), branch to CARRY_GEN
    ; No carry: store sum in internal RAM location 31H
    MOV 31H, A
    SJMP DONE
CARRY_GEN:
    ; Carry generated: store sum in external RAM location 2001H
    MOV DPTR, #2001H    ; Point DPTR to external RAM 2001H
    MOVX @DPTR, A       ; Store sum into external RAM
DONE:
    SJMP $              ; Stop
    END
```

---

# ASSIGNMENT 5: String Processing, Sorting & Prime Factorization

---

## Assignment 5 — Question 1
### Proper Question Title:
**Counting Occurrences of Character 'G' in String "PROGRAMMINGDATA"**

### Short Name to Remember:
`A5Q1_Count_Char_G` (Count 'G' in "PROGRAMMINGDATA"; store count at 30H)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV DPTR, #STR
    MOV R2, #15
    MOV R3, #00H
CHECK_G:
    CLR A
    MOVC A, @A+DPTR
    CJNE A, #'G', SKIP_INC
    INC R3
SKIP_INC:
    INC DPTR
    DJNZ R2, CHECK_G
    MOV 30H, R3
    SJMP $

    ORG 0200H
STR:
    DB 'P','R','O','G','R','A','M','M','I','N','G','D','A','T','A'
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV DPTR, #STR      ; DPTR points to the string in ROM
    MOV R2, #15         ; String length = 15 characters
    MOV R3, #00H        ; Clear match counter in R3
CHECK_G:
    CLR A               ; Clear index
    MOVC A, @A+DPTR     ; Fetch character from ROM
    CJNE A, #'G', SKIP_INC ; Compare with ASCII 'G' (47H); if not equal, jump
    INC R3              ; If character == 'G', increment count
SKIP_INC:
    INC DPTR            ; Advance to next character
    DJNZ R2, CHECK_G    ; Loop through entire string
    MOV 30H, R3         ; Store final count at internal RAM 30H (Expect 02H)
    SJMP $              ; Stop
    ORG 0200H
STR:
    DB 'P','R','O','G','R','A','M','M','I','N','G','D','A','T','A'
    END
```

---

## Assignment 5 — Question 2
### Proper Question Title:
**Ascending Alphabetical Sorting of String "MICROCONTROLLER" Starting at RAM 35H**

### Short Name to Remember:
`A5Q2_Bubble_Sort_String` (Sort "MICROCONTROLLER" alphabetically at 35H)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV DPTR, #RAW_STR
    MOV R0, #35H
    MOV R2, #15
LOAD_STR:
    CLR A
    MOVC A, @A+DPTR
    MOV @R0, A
    INC DPTR
    INC R0
    DJNZ R2, LOAD_STR

    MOV R3, #14
OUTER_SORT:
    MOV A, R3
    MOV R4, A
    MOV R0, #35H
INNER_SORT:
    MOV A, @R0
    MOV B, A
    INC R0
    MOV A, @R0
    CLR C
    SUBB A, B
    JNC NO_SWAP
    MOV A, @R0
    DEC R0
    MOV @R0, A
    INC R0
    MOV @R0, B
NO_SWAP:
    DJNZ R4, INNER_SORT
    DJNZ R3, OUTER_SORT
    SJMP $

    ORG 0200H
RAW_STR:
    DB 'M','I','C','R','O','C','O','N','T','R','O','L','L','E','R'
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    ; First copy the string from ROM to RAM starting at 35H (length = 15)
    MOV DPTR, #RAW_STR
    MOV R0, #35H
    MOV R2, #15
LOAD_STR:
    CLR A
    MOVC A, @A+DPTR
    MOV @R0, A
    INC DPTR
    INC R0
    DJNZ R2, LOAD_STR

    ; Bubble Sort algorithm in ascending order (N-1 = 14 passes)
    MOV R3, #14         ; Outer loop counter = N - 1
OUTER_SORT:
    MOV A, R3
    MOV R4, A           ; Inner loop counter = remaining unsorted elements
    MOV R0, #35H        ; Reset pointer to start of string in RAM
INNER_SORT:
    MOV A, @R0          ; Read first element into A
    MOV B, A            ; Save in B
    INC R0              ; Point to adjacent element
    MOV A, @R0          ; Read adjacent element into A
    CLR C
    SUBB A, B           ; Compare: Second - First
    JNC NO_SWAP         ; If Second >= First, already in order -> skip swap
    ; Swap elements if Second < First
    MOV A, @R0          ; Load second element
    DEC R0              ; Point back to first slot
    MOV @R0, A          ; Put second element into first slot
    INC R0              ; Point to second slot
    MOV @R0, B          ; Put first element (from B) into second slot
NO_SWAP:
    DJNZ R4, INNER_SORT ; Inner loop
    DJNZ R3, OUTER_SORT ; Outer loop
    SJMP $              ; Result: "CCEILLMNOOORRRT" stored from 35H
    ORG 0200H
RAW_STR:
    DB 'M','I','C','R','O','C','O','N','T','R','O','L','L','E','R'
    END
```

---

## Assignment 5 — Question 3
### Proper Question Title:
**Prime Factorization of Hexadecimal Number 60H Stored Sequentially from RAM 40H**

### Short Name to Remember:
`A5Q3_Prime_Factors_60H` (Prime factors of 60H [96 dec] = 2,2,2,2,2,3 -> RAM 40H)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV R0, #40H
    MOV R1, #60H
    MOV R2, #02H
FACTOR_LOOP:
    MOV A, R1
    MOV B, R2
    DIV AB
    MOV A, B
    JNZ NEXT_DIVISOR
    MOV A, R2
    MOV @R0, A
    INC R0
    MOV A, R1
    MOV B, R2
    DIV AB
    MOV R1, A
    CJNE R1, #01H, FACTOR_LOOP
    SJMP DONE
NEXT_DIVISOR:
    INC R2
    SJMP FACTOR_LOOP
DONE:
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV R0, #40H        ; Pointer R0 stores prime factors starting at 40H
    MOV R1, #60H        ; Number to factor = 60H (Decimal 96)
    MOV R2, #02H        ; First candidate prime factor divisor = 2
FACTOR_LOOP:
    MOV A, R1           ; Load current number
    MOV B, R2           ; Load candidate divisor
    DIV AB              ; Divide: Quotient in A, Remainder in B
    MOV A, B            ; Check remainder
    JNZ NEXT_DIVISOR    ; If remainder != 0, candidate is not a factor -> try next
    ; Divisible! R2 is a prime factor
    MOV A, R2           ; Get factor
    MOV @R0, A          ; Store factor into RAM @R0
    INC R0              ; Advance factor storage pointer
    MOV A, R1
    MOV B, R2
    DIV AB
    MOV R1, A           ; Update number to quotient (R1 = R1 / R2)
    CJNE R1, #01H, FACTOR_LOOP ; Continue until number reduced to 1
    SJMP DONE
NEXT_DIVISOR:
    INC R2              ; Try next candidate divisor (2 -> 3 -> 4...)
    SJMP FACTOR_LOOP
DONE:
    SJMP $              ; Result at 40H..45H: 02H, 02H, 02H, 02H, 02H, 03H (2^5 * 3 = 96)
    END
```

---

## Assignment 5 — Question 4
### Proper Question Title:
**Prime Factorization of Hexadecimal Number 48H, Storing Factors at 30H, Sum at 40H, and Carry at 41H**

### Short Name to Remember:
`A5Q4_Prime_Factors_48H_Sum` (Factors of 48H [72 dec] -> 30H, Sum -> 40H, Carry -> 41H)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV R0, #30H
    MOV R1, #48H
    MOV R2, #02H
    CLR A
    MOV R5, A
    MOV R6, A
FACTOR_48:
    MOV A, R1
    MOV B, R2
    DIV AB
    MOV A, B
    JNZ NEXT_P
    MOV A, R2
    MOV @R0, A
    INC R0
    ADD A, R5
    MOV R5, A
    JNC NO_CY_P
    INC R6
NO_CY_P:
    MOV A, R1
    MOV B, R2
    DIV AB
    MOV R1, A
    CJNE R1, #01H, FACTOR_48
    SJMP STORE_SUM
NEXT_P:
    INC R2
    SJMP FACTOR_48
STORE_SUM:
    MOV 40H, R5
    MOV 41H, R6
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV R0, #30H        ; Destination for factors starting at 30H
    MOV R1, #48H        ; Number = 48H (Decimal 72 = 2 * 2 * 2 * 3 * 3)
    MOV R2, #02H        ; Starting divisor = 2
    CLR A
    MOV R5, A           ; R5 accumulates sum of prime factors
    MOV R6, A           ; R6 holds carry for sum
FACTOR_48:
    MOV A, R1
    MOV B, R2
    DIV AB              ; Check divisibility: Remainder in B
    MOV A, B
    JNZ NEXT_P          ; If remainder != 0, increment divisor
    ; Divisible: R2 is a prime factor
    MOV A, R2
    MOV @R0, A          ; Store factor at @R0 (30H..)
    INC R0              ; Advance pointer
    ADD A, R5           ; Add factor to cumulative sum
    MOV R5, A           ; Update sum
    JNC NO_CY_P         ; If no carry, skip
    INC R6              ; Increment carry
NO_CY_P:
    MOV A, R1
    MOV B, R2
    DIV AB
    MOV R1, A           ; R1 = R1 / R2
    CJNE R1, #01H, FACTOR_48 ; Loop until number is 1
    SJMP STORE_SUM
NEXT_P:
    INC R2              ; Next candidate divisor
    SJMP FACTOR_48
STORE_SUM:
    MOV 40H, R5         ; Store sum of prime factors at 40H (2+2+2+3+3 = 12 = 0CH)
    MOV 41H, R6         ; Store carry at 41H (00H)
    SJMP $              ; Stop
    END
```



# ASSIGNMENT 6: LED Panel Interfacing (Virtual HW MCU 8051 Simulator)

> [!NOTE]
> **Active-Low Hardware Rule:** In the MCU 8051 Simulator LED Panel, **logic 0 turns the LED ON** and **logic 1 turns the LED OFF**. The codes below use the exact active-low specifications matching the college lab hardware.

---

## Assignment 6 — Question 1
### Proper Question Title:
**Turning ON All Eight LEDs Simultaneously on the Virtual HW LED Panel**

### Short Name to Remember:
`A6Q1_LED_Panel_All_ON` (Send 00H to Port 1 to turn all 8 LEDs ON)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV P1, #00H
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV P1, #00H        ; Send 0000 0000B to Port 1; in active-low, 0 turns ON all LEDs
    SJMP $              ; Infinite loop to keep LEDs illuminated
    END
```

### How It Worked & Registers Used:
- `Port 1` (`P1` at SFR `90H`) is tied to the 8 LEDs of the LED Panel in settings.
- Writing `0` provides a ground sink path, causing current to flow through the LED cathode and turning it ON.

### Examiner Viva Questions & Potential Modifications:
- **"What if the circuit is active-high?"**
  - *Answer:* For active-high, logic `1` turns the LED ON, so send `MOV P1, #0FFH`.

---

## Assignment 6 — Question 2
### Proper Question Title:
**Turning ON Even-Numbered LEDs (Bit 0, Bit 2, Bit 4, Bit 6) on the Selected Port**

### Short Name to Remember:
`A6Q2_LED_Even_Bits_ON` (Bits 0, 2, 4, 6 ON -> P1 = 0AAH)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV P1, #0AAH
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    ; In active-low: 0 = ON, 1 = OFF
    ; Bit 7: OFF (1), Bit 6: ON (0), Bit 5: OFF (1), Bit 4: ON (0)
    ; Bit 3: OFF (1), Bit 2: ON (0), Bit 1: OFF (1), Bit 0: ON (0)
    ; Binary: 1010 1010B = 0AAH
    MOV P1, #0AAH       ; Turn ON bits 0, 2, 4, 6; keep bits 1, 3, 5, 7 OFF
    SJMP $              ; Halt
    END
```

---

## Assignment 6 — Question 3
### Proper Question Title:
**Displaying Binary Pattern 00110011 on the Eight LEDs of the LED Panel**

### Short Name to Remember:
`A6Q3_LED_Pattern_00110011` (Active-low inverted pattern: 0CCH)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV P1, #0CCH
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    ; Target visual pattern: 0 0 1 1 0 0 1 1 (where 1 represents bright LED)
    ; In MCU 8051 Active-Low panel, bright LED requires bit = 0
    ; Inverted bit representation:
    ; Bit 7..0: 1 1 0 0 1 1 0 0 B = 0CCH
    MOV P1, #0CCH       ; Output inverted bit pattern to illuminate 00110011
    SJMP $              ; Halt
    END
```

---

## Assignment 6 — Question 4
### Proper Question Title:
**Continuous Blinking of Alternate LEDs Using the LED Panel and Software Delay**

### Short Name to Remember:
`A6Q4_LED_Alternate_Blink` (Toggle between 55H and 0AAH continuously with delay)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
BLINK_LOOP:
    MOV P1, #55H
    ACALL DELAY
    MOV P1, #0AAH
    ACALL DELAY
    SJMP BLINK_LOOP

DELAY:
    MOV R6, #04H
D1:
    MOV R5, #0FFH
D2:
    MOV R4, #0FFH
D3:
    DJNZ R4, D3
    DJNZ R5, D2
    DJNZ R6, D1
    RET
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
BLINK_LOOP:
    MOV P1, #55H        ; 0101 0101B -> Turns ON bits 1, 3, 5, 7
    ACALL DELAY         ; Keep state visible
    MOV P1, #0AAH       ; 1010 1010B -> Turns ON bits 0, 2, 4, 6
    ACALL DELAY         ; Keep state visible
    SJMP BLINK_LOOP     ; Repeat toggle forever

DELAY:
    MOV R6, #04H        ; Triple-nested software delay subroutine
D1:
    MOV R5, #0FFH
D2:
    MOV R4, #0FFH
D3:
    DJNZ R4, D3         ; Innermost loop
    DJNZ R5, D2         ; Middle loop
    DJNZ R6, D1         ; Outer loop
    RET                 ; Return to caller
    END
```

---

## Assignment 6 — Question 5
### Proper Question Title:
**Sequential Running LED (Chaser) from Bit 0 to Bit 7**

### Short Name to Remember:
`A6Q5_LED_Sequence_0_to_7` (Single ON LED moves from Bit 0 to Bit 7)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
START:
    MOV A, #0FEH
    MOV R2, #08H
SEQ_LOOP:
    MOV P1, A
    ACALL DELAY
    RL A
    DJNZ R2, SEQ_LOOP
    SJMP START

DELAY:
    MOV R6, #03H
L1:
    MOV R5, #0FFH
L2:
    MOV R4, #0FFH
L3:
    DJNZ R4, L3
    DJNZ R5, L2
    DJNZ R6, L1
    RET
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
START:
    MOV A, #0FEH        ; 1111 1110B (Bit 0 is 0 = ON, all other bits 1 = OFF)
    MOV R2, #08H        ; 8 LED positions
SEQ_LOOP:
    MOV P1, A           ; Output current active bit to Port 1
    ACALL DELAY         ; Delay for human visibility
    RL A                ; Rotate left: moves 0 from Bit 0 to Bit 1, Bit 2... Bit 7
    DJNZ R2, SEQ_LOOP   ; Repeat for all 8 bits
    SJMP START          ; Continuously repeat sequence

DELAY:
    MOV R6, #03H
L1:
    MOV R5, #0FFH
L2:
    MOV R4, #0FFH
L3:
    DJNZ R4, L3
    DJNZ R5, L2
    DJNZ R6, L1
    RET
    END
```

---

## Assignment 6 — Question 6
### Proper Question Title:
**Reverse Sequential Running LED (Chaser) from Bit 7 to Bit 0**

### Short Name to Remember:
`A6Q6_LED_Sequence_7_to_0` (Single ON LED moves from Bit 7 down to Bit 0 using RR)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
START:
    MOV A, #7FH
    MOV R2, #08H
REV_LOOP:
    MOV P1, A
    ACALL DELAY
    RR A
    DJNZ R2, REV_LOOP
    SJMP START

DELAY:
    MOV R6, #03H
L1:
    MOV R5, #0FFH
L2:
    MOV R4, #0FFH
L3:
    DJNZ R4, L3
    DJNZ R5, L2
    DJNZ R6, L1
    RET
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
START:
    MOV A, #7FH         ; 0111 1111B (Bit 7 is 0 = ON, Bits 0-6 are 1 = OFF)
    MOV R2, #08H        ; 8 LED positions
REV_LOOP:
    MOV P1, A           ; Output to Port 1
    ACALL DELAY         ; Delay
    RR A                ; Rotate right: moves the 0 from Bit 7 down to Bit 0
    DJNZ R2, REV_LOOP   ; Loop 8 times
    SJMP START          ; Repeat sequence forever

DELAY:
    MOV R6, #03H
L1:
    MOV R5, #0FFH
L2:
    MOV R4, #0FFH
L3:
    DJNZ R4, L3
    DJNZ R5, L2
    DJNZ R6, L1
    RET
    END
```

---

# ASSIGNMENT 7: LED Display Configuration (Common Anode & Common Cathode)

---

## Assignment 7 — Question 1
### Proper Question Title:
**Testing Bit 5 of Port 2 and Turning ON the Corresponding LED Segment in Common Anode Configuration**

### Short Name to Remember:
`A7Q1_Test_Bit5_CA` (Turn ON segment 5 in Common Anode -> CLR P2.5)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV P2, #0FFH
    CLR P2.5
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV P2, #0FFH       ; In Common Anode, 1 = OFF; turn OFF all segments first
    CLR P2.5            ; Clear bit 5 (P2.5 = 0); in Common Anode, 0 turns ON the segment
    SJMP $              ; Halt
    END
```

---

## Assignment 7 — Question 2
### Proper Question Title:
**Turning ON All LED Segments of Port 3 in Common Anode Configuration**

### Short Name to Remember:
`A7Q2_All_ON_Common_Anode` (Port 3 all ON in Common Anode -> P3 = 00H)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV P3, #00H
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV P3, #00H        ; Common Anode: Anodes tied to VCC (+5V). Low (0) turns segment ON.
    SJMP $              ; Keep all segments ON
    END
```

---

## Assignment 7 — Question 3
### Proper Question Title:
**Turning ON All LED Segments of Port 3 in Common Cathode Configuration**

### Short Name to Remember:
`A7Q3_All_ON_Common_Cathode` (Port 3 all ON in Common Cathode -> P3 = 0FFH)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV P3, #0FFH
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV P3, #0FFH       ; Common Cathode: Cathodes tied to GND (0V). High (1) turns segment ON.
    SJMP $              ; Keep all segments ON
    END
```

---

## Assignment 7 — Question 4
### Proper Question Title:
**Sequential Illumination of Segments Bit 0 to Bit 7 on Port 0 in Common Anode Configuration with Delay**

### Short Name to Remember:
`A7Q4_Sequential_P0_CA` (Sequential 0 to 7 on Port 0 Common Anode)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
START:
    MOV A, #0FEH
    MOV R2, #08H
SEQ_LOOP:
    MOV P0, A
    ACALL DELAY
    RL A
    DJNZ R2, SEQ_LOOP
    SJMP START

DELAY:
    MOV R6, #03H
D1:
    MOV R5, #0FFH
D2:
    MOV R4, #0FFH
D3:
    DJNZ R4, D3
    DJNZ R5, D2
    DJNZ R6, D1
    RET
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
START:
    MOV A, #0FEH        ; 1111 1110B (Bit 0 = 0 -> ON in Common Anode)
    MOV R2, #08H        ; 8 segments
SEQ_LOOP:
    MOV P0, A           ; Output to Port 0 (Ensure pull-up resistors connected)
    ACALL DELAY         ; Visibility delay
    RL A                ; Rotate left to next segment
    DJNZ R2, SEQ_LOOP   ; Loop through all 8 segments
    SJMP START          ; Continuous operation

DELAY:
    MOV R6, #03H
D1:
    MOV R5, #0FFH
D2:
    MOV R4, #0FFH
D3:
    DJNZ R4, D3
    DJNZ R5, D2
    DJNZ R6, D1
    RET
    END
```

---

## Assignment 7 — Question 5
### Proper Question Title:
**Displaying Alternate Bit Pattern (Bits 0, 2, 4, 6 ON and Bits 1, 3, 5, 7 OFF) on Port 0**

### Short Name to Remember:
`A7Q5_Alternate_Bits_P0_CA` (Bits 0,2,4,6 ON in Common Anode -> P0 = 0AAH)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV P0, #0AAH
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    ; In Common Anode: 0 = ON, 1 = OFF
    ; Required ON: Bits 0, 2, 4, 6 (Logic 0)
    ; Required OFF: Bits 1, 3, 5, 7 (Logic 1)
    ; Binary: 1010 1010B = 0AAH
    MOV P0, #0AAH       ; Output alternate pattern to Port 0
    SJMP $              ; Halt
    END
```

---

## Assignment 7 — Question 6
### Proper Question Title:
**Sequential Illumination of Segments from Bit 7 to Bit 0 on Port 1 in Common Anode Configuration with Delay**

### Short Name to Remember:
`A7Q6_Reverse_Sequential_P1_CA` (Sequential 7 to 0 on Port 1 Common Anode using RR)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
START:
    MOV A, #7FH
    MOV R2, #08H
REV_P1:
    MOV P1, A
    ACALL DELAY
    RR A
    DJNZ R2, REV_P1
    SJMP START

DELAY:
    MOV R6, #03H
D1:
    MOV R5, #0FFH
D2:
    MOV R4, #0FFH
D3:
    DJNZ R4, D3
    DJNZ R5, D2
    DJNZ R6, D1
    RET
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
START:
    MOV A, #7FH         ; 0111 1111B (Bit 7 = 0 -> ON in Common Anode)
    MOV R2, #08H        ; 8 segments
REV_P1:
    MOV P1, A           ; Output to Port 1
    ACALL DELAY         ; Delay
    RR A                ; Rotate right: moves the 0 from Bit 7 to Bit 6, 5... 0
    DJNZ R2, REV_P1     ; Loop 8 times
    SJMP START          ; Continuous loop

DELAY:
    MOV R6, #03H
D1:
    MOV R5, #0FFH
D2:
    MOV R4, #0FFH
D3:
    DJNZ R4, D3
    DJNZ R5, D2
    DJNZ R6, D1
    RET
    END
```

---

## Assignment 7 — Question 7
### Proper Question Title:
**Bidirectional Running LED Pattern on Port 0 (Bit 0 to Bit 7 and Back to Bit 0 Continuously)**

### Short Name to Remember:
`A7Q7_Bidirectional_Runner_P0` (Bouncing LED runner: 0 -> 7 -> 0 continuously on Port 0)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
MAIN_LOOP:
    MOV A, #0FEH
    MOV R2, #07H
FWD:
    MOV P0, A
    ACALL DELAY
    RL A
    DJNZ R2, FWD

    MOV R2, #07H
BWD:
    MOV P0, A
    ACALL DELAY
    RR A
    DJNZ R2, BWD

    SJMP MAIN_LOOP

DELAY:
    MOV R6, #03H
D1:
    MOV R5, #0FFH
D2:
    MOV R4, #0FFH
D3:
    DJNZ R4, D3
    DJNZ R5, D2
    DJNZ R6, D1
    RET
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
MAIN_LOOP:
    ; Forward Pass: Bit 0 to Bit 7 (7 steps)
    MOV A, #0FEH        ; Starts at Bit 0 (1111 1110B)
    MOV R2, #07H        ; 7 forward shifts to reach Bit 7
FWD:
    MOV P0, A           ; Output current position
    ACALL DELAY         ; Delay
    RL A                ; Shift left towards Bit 7
    DJNZ R2, FWD        ; Repeat 7 times

    ; Backward Pass: Bit 7 down to Bit 0 (7 steps)
    MOV R2, #07H        ; 7 backward shifts to return to Bit 0
BWD:
    MOV P0, A           ; Output current position
    ACALL DELAY         ; Delay
    RR A                ; Shift right towards Bit 0
    DJNZ R2, BWD        ; Repeat 7 times

    SJMP MAIN_LOOP      ; Continuous back-and-forth bouncing

DELAY:
    MOV R6, #03H
D1:
    MOV R5, #0FFH
D2:
    MOV R4, #0FFH
D3:
    DJNZ R4, D3
    DJNZ R5, D2
    DJNZ R6, D1
    RET
    END
```

---

# ASSIGNMENT 8: Multiplexed 4-Digit 7-Segment & 8x8 LED Matrix

---

## Assignment 8 — Question 1
### Proper Question Title:
**Displaying Year of Birth (e.g., 2003) on a 4-Digit Common-Anode 7-Segment Display Using Multiplexing**

### Short Name to Remember:
`A8Q1_Birth_Year_Multiplex_7Seg` (Display '2003' on 4-digit CA display via multiplexing)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
DISP_LOOP:
    MOV P2, #08H
    MOV P1, #0A4H
    ACALL SHORT_DELAY

    MOV P2, #04H
    MOV P1, #0C0H
    ACALL SHORT_DELAY

    MOV P2, #02H
    MOV P1, #0C0H
    ACALL SHORT_DELAY

    MOV P2, #01H
    MOV P1, #0B0H
    ACALL SHORT_DELAY

    SJMP DISP_LOOP

SHORT_DELAY:
    MOV R5, #0AH
SD1:
    MOV R4, #0FFH
SD2:
    DJNZ R4, SD2
    DJNZ R5, SD1
    RET
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    ; 4-Digit Common Anode Setup:
    ; Digit selection on Port 2 (Active High PNP drivers: P2.3=Digit3, P2.2=Digit2, P2.1=Digit1, P2.0=Digit0)
    ; 7-segment data on Port 1 (Common Anode: 0 = ON, 1 = OFF)
    ; 7-segment 0-9 CA codes:
    ; '0' = C0H, '1' = F9H, '2' = A4H, '3' = B0H, '4' = 99H, '5' = 92H, '6' = 82H, '7' = F8H, '8' = 80H, '9' = 90H
DISP_LOOP:
    ; Display Digit 3 (Leftmost) = '2'
    MOV P2, #08H        ; Enable Digit 3 (P2.3 = 1)
    MOV P1, #0A4H       ; 7-seg code for '2'
    ACALL SHORT_DELAY   ; Persistence of vision delay (~2ms)

    ; Display Digit 2 = '0'
    MOV P2, #04H        ; Enable Digit 2 (P2.2 = 1)
    MOV P1, #0C0H       ; 7-seg code for '0'
    ACALL SHORT_DELAY

    ; Display Digit 1 = '0'
    MOV P2, #02H        ; Enable Digit 1 (P2.1 = 1)
    MOV P1, #0C0H       ; 7-seg code for '0'
    ACALL SHORT_DELAY

    ; Display Digit 0 (Rightmost) = '3'
    MOV P2, #01H        ; Enable Digit 0 (P2.0 = 1)
    MOV P1, #0B0H       ; 7-seg code for '3'
    ACALL SHORT_DELAY

    SJMP DISP_LOOP      ; Loop continuously to maintain steady multiplexed display

SHORT_DELAY:
    MOV R5, #0AH        ; Short delay routine
SD1:
    MOV R4, #0FFH
SD2:
    DJNZ R4, SD2
    DJNZ R5, SD1
    RET
    END
```

---

## Assignment 8 — Question 2
### Proper Question Title:
**Displaying Repeated Sequence 0000 -> 1111 -> ... -> 9999 on 4-Digit 7-Segment Display**

### Short Name to Remember:
`A8Q2_0000_to_9999_Sequence` (Sequential 0000 to 9999 on 4-digit display with pattern delay)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
START:
    MOV DPTR, #SEG_CODES
    MOV R7, #10
NEXT_DIGIT:
    CLR A
    MOVC A, @A+DPTR
    MOV R6, A
    MOV R3, #50
REFRESH:
    MOV P2, #08H
    MOV P1, R6
    ACALL SDELAY
    MOV P2, #04H
    MOV P1, R6
    ACALL SDELAY
    MOV P2, #02H
    MOV P1, R6
    ACALL SDELAY
    MOV P2, #01H
    MOV P1, R6
    ACALL SDELAY
    DJNZ R3, REFRESH
    INC DPTR
    DJNZ R7, NEXT_DIGIT
    SJMP START

SDELAY:
    MOV R5, #05H
S1:
    MOV R4, #0FFH
S2:
    DJNZ R4, S2
    DJNZ R5, S1
    RET

    ORG 0200H
SEG_CODES:
    DB 0C0H, 0F9H, 0A4H, 0B0H, 99H, 92H, 82H, 0F8H, 80H, 90H
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
START:
    MOV DPTR, #SEG_CODES; DPTR points to 7-segment table for digits 0-9
    MOV R7, #10         ; 10 digits to cycle through (0 to 9)
NEXT_DIGIT:
    CLR A
    MOVC A, @A+DPTR     ; Fetch CA code for current digit
    MOV R6, A           ; Hold current pattern in R6
    MOV R3, #50         ; Hold pattern for ~50 refresh cycles (~0.5 sec)
REFRESH:
    MOV P2, #08H        ; Enable Digit 3
    MOV P1, R6          ; Output digit pattern
    ACALL SDELAY
    MOV P2, #04H        ; Enable Digit 2
    MOV P1, R6
    ACALL SDELAY
    MOV P2, #02H        ; Enable Digit 1
    MOV P1, R6
    ACALL SDELAY
    MOV P2, #01H        ; Enable Digit 0
    MOV P1, R6
    ACALL SDELAY
    DJNZ R3, REFRESH    ; Repeat multiplexing to achieve visible hold time
    INC DPTR            ; Advance to next digit code
    DJNZ R7, NEXT_DIGIT ; Cycle through 0 to 9
    SJMP START          ; Repeat sequence continuously

SDELAY:
    MOV R5, #05H
S1:
    MOV R4, #0FFH
S2:
    DJNZ R4, S2
    DJNZ R5, S1
    RET

    ORG 0200H
SEG_CODES:
    DB 0C0H, 0F9H, 0A4H, 0B0H, 99H, 92H, 82H, 0F8H, 80H, 90H
    END
```

---

## Assignment 8 — Question 3
### Proper Question Title:
**Simultaneously Turning ON Two Specific LEDs on an 8x8 LED Matrix (Intersections of Rows C & F with Column 4)**

### Short Name to Remember:
`A8Q3_LED_Matrix_Two_LEDs` (Turn ON Row C & F at Column 4 using Active-Low logic)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV P1, #0DBH
    MOV P2, #0EFH
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    ; 8x8 LED Matrix Active-Low Configuration:
    ; Row Port = P1 (Row A = Bit 0, B = Bit 1, C = Bit 2, D = Bit 3, E = Bit 4, F = Bit 5, G = Bit 6, H = Bit 7)
    ; Column Port = P2 (Col 0 = Bit 0, Col 1 = Bit 1, Col 2 = Bit 2, Col 3 = Bit 3, Col 4 = Bit 4...)
    ; In active-low matrix: An LED at (Row R, Col C) is ON when Row = 0 and Col = 0
    ; Required Rows:
    ; Row C (Bit 2) = 0
    ; Row F (Bit 5) = 0
    ; All other rows = 1 (OFF)
    ; Binary: 1101 1011B = 0DBH -> Row Port P1
    ; Required Column:
    ; Column 4 (Bit 4) = 0
    ; All other columns = 1 (OFF)
    ; Binary: 1110 1111B = 0EFH -> Column Port P2
    MOV P1, #0DBH       ; Activate Rows C and F (Active Low)
    MOV P2, #0EFH       ; Activate Column 4 (Active Low)
    SJMP $              ; Keep the two LEDs illuminated
    END
```

---

## Assignment 8 — Question 4
### Proper Question Title:
**Continuously Moving Bright Row on an 8x8 LED Matrix (Rows A to H) Using Active-Low Logic**

### Short Name to Remember:
`A8Q4_LED_Matrix_Moving_Row` (Scan Row A through H continuously on 8x8 matrix)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV P2, #00H
ROW_SCAN:
    MOV A, #0FEH
    MOV R2, #08H
NEXT_ROW:
    MOV P1, A
    ACALL MDELAY
    RL A
    DJNZ R2, NEXT_ROW
    SJMP ROW_SCAN

MDELAY:
    MOV R6, #03H
M1:
    MOV R5, #0FFH
M2:
    MOV R4, #0FFH
M3:
    DJNZ R4, M3
    DJNZ R5, M2
    DJNZ R6, M1
    RET
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV P2, #00H        ; Activate all columns (Col = 00H = 00000000B -> all 8 columns ON)
ROW_SCAN:
    MOV A, #0FEH        ; Start with Row A (Bit 0 = 0 -> ON)
    MOV R2, #08H        ; 8 rows (A through H)
NEXT_ROW:
    MOV P1, A           ; Output active row to Port 1
    ACALL MDELAY        ; Hold row for specified delay
    RL A                ; Rotate left: moves active 0 from Row A to B, C, D... H
    DJNZ R2, NEXT_ROW   ; Cycle through all 8 rows
    SJMP ROW_SCAN       ; Repeat scanning forever

MDELAY:
    MOV R6, #03H
M1:
    MOV R5, #0FFH
M2:
    MOV R4, #0FFH
M3:
    DJNZ R4, M3
    DJNZ R5, M2
    DJNZ R6, M1
    RET
    END
```

---

## Assignment 8 — Question 5
### Proper Question Title:
**Continuously Moving Bright Column on an 8x8 LED Matrix (Columns C0 to C7) Using Active-Low Logic**

### Short Name to Remember:
`A8Q5_LED_Matrix_Moving_Col` (Scan Col C0 through C7 continuously on 8x8 matrix)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV P1, #00H
COL_SCAN:
    MOV A, #0FEH
    MOV R2, #08H
NEXT_COL:
    MOV P2, A
    ACALL MDELAY
    RL A
    DJNZ R2, NEXT_COL
    SJMP COL_SCAN

MDELAY:
    MOV R6, #03H
M1:
    MOV R5, #0FFH
M2:
    MOV R4, #0FFH
M3:
    DJNZ R4, M3
    DJNZ R5, M2
    DJNZ R6, M1
    RET
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV P1, #00H        ; Activate all rows (Row = 00H -> all rows grounded)
COL_SCAN:
    MOV A, #0FEH        ; Start with Column 0 (Bit 0 = 0 -> ON)
    MOV R2, #08H        ; 8 columns (C0 through C7)
NEXT_COL:
    MOV P2, A           ; Output active column to Port 2
    ACALL MDELAY        ; Hold column for specified delay
    RL A                ; Rotate left: moves active 0 from Col C0 to C1... C7
    DJNZ R2, NEXT_COL   ; Cycle through all 8 columns
    SJMP COL_SCAN       ; Repeat scanning forever

MDELAY:
    MOV R6, #03H
M1:
    MOV R5, #0FFH
M2:
    MOV R4, #0FFH
M3:
    DJNZ R4, M3
    DJNZ R5, M2
    DJNZ R6, M1
    RET
    END
```

---

## Assignment 8 — Question 6
### Proper Question Title:
**Sequential Display of Decimal Digits 0 to 9 on an 8x8 LED Matrix Using Active-Low Logic**

### Short Name to Remember:
`A8Q6_LED_Matrix_Digits_0_to_9` (Display digits 0-9 sequentially on 8x8 matrix with multiplexing)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
START:
    MOV DPTR, #DIGIT_PATTERNS
    MOV R7, #10
DIGIT_LOOP:
    MOV R3, #40
FRAME_LOOP:
    MOV R1, #08H
    MOV R2, #0FEH
    MOV A, DPL
    MOV 30H, A
    MOV A, DPH
    MOV 31H, A
SCAN_COLS:
    CLR A
    MOVC A, @A+DPTR
    MOV P1, A
    MOV P2, R2
    ACALL RDELAY
    MOV P2, #0FFH
    INC DPTR
    MOV A, R2
    RL A
    MOV R2, A
    DJNZ R1, SCAN_COLS
    MOV DPL, 30H
    MOV DPH, 31H
    DJNZ R3, FRAME_LOOP
    MOV A, DPL
    ADD A, #08H
    MOV DPL, A
    JNC NO_C_DPTR
    INC DPH
NO_C_DPTR:
    DJNZ R7, DIGIT_LOOP
    SJMP START

RDELAY:
    MOV R5, #02H
R1L:
    MOV R4, #080H
R2L:
    DJNZ R4, R2L
    DJNZ R5, R1L
    RET

    ORG 0200H
DIGIT_PATTERNS:
    ; Digit 0
    DB 0E3H, 0DDH, 0BEH, 0BEH, 0BEH, 0BEH, 0DDH, 0E3H
    ; Digit 1
    DB 0EFH, 0CFH, 0EFH, 0EFH, 0EFH, 0EFH, 0EFH, 083H
    ; Digit 2
    DB 0E3H, 0DDH, 0FDH, 0FBH, 0F7H, 0EFH, 0DFH, 081H
    ; Digit 3
    DB 081H, 0FBH, 0F7H, 0E3H, 0FDH, 0FDH, 0DDH, 0E3H
    ; Digit 4
    DB 0FBH, 0F3H, 0EBH, 0DBH, 081H, 0FBH, 0FBH, 0E3H
    ; Digit 5
    DB 081H, 0DFH, 0DFH, 083H, 0FDH, 0FDH, 0DDH, 0E3H
    ; Digit 6
    DB 0E3H, 0DDH, 0DFH, 083H, 0DDH, 0DDH, 0DDH, 0E3H
    ; Digit 7
    DB 081H, 0FDH, 0FBH, 0F7H, 0EFH, 0DFH, 0BFH, 07FH
    ; Digit 8
    DB 0E3H, 0DDH, 0DDH, 0E3H, 0DDH, 0DDH, 0DDH, 0E3H
    ; Digit 9
    DB 0E3H, 0DDH, 0DDH, 0C1H, 0FDH, 0FDH, 0DDH, 0E3H
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
START:
    MOV DPTR, #DIGIT_PATTERNS ; Base of font table in ROM (8 bytes per digit)
    MOV R7, #10         ; 10 decimal digits (0 to 9)
DIGIT_LOOP:
    MOV R3, #40         ; Hold each digit on display for ~40 refresh frames
FRAME_LOOP:
    MOV R1, #08H        ; 8 columns per character matrix
    MOV R2, #0FEH       ; Column 0 active-low mask (1111 1110B)
    MOV A, DPL          ; Save DPTR base for column scanning
    MOV 30H, A
    MOV A, DPH
    MOV 31H, A
SCAN_COLS:
    CLR A
    MOVC A, @A+DPTR     ; Fetch row mask for current column
    MOV P1, A           ; Output rows to Port 1
    MOV P2, R2          ; Drive active column on Port 2
    ACALL RDELAY        ; Multiplex delay (~1ms)
    MOV P2, #0FFH       ; Blank column to prevent ghosting
    INC DPTR            ; Advance to next column data
    MOV A, R2
    RL A                ; Shift active column mask
    MOV R2, A
    DJNZ R1, SCAN_COLS  ; Scan all 8 columns
    MOV DPL, 30H        ; Restore DPTR base for current digit
    MOV DPH, 31H
    DJNZ R3, FRAME_LOOP ; Repeat frame to achieve visible display duration
    ; Advance DPTR to next digit (add 8 bytes)
    MOV A, DPL
    ADD A, #08H
    MOV DPL, A
    JNC NO_C_DPTR
    INC DPH
NO_C_DPTR:
    DJNZ R7, DIGIT_LOOP ; Process next digit (0 to 9)
    SJMP START          ; Loop continuously from 0 again

RDELAY:
    MOV R5, #02H
R1L:
    MOV R4, #080H
R2L:
    DJNZ R4, R2L
    DJNZ R5, R1L
    RET

    ORG 0200H
DIGIT_PATTERNS:
    ; 8x8 font bitmaps for decimal digits 0 through 9 (Active-Low: 0 = LED ON)
    DB 0E3H, 0DDH, 0BEH, 0BEH, 0BEH, 0BEH, 0DDH, 0E3H ; '0'
    DB 0EFH, 0CFH, 0EFH, 0EFH, 0EFH, 0EFH, 0EFH, 083H ; '1'
    DB 0E3H, 0DDH, 0FDH, 0FBH, 0F7H, 0EFH, 0DFH, 081H ; '2'
    DB 081H, 0FBH, 0F7H, 0E3H, 0FDH, 0FDH, 0DDH, 0E3H ; '3'
    DB 0FBH, 0F3H, 0EBH, 0DBH, 081H, 0FBH, 0FBH, 0E3H ; '4'
    DB 081H, 0DFH, 0DFH, 083H, 0FDH, 0FDH, 0DDH, 0E3H ; '5'
    DB 0E3H, 0DDH, 0DFH, 083H, 0DDH, 0DDH, 0DDH, 0E3H ; '6'
    DB 081H, 0FDH, 0FBH, 0F7H, 0EFH, 0DFH, 0BFH, 07FH ; '7'
    DB 0E3H, 0DDH, 0DDH, 0E3H, 0DDH, 0DDH, 0DDH, 0E3H ; '8'
    DB 0E3H, 0DDH, 0DDH, 0C1H, 0FDH, 0FDH, 0DDH, 0E3H ; '9'
    END
```



# ASSIGNMENT 9: Proteus Simulator — Embedded C Port & Hardware Interfacing

---

## Assignment 9 — Question 1
### Proper Question Title:
**16-Bit Addition in Embedded C with Result Displayed on Ports P1 (MSB) and P2 (LSB)**

### Short Name to Remember:
`A9Q1_16Bit_Add_P1_P2` (Add two 16-bit ints; MSB -> P1, LSB -> P2)

### Answer WITHOUT Comments:
```c
#include <reg51.h>

void main(void) {
    unsigned int num1 = 0x1234;
    unsigned int num2 = 0x5678;
    unsigned int sum;

    sum = num1 + num2;
    P1 = (unsigned char)(sum >> 8);
    P2 = (unsigned char)(sum & 0x00FF);

    while (1);
}
```

### Answer WITH Comments:
```c
#include <reg51.h>      // Standard 8051 SFR register definitions

void main(void) {
    // Two 16-bit unsigned variables (range: 0 to 65535)
    unsigned int num1 = 0x1234;
    unsigned int num2 = 0x5678;
    unsigned int sum;

    sum = num1 + num2;                      // 0x1234 + 0x5678 = 0x68AC
    P1 = (unsigned char)(sum >> 8);         // Extract high byte (0x68) and display on Port 1
    P2 = (unsigned char)(sum & 0x00FF);     // Extract low byte (0xAC) and display on Port 2

    while (1);                              // Halt in continuous loop to keep LEDs active
}
```

### How It Worked & Registers Used:
- `unsigned int` in Keil C51 is a 16-bit word stored in Big-Endian format.
- Bitwise right-shift `sum >> 8` shifts the upper 8 bits down to the lower byte for Port 1.
- Bitwise AND `sum & 0x00FF` isolates the lower 8 bits for Port 2.
- **Proteus Circuit Setup:**
  - Connect Port 1 (Pins 1-8) to `LED-BARGRAPH-RED` (MSB).
  - Connect Port 2 (Pins 21-28) to a second `LED-BARGRAPH-RED` (LSB).
  - Connect cathode pins of LED bargraph to `GND`.
  - Connect `CRYSTAL` with two 33pF capacitors between XTAL1 and XTAL2.

---

## Assignment 9 — Question 2
### Proper Question Title:
**Sending Signed Values from -7 to +7 to Port P2 in Embedded C**

### Short Name to Remember:
`A9Q2_Send_Minus7_to_Plus7_P2` (Cycle signed values -7 to +7 to Port 2 with delay)

### Answer WITHOUT Comments:
```c
#include <reg51.h>

void delay_ms(unsigned int ms) {
    unsigned int i, j;
    for (i = 0; i < ms; i++)
        for (j = 0; j < 120; j++);
}

void main(void) {
    signed char val;
    while (1) {
        for (val = -7; val <= 7; val++) {
            P2 = val;
            delay_ms(200);
        }
    }
}
```

### Answer WITH Comments:
```c
#include <reg51.h>

// Millisecond software delay routine for 11.0592 MHz / 12 MHz
void delay_ms(unsigned int ms) {
    unsigned int i, j;
    for (i = 0; i < ms; i++)
        for (j = 0; j < 120; j++);
}

void main(void) {
    signed char val;    // 8-bit signed integer (-128 to +127)
    while (1) {
        for (val = -7; val <= 7; val++) {
            P2 = val;   // Send 2's complement value to Port 2 (e.g. -7 is 0xF9, 0 is 0x00, +7 is 0x07)
            delay_ms(200); // 200 ms visible delay
        }
    }
}
```

---

## Assignment 9 — Question 3
### Proper Question Title:
**Continuous Toggling of Port P1 Bits with Software Delay in Embedded C**

### Short Name to Remember:
`A9Q3_Toggle_P1_Forever` (Toggle P1 indefinitely with delay: P1 = ~P1)

### Answer WITHOUT Comments:
```c
#include <reg51.h>

void delay(void) {
    unsigned int i, j;
    for (i = 0; i < 250; i++)
        for (j = 0; j < 250; j++);
}

void main(void) {
    P1 = 0x55;
    while (1) {
        P1 = ~P1;
        delay();
    }
}
```

### Answer WITH Comments:
```c
#include <reg51.h>

void delay(void) {
    unsigned int i, j;
    for (i = 0; i < 250; i++)
        for (j = 0; j < 250; j++);
}

void main(void) {
    P1 = 0x55;          // Initial pattern: 0101 0101B
    while (1) {
        P1 = ~P1;       // Invert all bits (0x55 -> 0xAA -> 0x55...)
        delay();        // Delay to make toggling visually perceivable on LEDs/scope
    }
}
```

---

## Assignment 9 — Question 4
### Proper Question Title:
**Generating 8-Bit Binary Count (00H to FFH) Displayed Simultaneously on Ports P1, P2, and P3**

### Short Name to Remember:
`A9Q4_Binary_Counter_P1_P2_P3` (Simultaneous 00H-FFH counter on P1, P2, P3)

### Answer WITHOUT Comments:
```c
#include <reg51.h>

void delay(void) {
    unsigned int i, j;
    for (i = 0; i < 100; i++)
        for (j = 0; j < 120; j++);
}

void main(void) {
    unsigned char count = 0;
    while (1) {
        P1 = count;
        P2 = count;
        P3 = count;
        delay();
        count++;
    }
}
```

### Answer WITH Comments:
```c
#include <reg51.h>

void delay(void) {
    unsigned int i, j;
    for (i = 0; i < 100; i++)
        for (j = 0; j < 120; j++);
}

void main(void) {
    unsigned char count = 0; // 8-bit counter (0 to 255)
    while (1) {
        P1 = count;          // Send count to Port 1 LEDs
        P2 = count;          // Send same count to Port 2 LEDs
        P3 = count;          // Send same count to Port 3 LEDs
        delay();             // Time delay between increments
        count++;             // Naturally rolls over from 0xFF to 0x00
    }
}
```

---

## Assignment 9 — Question 5
### Proper Question Title:
**Reading Byte from Port P0 and Routing to Port P1 if < 100, Otherwise to Port P2**

### Short Name to Remember:
`A9Q5_Threshold_P0_to_P1_or_P2` (If P0 < 100 -> P1, else -> P2)

### Answer WITHOUT Comments:
```c
#include <reg51.h>

void main(void) {
    unsigned char input_data;
    P0 = 0xFF;

    while (1) {
        input_data = P0;
        if (input_data < 100) {
            P1 = input_data;
            P2 = 0x00;
        } else {
            P2 = input_data;
            P1 = 0x00;
        }
    }
}
```

### Answer WITH Comments:
```c
#include <reg51.h>

void main(void) {
    unsigned char input_data;
    P0 = 0xFF;          // Configure Port 0 as input port by writing all 1s (Open drain requires external pull-ups)

    while (1) {
        input_data = P0; // Read 8-bit value from switches connected to Port 0
        if (input_data < 100) {
            P1 = input_data; // Value is strictly less than 100: route to Port 1
            P2 = 0x00;       // Clear Port 2
        } else {
            P2 = input_data; // Value >= 100: route to Port 2
            P1 = 0x00;       // Clear Port 1
        }
    }
}
```

### Examiner Viva Questions & Potential Modifications:
- **"Why is `P0 = 0xFF` mandatory before reading?"**
  - *Answer:* 8051 port pins are quasi-bidirectional with open-drain/internal pullups. Writing `1` turns off the internal pulldown FET, allowing the external switch to pull the pin high or low without contention.

---

## Assignment 9 — Question 6
### Proper Question Title:
**DIP Switch Controlled Generation of 11 Fibonacci Terms on Port P2 with 400ms Delay in Embedded C**

### Short Name to Remember:
`A9Q6_DIPSW_Fibonacci_11_Terms` (DIPSW on P1.0-P1.3: when 11 -> 11 Fibonacci on P2; 400ms delay)

### Answer WITHOUT Comments:
```c
#include <reg51.h>

void delay_ms(unsigned int ms) {
    unsigned int i, j;
    for (i = 0; i < ms; i++)
        for (j = 0; j < 120; j++);
}

void main(void) {
    unsigned char fib[11];
    unsigned char i;
    unsigned char sw_val;

    P1 = 0xFF;
    P2 = 0x00;

    fib[0] = 0;
    fib[1] = 1;
    for (i = 2; i < 11; i++) {
        fib[i] = fib[i - 1] + fib[i - 2];
    }

    while (1) {
        sw_val = P1 & 0x0F;
        if (sw_val == 11) {
            for (i = 0; i < 11; i++) {
                P2 = fib[i];
                delay_ms(400);
            }
            while ((P1 & 0x0F) == 11);
        }
    }
}
```

### Answer WITH Comments:
```c
#include <reg51.h>

// Software millisecond delay function calibrated for 12 MHz
void delay_ms(unsigned int ms) {
    unsigned int i, j;
    for (i = 0; i < ms; i++)
        for (j = 0; j < 120; j++);
}

void main(void) {
    unsigned char fib[11]; // Array to hold 11 Fibonacci terms
    unsigned char i;
    unsigned char sw_val;

    P1 = 0xFF;             // Configure P1 as input port for DIP switch
    P2 = 0x00;             // Clear Port 2 output bargraph

    // Precompute first 11 terms: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55
    fib[0] = 0;
    fib[1] = 1;
    for (i = 2; i < 11; i++) {
        fib[i] = fib[i - 1] + fib[i - 2];
    }

    while (1) {
        sw_val = P1 & 0x0F; // Read 4-bit DIP switch on P1.0 - P1.3
        if (sw_val == 11) { // 11 in decimal = 1011 in binary (0x0B)
            for (i = 0; i < 11; i++) {
                P2 = fib[i];    // Display Fibonacci term on Port 2
                delay_ms(400);  // Hold display for 400 milliseconds
            }
            // Halt and wait until user releases or changes DIP switch from 11
            while ((P1 & 0x0F) == 11);
        }
    }
}
```

### Proteus Circuit Components Needed:
1. `AT89C51` Microcontroller.
2. `DIPSW_8` with `RESPACK-8` connected to `P1.0 - P1.3`.
3. `LED-BARGRAPH-RED` connected to `P2.0 - P2.7`.

---

# ASSIGNMENT 10: Logic Operations, Number Conversion & Serial Communication

---

## Assignment 10 — Question 1
### Proper Question Title:
**Bitwise XOR of Inputs from Port P1 and Port P0 with Complementary LED Displays on Ports P2 and P3**

### Short Name to Remember:
`A10Q1_Bitwise_XOR_P1_P0` (P1 ^ P0: 1 -> P2, 0 -> P3; exactly one LED ON per bit)

### Answer WITHOUT Comments:
```c
#include <reg51.h>

void main(void) {
    unsigned char in1, in2, xor_res;
    P1 = 0xFF;
    P0 = 0xFF;

    while (1) {
        in1 = P1;
        in2 = P0;
        xor_res = in1 ^ in2;
        P2 = xor_res;
        P3 = ~xor_res;
    }
}
```

### Answer WITH Comments:
```c
#include <reg51.h>

void main(void) {
    unsigned char in1, in2, xor_res;
    P1 = 0xFF;          // Configure Port 1 as input (from DIPSW 1)
    P0 = 0xFF;          // Configure Port 0 as input (from DIPSW 2; requires pull-up RESPACK-8)

    while (1) {
        in1 = P1;              // Read 8-bit input 1
        in2 = P0;              // Read 8-bit input 2
        xor_res = in1 ^ in2;   // Perform bitwise XOR operation
        P2 = xor_res;          // If XOR result is 1, turn ON LED on Port 2
        P3 = ~xor_res;         // If XOR result is 0, turn ON LED on Port 3
        // Since P3 = ~xor_res, for every bit position, exactly one LED on P2 or P3 is ON!
    }
}
```

---

## Assignment 10 — Question 2
### Proper Question Title:
**Hexadecimal to Decimal Conversion with Hundreds, Tens, and Units Displayed on Ports P1, P2, and P3**

### Short Name to Remember:
`A10Q2_Hex_to_Decimal_P1_P2_P3` (Hex to Dec: Hundreds -> P1, Tens -> P2, Units -> P3)

### Answer WITHOUT Comments:
```c
#include <reg51.h>

void main(void) {
    unsigned char hex_num = 0xFD;
    unsigned char hundreds, tens, units;

    hundreds = hex_num / 100;
    tens = (hex_num % 100) / 10;
    units = hex_num % 10;

    P1 = hundreds;
    P2 = tens;
    P3 = units;

    while (1);
}
```

### Answer WITH Comments:
```c
#include <reg51.h>

void main(void) {
    unsigned char hex_num = 0xFD; // Example input: 0xFD = 253 in decimal
    unsigned char hundreds, tens, units;

    hundreds = hex_num / 100;         // 253 / 100 = 2
    tens = (hex_num % 100) / 10;      // (253 % 100) / 10 = 53 / 10 = 5
    units = hex_num % 10;             // 253 % 10 = 3

    P1 = hundreds;                    // Display hundreds digit (2) on Port 1
    P2 = tens;                        // Display tens digit (5) on Port 2
    P3 = units;                       // Display units digit (3) on Port 3

    while (1);                        // Halt
}
```

---

## Assignment 10 — Question 3
### Proper Question Title:
**Prime Number Verification from Port P0 with Conditional Display on Ports P1/P3 and Status LEDs**

### Short Name to Remember:
`A10Q3_Prime_Check_P0` (Read P0: Prime -> P1 & P2.0; Non-prime -> P3 & P2.2)

### Answer WITHOUT Comments:
```c
#include <reg51.h>

sbit LED_PRIME = P2^0;
sbit LED_NOT_PRIME = P2^2;

unsigned char is_prime(unsigned char n) {
    unsigned char i;
    if (n < 2) return 0;
    for (i = 2; i * i <= n; i++) {
        if (n % i == 0)
            return 0;
    }
    return 1;
}

void main(void) {
    unsigned char num;
    P0 = 0xFF;

    while (1) {
        num = P0;
        if (is_prime(num)) {
            P1 = num;
            LED_PRIME = 1;
            P3 = 0x00;
            LED_NOT_PRIME = 0;
        } else {
            P3 = num;
            LED_NOT_PRIME = 1;
            P1 = 0x00;
            LED_PRIME = 0;
        }
    }
}
```

### Answer WITH Comments:
```c
#include <reg51.h>

sbit LED_PRIME = P2^0;      // Indicator LED for Prime number on P2.0
sbit LED_NOT_PRIME = P2^2;  // Indicator LED for Not Prime number on P2.2

// Function to check primality of an 8-bit unsigned integer
unsigned char is_prime(unsigned char n) {
    unsigned char i;
    if (n < 2) return 0;    // 0 and 1 are not prime numbers
    for (i = 2; i * i <= n; i++) {
        if (n % i == 0)
            return 0;       // Divisible by i -> not prime
    }
    return 1;               // No divisors found -> prime
}

void main(void) {
    unsigned char num;
    P0 = 0xFF;              // Set Port 0 as input

    while (1) {
        num = P0;           // Read number from Port 0 DIP switch
        if (is_prime(num)) {
            P1 = num;            // Display number on Port 1
            LED_PRIME = 1;       // Turn ON P2.0 indicator LED
            P3 = 0x00;           // Turn OFF unused Port 3
            LED_NOT_PRIME = 0;   // Turn OFF non-prime indicator
        } else {
            P3 = num;            // Display number on Port 3
            LED_NOT_PRIME = 1;   // Turn ON P2.2 indicator LED
            P1 = 0x00;           // Turn OFF unused Port 1
            LED_PRIME = 0;       // Turn OFF prime indicator
        }
    }
}
```

---

## Assignment 10 — Question 4
### Proper Question Title:
**Continuous Serial Transmission of String "YES" at 9600 Baud (8-N-1) in Embedded C**

### Short Name to Remember:
`A10Q4_Serial_TX_YES_9600` (Continuous serial transmit "YES" at 9600 baud, 8-N-1)

### Answer WITHOUT Comments:
```c
#include <reg51.h>

void uart_init(void) {
    TMOD = 0x20;
    TH1 = 0xFD;
    SCON = 0x50;
    TR1 = 1;
}

void uart_tx_char(char c) {
    SBUF = c;
    while (TI == 0);
    TI = 0;
}

void uart_tx_string(char *str) {
    while (*str) {
        uart_tx_char(*str++);
    }
}

void delay(void) {
    unsigned int i, j;
    for (i = 0; i < 200; i++)
        for (j = 0; j < 120; j++);
}

void main(void) {
    uart_init();
    while (1) {
        uart_tx_string("YES ");
        delay();
    }
}
```

### Answer WITH Comments:
```c
#include <reg51.h>

// Initialize 8051 UART for 9600 Baud, 8-bit Data, 1 Stop Bit (XTAL = 11.0592 MHz)
void uart_init(void) {
    TMOD = 0x20;    // Timer 1 in Mode 2 (8-bit auto-reload timer)
    TH1 = 0xFD;     // Reload value for 9600 baud (256 - 3 = 253 = 0xFD)
    SCON = 0x50;    // Mode 1: 8-bit UART, REN = 1 (Receive Enabled)
    TR1 = 1;        // Start Timer 1
}

// Transmit a single character over UART
void uart_tx_char(char c) {
    SBUF = c;       // Load character into Serial Buffer
    while (TI == 0);// Wait until hardware sets Transmit Interrupt (TI) flag
    TI = 0;         // Clear TI flag manually for next transmission
}

// Transmit a null-terminated string over UART
void uart_tx_string(char *str) {
    while (*str) {
        uart_tx_char(*str++);
    }
}

void delay(void) {
    unsigned int i, j;
    for (i = 0; i < 200; i++)
        for (j = 0; j < 120; j++);
}

void main(void) {
    uart_init();    // Setup serial parameters
    while (1) {
        uart_tx_string("YES "); // Transmit "YES" continuously
        delay();                // Spacing delay between messages
    }
}
```

### Baud Rate Math (Crucial for Viva):
$$\text{Baud Rate} = \frac{1}{32} \times \frac{\text{Timer 1 Overflow Rate}}{1}$$
$$\text{Timer 1 Frequency} = \frac{11.0592\text{ MHz}}{12} = 921.6\text{ kHz}$$
$$\text{Machine Cycles per Bit} = \frac{921.6\text{ kHz}}{32 \times 9600} = 3\text{ counts}$$
$$\text{TH1} = 256 - 3 = 253 = \text{0xFD}$$

---

## Assignment 10 — Question 5
### Proper Question Title:
**Switch-Controlled Serial Transmission of First Name / Last Name at 9600 Baud**

### Short Name to Remember:
`A10Q5_Serial_TX_Name_SW` (SW at P2.0: 0 -> First Name, 1 -> Last Name at 9600 baud)

### Answer WITHOUT Comments:
```c
#include <reg51.h>

sbit SW = P2^0;

void uart_init(void) {
    TMOD = 0x20;
    TH1 = 0xFD;
    SCON = 0x50;
    TR1 = 1;
}

void uart_tx_char(char c) {
    SBUF = c;
    while (TI == 0);
    TI = 0;
}

void uart_tx_string(char *str) {
    while (*str) {
        uart_tx_char(*str++);
    }
}

void delay_ms(unsigned int ms) {
    unsigned int i, j;
    for (i = 0; i < ms; i++)
        for (j = 0; j < 120; j++);
}

void main(void) {
    SW = 1;
    uart_init();

    while (1) {
        if (SW == 0) {
            uart_tx_string("Gaurav\r\n");
        } else {
            uart_tx_string("Kumar\r\n");
        }
        delay_ms(500);
    }
}
```

### Answer WITH Comments:
```c
#include <reg51.h>

sbit SW = P2^0;     // Decision switch connected to Pin P2.0

void uart_init(void) {
    TMOD = 0x20;    // Timer 1 Mode 2 (8-bit auto-reload)
    TH1 = 0xFD;     // 9600 baud at 11.0592 MHz
    SCON = 0x50;    // 8-bit UART, REN = 1
    TR1 = 1;        // Start Timer 1
}

void uart_tx_char(char c) {
    SBUF = c;
    while (TI == 0);
    TI = 0;
}

void uart_tx_string(char *str) {
    while (*str) {
        uart_tx_char(*str++);
    }
}

void delay_ms(unsigned int ms) {
    unsigned int i, j;
    for (i = 0; i < ms; i++)
        for (j = 0; j < 120; j++);
}

void main(void) {
    SW = 1;         // Configure P2.0 as input pin
    uart_init();    // Initialize serial communication

    while (1) {
        if (SW == 0) {
            uart_tx_string("Gaurav\r\n"); // SW = 0: send First Name
        } else {
            uart_tx_string("Kumar\r\n");  // SW = 1: send Last Name
        }
        delay_ms(500); // Wait 500ms before transmitting again
    }
}
```

---

# ASSIGNMENT 11: Timers, Hardware Counters & LCD Interfacing

---

## Assignment 11 — Question 1
### Proper Question Title:
**Generation of 2 kHz Square Wave on Port 2 (P2.0) Using Timer 1 Mode 2 Auto-Reload at XTAL = 12 MHz**

### Short Name to Remember:
`A11Q1_Square_Wave_2kHz_Mode2` (2 kHz square wave on P2.0; Timer 1 Mode 2; XTAL=12MHz)

### Answer WITHOUT Comments:
```c
#include <reg51.h>

sbit WAVE_OUT = P2^0;

void main(void) {
    TMOD = 0x20;
    TH1 = 0x06;
    TL1 = 0x06;
    TR1 = 1;

    while (1) {
        while (TF1 == 0);
        TF1 = 0;
        WAVE_OUT = ~WAVE_OUT;
    }
}
```

### Answer WITH Comments:
```c
#include <reg51.h>

sbit WAVE_OUT = P2^0; // Output pin for square wave

void main(void) {
    // Clock calculation:
    // f_wave = 2 kHz -> Period T = 1 / 2000 = 500 us
    // Square wave has 50% duty cycle -> Half-period delay = 250 us
    // Crystal = 12 MHz -> 1 machine cycle = 12 / 12 MHz = 1.0 us
    // Ticks required = 250 us / 1 us = 250 ticks
    // Mode 2 (8-bit auto-reload): Reload value = 256 - 250 = 6 = 0x06
    TMOD = 0x20;    // Timer 1 in Mode 2 (8-bit auto reload)
    TH1 = 0x06;     // Auto-reload value = 0x06
    TL1 = 0x06;     // Initial value = 0x06
    TR1 = 1;        // Start Timer 1

    while (1) {
        while (TF1 == 0);   // Wait until Timer 1 rolls over from FFH to 06H (sets TF1)
        TF1 = 0;            // Clear Timer 1 overflow flag manually
        WAVE_OUT = ~WAVE_OUT; // Toggle output pin to form square wave edges
    }
}
```

### Oscilloscope Setup in Proteus:
1. Connect Channel A probe to `P2.0` (Pin 21).
2. Set Time/Div to `0.1 ms`.
3. You will observe one full cycle occupies exactly 5 divisions = $5 \times 0.1\text{ ms} = 0.5\text{ ms} = 2\text{ kHz}$.

---

## Assignment 11 — Question 2
### Proper Question Title:
**Generation of 1 kHz Square Wave on Port 2 (P2.0) Using Timer 1 Mode 1 (16-Bit Timer) at XTAL = 12 MHz**

### Short Name to Remember:
`A11Q2_Square_Wave_1kHz_Mode1` (1 kHz square wave on P2.0; Timer 1 Mode 1 16-bit; XTAL=12MHz)

### Answer WITHOUT Comments:
```c
#include <reg51.h>

sbit WAVE_OUT = P2^0;

void main(void) {
    TMOD = 0x10;

    while (1) {
        TH1 = 0xFE;
        TL1 = 0x0C;
        TR1 = 1;
        while (TF1 == 0);
        TR1 = 0;
        TF1 = 0;
        WAVE_OUT = ~WAVE_OUT;
    }
}
```

### Answer WITH Comments:
```c
#include <reg51.h>

sbit WAVE_OUT = P2^0;

void main(void) {
    // Frequency = 1 kHz -> Period = 1 ms = 1000 us
    // Half cycle delay = 500 us
    // Machine cycle at 12 MHz = 1.0 us -> 500 counts required
    // 16-bit timer reload: 65536 - 500 = 65036 = 0xFE0C
    // TH1 = 0xFE, TL1 = 0x0C
    TMOD = 0x10;    // Timer 1 in Mode 1 (16-bit timer)

    while (1) {
        TH1 = 0xFE;         // Load high byte
        TL1 = 0x0C;         // Load low byte
        TR1 = 1;            // Start Timer 1
        while (TF1 == 0);   // Wait for 500 us overflow
        TR1 = 0;            // Stop timer
        TF1 = 0;            // Clear overflow flag
        WAVE_OUT = ~WAVE_OUT; // Toggle output pin
    }
}
```

---

## Assignment 11 — Question 3
### Proper Question Title:
**Vehicle Traffic Counter Using Timer 1 in Counter Mode 1 (16-Bit) with "Lane Full" Indicator on P1.0**

### Short Name to Remember:
`A11Q3_Vehicle_Traffic_Counter` (T1 pin P3.5 counts vehicles; at 100 pulses -> P1.0 LED ON)

### Answer WITHOUT Comments:
```c
#include <reg51.h>

sbit LANE_FULL_LED = P1^0;

void main(void) {
    TMOD = 0x50;
    TH1 = 0xFF;
    TL1 = 0x9C;
    LANE_FULL_LED = 0;
    TR1 = 1;

    while (TF1 == 0);

    TR1 = 0;
    TF1 = 0;
    LANE_FULL_LED = 1;

    while (1);
}
```

### Answer WITH Comments:
```c
#include <reg51.h>

sbit LANE_FULL_LED = P1^0; // Lane Full Indicator LED connected to Pin 1 (P1.0)

void main(void) {
    // Counter Configuration:
    // Timer 1 in Counter Mode (C/T = 1) -> Increments on 1-to-0 transition at Pin T1 (P3.5 / Pin 15)
    // Mode 1 (16-bit counter):
    // TMOD = 0x50 (Timer 1: GATE=0, C/T=1, M1=0, M0=1)
    // Target = 100 vehicle pulses
    // Initial value = 65536 - 100 = 65436 = 0xFF9C
    // TH1 = 0xFF, TL1 = 0x9C
    TMOD = 0x50;            // Timer 1, Counter mode, 16-bit Mode 1
    TH1 = 0xFF;             // High byte
    TL1 = 0x9C;             // Low byte
    LANE_FULL_LED = 0;      // LED initially OFF
    TR1 = 1;                // Start counting external pulses on T1 (P3.5)

    while (TF1 == 0);       // Wait until 100 vehicle pulses arrive (sets TF1)

    TR1 = 0;                // Stop counter
    TF1 = 0;                // Clear overflow flag
    LANE_FULL_LED = 1;      // Turn ON "Lane Full" indicator LED!

    while (1);              // Latch state until system reset
}
```

### Proteus Circuit Setup:
- Connect VSM **SIGNAL GENERATOR** (Square wave, 10 Hz) to `Pin 15` (`P3.5 / T1`).
- Connect `LED-RED` to `Pin 1` (`P1.0`) with a 330 ohm series resistor to `GND`.
- Watch the counter tick: on the 100th pulse, the LED turns ON!

---

## Assignment 11 — Question 4
### Proper Question Title:
**16x2 Alphanumeric LCD (LM016L) 8-Bit Interfacing and Display of "MNNIT Allahabad"**

### Short Name to Remember:
`A11Q4_LCD_16x2_MNNIT_Allahabad` (LM016L LCD 8-bit mode: Display "MNNIT Allahabad" on Line 1)

### Answer WITHOUT Comments:
```c
#include <reg51.h>

#define LCD_DATA P3
sbit RS = P2^0;
sbit RW = P2^1;
sbit E  = P2^2;

void delay_ms(unsigned int ms) {
    unsigned int i, j;
    for (i = 0; i < ms; i++)
        for (j = 0; j < 120; j++);
}

void lcd_cmd(unsigned char cmd) {
    LCD_DATA = cmd;
    RS = 0;
    RW = 0;
    E = 1;
    delay_ms(2);
    E = 0;
}

void lcd_data_write(unsigned char dat) {
    LCD_DATA = dat;
    RS = 1;
    RW = 0;
    E = 1;
    delay_ms(2);
    E = 0;
}

void lcd_init(void) {
    delay_ms(20);
    lcd_cmd(0x38);
    lcd_cmd(0x0C);
    lcd_cmd(0x06);
    lcd_cmd(0x01);
    delay_ms(5);
}

void lcd_print(char *str) {
    while (*str) {
        lcd_data_write(*str++);
    }
}

void main(void) {
    lcd_init();
    lcd_cmd(0x80);
    lcd_print("MNNIT Allahabad");
    while (1);
}
```

### Answer WITH Comments:
```c
#include <reg51.h>

#define LCD_DATA P3     // LCD Data Bus D0-D7 connected to Port 3 (Pins 10-17)
sbit RS = P2^0;         // Register Select: 0 = Command, 1 = Data
sbit RW = P2^1;         // Read/Write: 0 = Write, 1 = Read
sbit E  = P2^2;         // Enable Pin: Active High pulse latches data

void delay_ms(unsigned int ms) {
    unsigned int i, j;
    for (i = 0; i < ms; i++)
        for (j = 0; j < 120; j++);
}

// Send control command to LCD instruction register
void lcd_cmd(unsigned char cmd) {
    LCD_DATA = cmd;     // Put command byte onto Port 3
    RS = 0;             // RS = 0 for command
    RW = 0;             // RW = 0 for write
    E = 1;              // High-to-Low pulse on Enable pin
    delay_ms(2);
    E = 0;
}

// Send displayable ASCII character to LCD data register
void lcd_data_write(unsigned char dat) {
    LCD_DATA = dat;     // Put ASCII character onto Port 3
    RS = 1;             // RS = 1 for character data
    RW = 0;             // RW = 0 for write
    E = 1;              // Pulse Enable
    delay_ms(2);
    E = 0;
}

// Standard HD44780 LCD Initialization sequence
void lcd_init(void) {
    delay_ms(20);       // Wait > 15ms after VCC reaches 4.5V
    lcd_cmd(0x38);      // Function Set: 8-bit mode, 2 lines, 5x7 dot matrix
    lcd_cmd(0x0C);      // Display ON, Cursor OFF
    lcd_cmd(0x06);      // Entry Mode: Auto-increment cursor position
    lcd_cmd(0x01);      // Clear Display screen
    delay_ms(5);        // Clear display needs ~2ms execution time
}

// Print string on LCD
void lcd_print(char *str) {
    while (*str) {
        lcd_data_write(*str++);
    }
}

void main(void) {
    lcd_init();         // Initialize LCD controller
    lcd_cmd(0x80);      // Set DDRAM address to 00H (Row 1, Column 1)
    lcd_print("MNNIT Allahabad"); // Output string
    while (1);          // Halt
}
```

### Proteus Circuit Connections (Pin-for-Pin):
- `LM016L` LCD:
  - `VSS` (Pin 1) -> `GND`
  - `VDD` (Pin 2) -> `+5V (POWER)`
  - `VEE` (Pin 3) -> Wiper of 10k Potentiometer (`POT-HG`)
  - `RS` (Pin 4) -> `P2.0`
  - `RW` (Pin 5) -> `P2.1`
  - `E` (Pin 6) -> `P2.2`
  - `D0 - D7` (Pins 7-14) -> `P3.0 - P3.7`



# MODULE 2: Senior Remarks & Real Lab Exam Questions

---

# SECTION 2.1: A2 Embedded Lab Exam Questions

---

## Senior A2 — Question 1
### Proper Question Title:
**Generation of 2 kHz Square Wave Specifically on Pin P1.5 Using 8051 Hardware Timers**

### Short Name to Remember:
`A2Q1_Square_Wave_2kHz_P1_5` (2 kHz square wave on P1.5; XTAL = 12 MHz)

### Answer WITHOUT Comments (Assembly):
```assembly
    ORG 0000H
    MOV TMOD, #02H
    MOV TH0, #06H
    MOV TL0, #06H
    SETB TR0
LOOP:
    JNB TF0, LOOP
    CLR TF0
    CPL P1.5
    SJMP LOOP
    END
```

### Answer WITHOUT Comments (Embedded C):
```c
#include <reg51.h>

sbit WAVE = P1^5;

void main(void) {
    TMOD = 0x02;
    TH0 = 0x06;
    TL0 = 0x06;
    TR0 = 1;
    while (1) {
        while (TF0 == 0);
        TF0 = 0;
        WAVE = ~WAVE;
    }
}
```

### Answer WITH Comments (Assembly):
```assembly
    ORG 0000H           ; Origin
    ; Timing Math:
    ; f = 2 kHz -> T = 500 us -> Half cycle = 250 us
    ; Clock = 12 MHz -> 1 machine cycle = 1 us
    ; Timer counts needed = 250 us / 1 us = 250 counts
    ; Timer 0 Mode 2 (8-bit auto-reload): Reload = 256 - 250 = 6 = 06H
    MOV TMOD, #02H      ; Timer 0 in Mode 2 (8-bit auto reload)
    MOV TH0, #06H       ; Load reload value
    MOV TL0, #06H       ; Initialize low byte
    SETB TR0            ; Start Timer 0
LOOP:
    JNB TF0, LOOP       ; Poll Timer 0 overflow flag until it reaches 1
    CLR TF0             ; Clear overflow flag
    CPL P1.5            ; Toggle Pin P1.5 to create square wave edge
    SJMP LOOP           ; Repeat continuously
    END
```

### Answer WITH Comments (Embedded C):
```c
#include <reg51.h>

sbit WAVE = P1^5;       // Output assigned specifically to Pin 1.5

void main(void) {
    TMOD = 0x02;        // Timer 0, Mode 2 (8-bit auto-reload)
    TH0 = 0x06;         // Reload value for 250 us delay (at 12 MHz)
    TL0 = 0x06;         // Initial timer value
    TR0 = 1;            // Start Timer 0

    while (1) {
        while (TF0 == 0);   // Wait for 250 us overflow
        TF0 = 0;            // Reset overflow flag
        WAVE = ~WAVE;       // Invert Pin P1.5
    }
}
```

### Examiner Viva Questions & Potential Modifications:
- **"What if XTAL is 11.0592 MHz instead of 12 MHz?"**
  - *Calculation:* Machine cycle $= 12 / 11.0592\text{ MHz} = 1.085069\ \mu\text{s}$.
  - Ticks for $250\ \mu\text{s} = 250 / 1.085069 \approx 230$ counts.
  - Reload value $\text{TH0} = 256 - 230 = 26 = \text{0x1A}$.

---

## Senior A2 — Question 2
### Proper Question Title:
**Frequency Divider: Generation of a Wave of 1/4th the Frequency of an Incoming Wave at Pin INT0**

### Short Name to Remember:
`A2Q2_Freq_Divider_Div4_INT0` (Input at INT0 / P3.2 -> Output at P1.0 has freq = fin / 4)

### Answer WITHOUT Comments (Assembly):
```assembly
    ORG 0000H
    LJMP MAIN

    ORG 0003H
    LJMP ISR_INT0

    ORG 0030H
MAIN:
    MOV R2, #00H
    SETB IT0
    SETB EX0
    SETB EA
    SJMP $

ISR_INT0:
    INC R2
    CJNE R2, #02H, RET_INT0
    MOV R2, #00H
    CPL P1.0
RET_INT0:
    RETI
    END
```

### Answer WITHOUT Comments (Embedded C):
```c
#include <reg51.h>

sbit OUT_PIN = P1^0;
unsigned char edge_count = 0;

void ext0_isr(void) interrupt 0 {
    edge_count++;
    if (edge_count >= 2) {
        edge_count = 0;
        OUT_PIN = ~OUT_PIN;
    }
}

void main(void) {
    IT0 = 1;
    EX0 = 1;
    EA = 1;
    OUT_PIN = 0;
    while (1);
}
```

### Answer WITH Comments (Assembly):
```assembly
    ORG 0000H           ; Reset vector
    LJMP MAIN

    ORG 0003H           ; Interrupt Vector address for External Interrupt 0 (INT0)
    LJMP ISR_INT0

    ORG 0030H           ; Main program code
MAIN:
    MOV R2, #00H        ; Clear pulse counter in R2
    SETB IT0            ; Configure INT0 for falling-edge trigger (IT0 = 1)
    SETB EX0            ; Enable External Interrupt 0
    SETB EA             ; Global Interrupt Enable (EA = 1)
    SJMP $              ; Background loop (all work handled by interrupt)

; Interrupt Service Routine executed on every incoming falling edge at INT0 (P3.2)
ISR_INT0:
    INC R2              ; Count incoming pulse edge
    CJNE R2, #02H, RET_INT0 ; We need output wave period = 4 * incoming period
    ; Half period = 2 * incoming period
    ; Therefore, toggle output once every 2 incoming falling edges!
    MOV R2, #00H        ; Reset edge counter
    CPL P1.0            ; Toggle output pin P1.0
RET_INT0:
    RETI                ; Return from Interrupt
    END
```

### Answer WITH Comments (Embedded C):
```c
#include <reg51.h>

sbit OUT_PIN = P1^0;            // Output divided frequency pin
unsigned char edge_count = 0;   // Count incoming pulses

// External Interrupt 0 Service Routine (Vector 0003H, interrupt 0)
void ext0_isr(void) interrupt 0 {
    edge_count++;
    // If output toggles every 2 input cycles, a full output cycle takes 4 input cycles.
    // Thus: f_out = f_in / 4!
    if (edge_count >= 2) {
        edge_count = 0;         // Reset edge counter
        OUT_PIN = ~OUT_PIN;     // Toggle output pin
    }
}

void main(void) {
    IT0 = 1;            // Set INT0 (Pin P3.2) to transition-activated (falling edge triggered)
    EX0 = 1;            // Enable External Interrupt 0
    EA = 1;             // Enable global interrupts
    OUT_PIN = 0;        // Initial output state

    while (1);          // Microcontroller remains idle waiting for interrupt pulses
}
```

### Theoretical Principle & Signal Waveform Explanation:
- Let incoming wave have frequency $f_{\text{in}}$ and period $T_{\text{in}}$.
- We want output frequency $f_{\text{out}} = f_{\text{in}} / 4$, which means output period $T_{\text{out}} = 4 \times T_{\text{in}}$.
- A symmetric square wave has a High duration of $T_{\text{out}} / 2$ and Low duration of $T_{\text{out}} / 2$.
- Here: $\text{Half-period} = 4 \times T_{\text{in}} / 2 = 2 \times T_{\text{in}}$.
- Therefore, the microcontroller must detect **2 incoming falling edges**, then toggle the output pin!
- In Proteus: Connect `VSM SIGNAL GENERATOR` to Pin `P3.2` (`INT0`). Connect Channel A of Oscilloscope to `P3.2` and Channel B to `P1.0`. You will visually observe Channel B has exactly 1 cycle for every 4 cycles of Channel A!

---

# SECTION 2.2: D2 Embedded Lab Exam Questions

---

## Senior D2 — Question 1
### Proper Question Title:
**Writing Numbers 0 to 9 Starting from RAM Location 200H, Fetching Sequentially and Sending to Port P1 in 8051 Assembly**

### Short Name to Remember:
`D2Q1_RAM_200_to_Port_P1` (Write 0-9 into ext RAM 0200H, read back and send to P1)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV DPTR, #0200H
    MOV R2, #10
    MOV A, #00H
WRITE_LOOP:
    MOVX @DPTR, A
    INC A
    INC DPTR
    DJNZ R2, WRITE_LOOP

    MOV DPTR, #0200H
    MOV R2, #10
READ_LOOP:
    MOVX A, @DPTR
    MOV P1, A
    ACALL DELAY
    INC DPTR
    DJNZ R2, READ_LOOP

    SJMP $

DELAY:
    MOV R6, #05H
D1:
    MOV R5, #0FFH
D2:
    MOV R4, #0FFH
D3:
    DJNZ R4, D3
    DJNZ R5, D2
    DJNZ R6, D1
    RET
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    ; Part 1: Write numbers 0 to 9 into RAM location starting at 0200H
    MOV DPTR, #0200H    ; Load DPTR with RAM address 0200H
    MOV R2, #10         ; Total numbers = 10 (0 through 9)
    MOV A, #00H         ; Start with value 0
WRITE_LOOP:
    MOVX @DPTR, A       ; Write value in A into external RAM at @DPTR
    INC A               ; Increment value (0 -> 1 -> 2 ... 9)
    INC DPTR            ; Advance DPTR to next memory location
    DJNZ R2, WRITE_LOOP ; Repeat 10 times

    ; Part 2: Fetch numbers one by one and send to Port P1 with visible delay
    MOV DPTR, #0200H    ; Reset DPTR back to base location 0200H
    MOV R2, #10         ; Counter = 10
READ_LOOP:
    MOVX A, @DPTR       ; Read number from RAM into Accumulator
    MOV P1, A           ; Send number to Port 1 (LEDs/Display)
    ACALL DELAY         ; Hold output so number can be observed
    INC DPTR            ; Point to next RAM location
    DJNZ R2, READ_LOOP  ; Repeat for all 10 numbers

    SJMP $              ; Stop

DELAY:
    MOV R6, #05H        ; Delay routine (~0.5 second)
D1:
    MOV R5, #0FFH
D2:
    MOV R4, #0FFH
D3:
    DJNZ R4, D3
    DJNZ R5, D2
    DJNZ R6, D1
    RET
    END
```

---

## Senior D2 — Question 2
### Proper Question Title:
**Determination of the Number of 1's (Set Bits) in a 16-Bit Number in Embedded C**

### Short Name to Remember:
`D2Q2_Count_Ones_16Bit` (Count 1s in 16-bit number in Embedded C; output count on P1)

### Answer WITHOUT Comments:
```c
#include <reg51.h>

unsigned char count_ones(unsigned int val) {
    unsigned char count = 0;
    while (val > 0) {
        if (val & 0x0001)
            count++;
        val >>= 1;
    }
    return count;
}

void main(void) {
    unsigned int number = 0xA5C3;
    unsigned char ones;

    ones = count_ones(number);
    P1 = ones;

    while (1);
}
```

### Answer WITH Comments:
```c
#include <reg51.h>

// Function to count total set bits ('1's) in a 16-bit unsigned integer
unsigned char count_ones(unsigned int val) {
    unsigned char count = 0;
    while (val > 0) {
        if (val & 0x0001)   // Check if the Least Significant Bit is 1
            count++;        // Increment set bit counter
        val >>= 1;          // Right shift by 1 bit to inspect next bit
    }
    return count;
}

void main(void) {
    // Example 16-bit test number: 0xA5C3
    // Binary: 1010 0101 1100 0011 -> Exactly 8 ones
    unsigned int number = 0xA5C3;
    unsigned char ones;

    ones = count_ones(number); // Calculate 1's count
    P1 = ones;                 // Display result count on Port 1 LEDs (P1 = 0x08)

    while (1);                 // Halt
}
```

### Brian Kernighan's Fast Algorithm Variation (High-Scoring Viva Alternative):
```c
unsigned char count_ones_fast(unsigned int val) {
    unsigned char count = 0;
    while (val) {
        val &= (val - 1); // Clears the lowest set bit in each iteration
        count++;
    }
    return count;
}
```

---

## Senior D2 — Question 3
### Proper Question Title:
**16-Bit Multiplication in Embedded C and Assembly Language**

### Short Name to Remember:
`D2Q3_16Bit_Multiplication_All` (16-bit x 16-bit multiplication in Embedded C & Assembly)

### Answer WITHOUT Comments (Embedded C):
```c
#include <reg51.h>

void main(void) {
    unsigned int a = 0x1234;
    unsigned int b = 0x0056;
    unsigned long product;

    product = (unsigned long)a * (unsigned long)b;

    P0 = (unsigned char)(product & 0xFF);
    P1 = (unsigned char)((product >> 8) & 0xFF);
    P2 = (unsigned char)((product >> 16) & 0xFF);
    P3 = (unsigned char)((product >> 24) & 0xFF);

    while (1);
}
```

### Answer WITH Comments (Embedded C):
```c
#include <reg51.h>

void main(void) {
    unsigned int a = 0x1234;        // 16-bit operand 1
    unsigned int b = 0x0056;        // 16-bit operand 2
    unsigned long product;          // 32-bit container in Keil C51

    // Typecast to 32-bit before multiplication to prevent 16-bit truncation overflow
    product = (unsigned long)a * (unsigned long)b;

    // Distribute 32-bit product across all 4 ports:
    P0 = (unsigned char)(product & 0xFF);           // Byte 0 (Least Significant Byte)
    P1 = (unsigned char)((product >> 8) & 0xFF);    // Byte 1
    P2 = (unsigned char)((product >> 16) & 0xFF);   // Byte 2
    P3 = (unsigned char)((product >> 24) & 0xFF);   // Byte 3 (Most Significant Byte)

    while (1);
}
```

---

## Senior D2 — Question 4
### Proper Question Title:
**Serial Transmission of String "Welcome to MNNIT Allahabad" at 4800 Baud Rate**

### Short Name to Remember:
`D2Q4_Serial_TX_MNNIT_4800` (Serial TX "Welcome to MNNIT Allahabad" at 4800 baud; TH1=0xFA)

### Answer WITHOUT Comments (Embedded C):
```c
#include <reg51.h>

void uart_init_4800(void) {
    TMOD = 0x20;
    TH1 = 0xFA;
    SCON = 0x50;
    TR1 = 1;
}

void uart_send_char(char c) {
    SBUF = c;
    while (TI == 0);
    TI = 0;
}

void uart_send_string(char *s) {
    while (*s) {
        uart_send_char(*s++);
    }
}

void main(void) {
    uart_init_4800();
    uart_send_string("Welcome to MNNIT Allahabad\r\n");
    while (1);
}
```

### Answer WITH Comments (Embedded C):
```c
#include <reg51.h>

// Initialize 8051 UART for 4800 Baud Rate (XTAL = 11.0592 MHz)
void uart_init_4800(void) {
    // Math for 4800 Baud:
    // Timer 1 overflow clock = 11.0592 MHz / 12 = 921,600 Hz
    // T1 clock for UART = 921,600 / 32 = 28,800 Hz
    // Reload count = 28,800 / 4800 = 6 counts
    // TH1 = 256 - 6 = 250 = 0xFA (or -6)
    TMOD = 0x20;    // Timer 1 in Mode 2 (8-bit auto-reload)
    TH1 = 0xFA;     // 4800 baud reload value
    SCON = 0x50;    // Mode 1: 8-bit UART, Receiver enabled
    TR1 = 1;        // Start Timer 1
}

void uart_send_char(char c) {
    SBUF = c;       // Load character into UART buffer
    while (TI == 0);// Wait until transmit interrupt flag is set
    TI = 0;         // Clear TI flag manually
}

void uart_send_string(char *s) {
    while (*s) {
        uart_send_char(*s++);
    }
}

void main(void) {
    uart_init_4800(); // Set up serial port at 4800 baud
    uart_send_string("Welcome to MNNIT Allahabad\r\n"); // Transmit string
    while (1);        // Halt
}
```

### Answer in 8051 Assembly Language:
```assembly
    ORG 0000H
    MOV TMOD, #20H      ; Timer 1 Mode 2 (8-bit auto-reload)
    MOV TH1, #0FAH      ; 4800 baud rate reload value (256 - 6 = 250 = FAH)
    MOV SCON, #50H      ; 8-bit UART, REN = 1
    SETB TR1            ; Start Timer 1
    MOV DPTR, #MSG
SEND_NEXT:
    CLR A
    MOVC A, @A+DPTR
    JZ HALT
    MOV SBUF, A
WAIT_TI:
    JNB TI, WAIT_TI
    CLR TI
    INC DPTR
    SJMP SEND_NEXT
HALT:
    SJMP $

    ORG 0200H
MSG:
    DB "Welcome to MNNIT Allahabad", 0DH, 0AH, 0
    END
```

---

# SECTION 2.3: Mazidi Solved Codes — Chapters 7, 10 & 11

---

## Mazidi Chapter 7: Timer/Counter Programming

### Example 1: 16-Bit Timer Delay Generation in Mode 1 (5 ms Delay)
```c
#include <reg51.h>

// Generate 5 ms delay using Timer 0 in Mode 1 (XTAL = 11.0592 MHz)
// 1 machine cycle = 1.085 us
// Number of counts = 5 ms / 1.085 us = 4608 counts
// Reload value = 65536 - 4608 = 60928 = 0xEE00
void delay_5ms(void) {
    TMOD &= 0xF0;   // Preserve Timer 1
    TMOD |= 0x01;   // Timer 0 in Mode 1 (16-bit timer)
    TL0 = 0x00;     // Load TL0
    TH0 = 0xEE;     // Load TH0
    TR0 = 1;        // Start Timer 0
    while (TF0 == 0); // Wait for overflow
    TR0 = 0;        // Stop Timer 0
    TF0 = 0;        // Clear overflow flag
}
```

### Example 2: External Event Counter Using Timer 0 (Counter Mode 2 on Pin T0 / P3.4)
```assembly
    ORG 0000H
    ; Configure Timer 0 as 8-bit auto-reload COUNTER (C/T = 1, Mode 2)
    ; Counts pulses on Pin T0 (P3.4) and outputs count on Port P1
    MOV TMOD, #06H      ; 0000 0110B (GATE=0, C/T=1, M1=1, M0=0)
    MOV TH0, #00H       ; Reload with 00H
    MOV TL0, #00H       ; Clear counter register
    SETB TR0            ; Start Counter 0
COUNT_MONITOR:
    MOV A, TL0          ; Read current count value from TL0
    MOV P1, A           ; Output count to Port 1 LEDs
    SJMP COUNT_MONITOR  ; Continuously monitor external events
    END
```

---

## Mazidi Chapter 10: 8051 Serial Communication

### Baud Rate Reload Values Table (XTAL = 11.0592 MHz, Timer 1 Mode 2):
| Baud Rate | SMOD Bit (PCON.7) | TH1 Reload (Decimal) | TH1 Reload (Hex) |
|---|---|---|---|
| **9600** | 0 | -3 | `0xFD` |
| **4800** | 0 | -6 | `0xFA` |
| **2400** | 0 | -12 | `0xF4` |
| **1200** | 0 | -24 | `0xE8` |
| **19200** | 1 (Doubled) | -3 | `0xFD` |

### Bidirectional Polled Serial Communication (Echo Receiver):
```c
#include <reg51.h>

void main(void) {
    unsigned char ch;
    TMOD = 0x20;    // Timer 1 Mode 2
    TH1 = 0xFD;     // 9600 baud
    SCON = 0x50;    // 8-bit UART, REN = 1
    TR1 = 1;        // Start Timer 1

    while (1) {
        while (RI == 0);    // Wait for incoming character
        ch = SBUF;          // Read received character
        RI = 0;             // Clear receive flag

        SBUF = ch;          // Echo character back to sender
        while (TI == 0);    // Wait until character transmitted
        TI = 0;             // Clear transmit flag
    }
}
```

---

## Mazidi Chapter 11: 8051 Interrupt Programming

### Standard 8051 Interrupt Vector Table:
| Interrupt Source | Flag | Vector Address | Priority Order |
|---|---|---|---|
| **Reset** | Pin RST | `0000H` | Highest |
| **External Interrupt 0** | `IE0` (P3.2) | `0003H` | 1 |
| **Timer 0 Overflow** | `TF0` | `000BH` | 2 |
| **External Interrupt 1** | `IE1` (P3.3) | `0013H` | 3 |
| **Timer 1 Overflow** | `TF1` | `001BH` | 4 |
| **Serial Port (UART)** | `TI` / `RI` | `0023H` | 5 (Lowest) |

### Timer 0 Interrupt Generating Square Wave While Main Executes Foreground Task:
```c
#include <reg51.h>

sbit SQUARE_WAVE = P1^0;

// Timer 0 Interrupt Service Routine (Vector 000BH)
void timer0_isr(void) interrupt 1 {
    SQUARE_WAVE = ~SQUARE_WAVE; // Toggle square wave pin
}

void delay(void) {
    unsigned int i, j;
    for (i = 0; i < 200; i++)
        for (j = 0; j < 120; j++);
}

void main(void) {
    TMOD = 0x02;        // Timer 0 Mode 2 (8-bit auto-reload)
    TH0 = 0x06;         // Reload value
    TL0 = 0x06;
    ET0 = 1;            // Enable Timer 0 Interrupt
    EA = 1;             // Enable Global Interrupts
    TR0 = 1;            // Start Timer 0

    // Foreground task: blinking LEDs on Port 2 while Timer 0 generates wave in background!
    while (1) {
        P2 = 0x55;
        delay();
        P2 = 0xAA;
        delay();
    }
}
```

---

# SECTION 2.4: B1 Lab Exam & 'Lab Questions.pdf' Hotlist

---

## B1 Hotlist — Question 1
### Proper Question Title:
**Finding the Largest and Smallest Numbers in an Array of 10 Bytes in Internal RAM**

### Short Name to Remember:
`B1Q1_Max_Min_Array` (Find Largest -> RAM 50H, Smallest -> RAM 51H)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV R0, #30H
    MOV R2, #09H
    MOV A, @R0
    MOV 50H, A
    MOV 51H, A
SCAN_LOOP:
    INC R0
    MOV A, @R0
    MOV B, A
    CLR C
    SUBB A, 50H
    JC CHECK_MIN
    MOV 50H, B
CHECK_MIN:
    MOV A, B
    CLR C
    SUBB A, 51H
    JNC SKIP_ELEM
    MOV 51H, B
SKIP_ELEM:
    DJNZ R2, SCAN_LOOP
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV R0, #30H        ; Pointer to array starting at 30H
    MOV R2, #09H        ; 9 comparisons for 10 elements
    MOV A, @R0          ; Initialize both Max and Min with first array element
    MOV 50H, A          ; 50H stores Maximum element
    MOV 51H, A          ; 51H stores Minimum element
SCAN_LOOP:
    INC R0              ; Point to next element
    MOV A, @R0          ; Load current element into A
    MOV B, A            ; Buffer in B
    CLR C
    SUBB A, 50H         ; A - Max: if Carry (CY=1), current < Max
    JC CHECK_MIN        ; Skip if not larger than current Max
    MOV 50H, B          ; Update new Maximum!
CHECK_MIN:
    MOV A, B            ; Reload current element
    CLR C
    SUBB A, 51H         ; A - Min: if Carry (CY=1), current < Min
    JNC SKIP_ELEM       ; Skip if not smaller than current Min
    MOV 51H, B          ; Update new Minimum!
SKIP_ELEM:
    DJNZ R2, SCAN_LOOP  ; Repeat for all remaining elements
    SJMP $              ; Result: Max at 50H, Min at 51H
    END
```

---

## B1 Hotlist — Question 2
### Proper Question Title:
**Linear Search in an Array: Return Index if Key Found, Otherwise Store FFH**

### Short Name to Remember:
`B1Q2_Linear_Search` (Search for key in array; index in R3, FFH if not found)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV R0, #30H
    MOV R2, #10
    MOV R3, #00H
    MOV B, #45H
SEARCH_LOOP:
    MOV A, @R0
    CJNE A, B, NOT_MATCH
    MOV A, R3
    MOV 50H, A
    SJMP DONE
NOT_MATCH:
    INC R0
    INC R3
    DJNZ R2, SEARCH_LOOP
    MOV 50H, #0FFH
DONE:
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV R0, #30H        ; Array base pointer at 30H
    MOV R2, #10         ; Array length = 10 elements
    MOV R3, #00H        ; Index tracker (0 to 9)
    MOV B, #45H         ; Search key (e.g., 45H)
SEARCH_LOOP:
    MOV A, @R0          ; Read element from array
    CJNE A, B, NOT_MATCH; Compare with key: branch if not equal
    ; Key found!
    MOV A, R3           ; Load matching index
    MOV 50H, A          ; Store index at RAM 50H
    SJMP DONE           ; Exit
NOT_MATCH:
    INC R0              ; Advance memory pointer
    INC R3              ; Advance index count
    DJNZ R2, SEARCH_LOOP; Continue searching
    MOV 50H, #0FFH      ; Key not found in array: store FFH at 50H
DONE:
    SJMP $              ; Halt
    END
```

---

## B1 Hotlist — Question 3
### Proper Question Title:
**Block Exchange: Swapping Two 10-Byte Blocks in Internal RAM**

### Short Name to Remember:
`B1Q3_Block_Exchange` (Swap 10 bytes between 30H..39H and 40H..49H)

### Answer WITHOUT Comments:
```assembly
    ORG 0000H
    MOV R0, #30H
    MOV R1, #40H
    MOV R2, #10
SWAP_LOOP:
    MOV A, @R0
    XCH A, @R1
    MOV @R0, A
    INC R0
    INC R1
    DJNZ R2, SWAP_LOOP
    SJMP $
    END
```

### Answer WITH Comments:
```assembly
    ORG 0000H           ; Origin
    MOV R0, #30H        ; Pointer to Block 1 (30H)
    MOV R1, #40H        ; Pointer to Block 2 (40H)
    MOV R2, #10         ; 10 bytes to swap
SWAP_LOOP:
    MOV A, @R0          ; Load byte from Block 1 into A
    XCH A, @R1          ; Atomically swap A with byte in Block 2 (@R1)
    ; Now A holds original byte from Block 2, and Block 2 has original byte from Block 1!
    MOV @R0, A          ; Write byte into Block 1 (@R0)
    INC R0              ; Advance Block 1 pointer
    INC R1              ; Advance Block 2 pointer
    DJNZ R2, SWAP_LOOP  ; Loop until all 10 bytes are exchanged
    SJMP $              ; Halt
    END
```

---

## B1 Hotlist — Question 4
### Proper Question Title:
**DAC 0808 Interfacing: Generating Sawtooth and Triangular Waveforms**

### Short Name to Remember:
`B1Q4_DAC_Waveforms` (Sawtooth & Triangular wave generation on Port 1 via DAC)

### Answer WITHOUT Comments (Sawtooth Wave):
```assembly
    ORG 0000H
SAW_LOOP:
    MOV A, #00H
RAMP:
    MOV P1, A
    INC A
    JNZ RAMP
    SJMP SAW_LOOP
    END
```

### Answer WITH Comments (Triangular Wave in Embedded C):
```c
#include <reg51.h>

void main(void) {
    unsigned char val;
    while (1) {
        // Ramp UP
        for (val = 0; val < 255; val++) {
            P1 = val; // Output to DAC 0808 inputs
        }
        // Ramp DOWN
        for (val = 255; val > 0; val--) {
            P1 = val; // Output to DAC 0808 inputs
        }
    }
}
```

---

## B1 Hotlist — Question 5
### Proper Question Title:
**Stepper Motor Interfacing: Clockwise and Counter-Clockwise Continuous Rotation**

### Short Name to Remember:
`B1Q5_Stepper_Motor_Control` (Rotate stepper motor CW / CCW using step sequence)

### Answer WITH Comments (Embedded C):
```c
#include <reg51.h>

// 4-step sequence for 4-phase unipolar stepper motor (Port 1 pins P1.0 - P1.3)
// Step sequence: 0x09 (1001b), 0x0C (1100b), 0x06 (0110b), 0x03 (0011b)
unsigned char step_seq[4] = {0x09, 0x0C, 0x06, 0x03};

void delay_ms(unsigned int ms) {
    unsigned int i, j;
    for (i = 0; i < ms; i++)
        for (j = 0; j < 120; j++);
}

void main(void) {
    unsigned char i;
    while (1) {
        // Clockwise Rotation
        for (i = 0; i < 4; i++) {
            P1 = step_seq[i];
            delay_ms(50); // Speed control delay
        }
        // To rotate Counter-Clockwise: cycle sequence in reverse (3 down to 0)
    }
}
```



# MODULE 3: Embedded Systems Lab Quiz & Viva Master Vault

---

# SECTION 3.1: 8051 Microcontroller Architecture & Memory Map

```
+-------------------------------------------------------------------------+
|                         8051 MEMORY ARCHITECTURE                        |
+------------------------------------+------------------------------------+
|         PROGRAM ROM (4KB)          |          DATA RAM (128B)           |
+------------------------------------+------------------------------------+
| 0000H - Reset Vector               | 7FH +----------------------------+ |
| 0003H - External Interrupt 0 (INT0)|     | General Purpose Scratchpad | |
| 000BH - Timer 0 Overflow (TF0)     |     | (80 Bytes: 30H to 7FH)     | |
| 0013H - External Interrupt 1 (INT1)| 30H +----------------------------+ |
| 001BH - Timer 1 Overflow (TF1)     | 2FH +----------------------------+ |
| 0023H - Serial Port (TI / RI)      |     | Bit-Addressable RAM        | |
| 002BH - Reserved / Timer 2 (8052)  |     | (16 Bytes / 128 Bits)      | |
| ...                                | 20H +----------------------------+ |
| 0FFFH - End of 4KB On-chip Flash   | 1FH +----------------------------+ |
|                                    |     | Bank 3 (18H - 1FH)         | |
|                                    | 17H +----------------------------+ |
|                                    |     | Bank 2 (10H - 17H)         | |
|                                    | 0FH +----------------------------+ |
|                                    |     | Bank 1 (08H - 0FH)         | |
|                                    | 07H +----------------------------+ |
|                                    |     | Bank 0 (00H - 07H) Default | |
|                                    | 00H +----------------------------+ |
+------------------------------------+------------------------------------+
```

### Key Architectural Specifications:
- **CPU:** 8-bit Arithmetic Logic Unit (ALU).
- **Internal RAM:** 128 Bytes (Addresses `00H` to `7FH`).
- **Internal ROM:** 4 Kilobytes Flash EEPROM (`0000H` to `0FFFH`).
- **External Memory Support:** Up to 64KB Program ROM and 64KB Data RAM.
- **I/O Ports:** 4 bidirectional 8-bit ports (32 pins total): Port 0, Port 1, Port 2, Port 3.
- **Timers/Counters:** Two 16-bit timers (`Timer 0` and `Timer 1`).
- **Serial Interface:** 1 Full-Duplex Universal Asynchronous Receiver/Transmitter (UART).
- **Interrupts:** 6 interrupt sources (5 vectors excluding Reset) with 2 priority levels.
- **Oscillator Frequency:** Typically 11.0592 MHz or 12.0 MHz.

---

# SECTION 3.2: Special Function Registers (SFRs) Deep-Dive

Special Function Registers occupy addresses **`80H` to `FFH`**. SFRs whose hex addresses end in **`0`** or **`8`** are **Bit-Addressable**.

| SFR Name | Hex Address | Bit-Addressable? | Description | Reset State |
|---|---|---|---|---|
| **ACC (A)** | `0E0H` | **Yes** | Accumulator register for ALU operations | `00H` |
| **B** | `0F0H` | **Yes** | Secondary arithmetic register (used in `MUL` / `DIV`) | `00H` |
| **PSW** | `0D0H` | **Yes** | Program Status Word (flags) | `00H` |
| **SP** | `081H` | No | Stack Pointer (points to top of stack) | `07H` |
| **DPTR (DPH/DPL)**| `83H / 82H` | No | 16-bit Data Pointer (`DPH` high byte, `DPL` low byte) | `0000H` |
| **P0** | `080H` | **Yes** | Port 0 Latches (Quasi-bidirectional / Open Drain) | `0FFH` |
| **P1** | `090H` | **Yes** | Port 1 Latches (Internal pull-ups) | `0FFH` |
| **P2** | `0A0H` | **Yes** | Port 2 Latches (Internal pull-ups / Address A8-A15) | `0FFH` |
| **P3** | `0B0H` | **Yes** | Port 3 Latches (Special alternate functions) | `0FFH` |
| **TMOD** | `089H` | No | Timer/Counter Mode register | `00H` |
| **TCON** | `088H` | **Yes** | Timer/Counter Control register | `00H` |
| **TL0 / TH0** | `8AH / 8CH` | No | Timer 0 Low / High counter registers | `00H` |
| **TL1 / TH1** | `8BH / 8DH` | No | Timer 1 Low / High counter registers | `00H` |
| **SCON** | `098H` | **Yes** | Serial Port Control register | `00H` |
| **SBUF** | `099H` | No | Serial Data Buffer (Separate physical TX & RX buffers) | Indeterminate |
| **PCON** | `087H` | No | Power Control register (SMOD baud doubler) | `00H` |
| **IE** | `0A8H` | **Yes** | Interrupt Enable register | `00H` |
| **IP** | `0B8H` | **Yes** | Interrupt Priority register | `00H` |

---

# SECTION 3.3: Bit-Level SFR Reference Cards

### 1. PSW (Program Status Word) — Address `0D0H` (Bit-Addressable)
| Bit 7 | Bit 6 | Bit 5 | Bit 4 | Bit 3 | Bit 2 | Bit 1 | Bit 0 |
|---|---|---|---|---|---|---|---|
| **CY** | **AC** | **F0** | **RS1** | **RS0** | **OV** | **—** | **P** |

- **CY (PSW.7):** Carry flag. Set by arithmetic overflow from MSB (bit 7).
- **AC (PSW.6):** Auxiliary Carry flag. Set when carry occurs from bit 3 to bit 4 (used in BCD addition via `DA A`).
- **F0 (PSW.5):** General purpose user flag 0.
- **RS1, RS0 (PSW.4, PSW.3):** Register Bank Select bits:
  - `00`: Bank 0 (Addresses `00H` - `07H`)
  - `01`: Bank 1 (Addresses `08H` - `0FH`)
  - `10`: Bank 2 (Addresses `10H` - `17H`)
  - `11`: Bank 3 (Addresses `18H` - `1FH`)
- **OV (PSW.2):** Overflow flag. Set when a signed arithmetic result overflows bit 6 into bit 7 incorrectly (-128 to +127).
- **P (PSW.0):** Parity flag. Set to 1 if Accumulator has an **ODD** number of 1's; cleared to 0 if EVEN parity.

---

### 2. TMOD (Timer Mode) — Address `089H` (NOT Bit-Addressable)
| Bit 7 | Bit 6 | Bit 5 | Bit 4 | Bit 3 | Bit 2 | Bit 1 | Bit 0 |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **GATE** | **C/T** | **M1** | **M0** | **GATE** | **C/T** | **M1** | **M0** |
| <-------- | **TIMER 1** | --------> | | <-------- | **TIMER 0** | --------> | |

- **GATE:** When `0`, timer starts/stops purely via software control bit `TRx`. When `1`, timer runs ONLY when external pin `INTx` is HIGH and `TRx = 1` (used for pulse width measurement).
- **C/T:** `0` = Timer mode (clocked by internal machine cycles $= f_{\text{osc}} / 12$). `1` = Counter mode (clocked by external pin `T0` / `T1` falling edges).
- **M1, M0:** Mode Selector bits:
  - `00` (**Mode 0**): 13-bit Timer/Counter (8-bit THx + 5-bit TLx; legacy 8048 mode).
  - `01` (**Mode 1**): 16-bit Timer/Counter (THx cascade with TLx; 0 to 65535).
  - `10` (**Mode 2**): 8-bit Auto-Reload Timer (TLx counts; THx holds reload value).
  - `11` (**Mode 3**): Split Timer mode (Timer 0 splits into two independent 8-bit timers).

---

### 3. TCON (Timer Control) — Address `088H` (Bit-Addressable)
| Bit 7 | Bit 6 | Bit 5 | Bit 4 | Bit 3 | Bit 2 | Bit 1 | Bit 0 |
|---|---|---|---|---|---|---|---|
| **TF1** | **TR1** | **TF0** | **TR0** | **IE1** | **IT1** | **IE0** | **IT0** |

- **TF1 / TF0:** Timer 1 / Timer 0 Overflow Flag. Hardware sets this flag upon roll-over from max value to 0. Cleared automatically upon entering ISR, or manually in software.
- **TR1 / TR0:** Timer 1 / Timer 0 Run Control bit. Software sets `1` to start timer, `0` to stop.
- **IE1 / IE0:** External Interrupt 1 / 0 Edge Flag. Set by hardware when falling edge is detected.
- **IT1 / IT0:** External Interrupt 1 / 0 Type Control bit. `0` = Low-level triggered, `1` = Falling edge triggered.

---

### 4. SCON (Serial Control) — Address `098H` (Bit-Addressable)
| Bit 7 | Bit 6 | Bit 5 | Bit 4 | Bit 3 | Bit 2 | Bit 1 | Bit 0 |
|---|---|---|---|---|---|---|---|
| **SM0** | **SM1** | **SM2** | **REN** | **TB8** | **RB8** | **TI** | **RI** |

- **SM0, SM1:** Serial Port Mode specifiers:
  - `00` (**Mode 0**): Shift Register mode (fixed baud $= f_{\text{osc}} / 12$).
  - `01` (**Mode 1**): 8-bit UART, variable baud rate set by Timer 1 (**Most widely used!**).
  - `10` (**Mode 2**): 9-bit UART, fixed baud rate ($f_{\text{osc}} / 64$ or $f_{\text{osc}} / 32$).
  - `11` (**Mode 3**): 9-bit UART, variable baud rate set by Timer 1.
- **SM2:** Multiprocessor communication enable.
- **REN:** Receiver Enable. Set to `1` to allow 8051 to receive serial data.
- **TB8 / RB8:** 9th data bit transmitted / received in Modes 2 and 3 (parity bit).
- **TI:** Transmit Interrupt Flag. Hardware sets `TI` to `1` when 8th data bit (or stop bit) has finished transmitting. **Must be cleared by software (`TI = 0;`)**.
- **RI:** Receive Interrupt Flag. Hardware sets `RI` to `1` when a complete incoming byte has arrived in SBUF. **Must be cleared by software (`RI = 0;`)**.

---

### 5. IE (Interrupt Enable) — Address `0A8H` (Bit-Addressable)
| Bit 7 | Bit 6 | Bit 5 | Bit 4 | Bit 3 | Bit 2 | Bit 1 | Bit 0 |
|---|---|---|---|---|---|---|---|
| **EA** | **—** | **ET2** | **ES** | **ET1** | **EX1** | **ET0** | **EX0** |

- **EA (IE.7):** Global Interrupt Enable bit. Must be set to `1` for any interrupt to trigger.
- **ES (IE.4):** Serial Port Interrupt Enable (`TI` or `RI`).
- **ET1 (IE.3):** Timer 1 Overflow Interrupt Enable (`TF1`).
- **EX1 (IE.2):** External Interrupt 1 Enable (`INT1` pin).
- **ET0 (IE.1):** Timer 0 Overflow Interrupt Enable (`TF0`).
- **EX0 (IE.0):** External Interrupt 0 Enable (`INT0` pin).

---

# SECTION 3.4: Vital Formulas, Mathematics & Calculations

### 1. Machine Cycle & Instruction Timing:
$$T_{\text{mc}} = \frac{12}{f_{\text{osc}}}$$
- At $f_{\text{osc}} = 12.0\text{ MHz} \implies T_{\text{mc}} = \frac{12}{12\text{ MHz}} = 1.000\ \mu\text{s}$.
- At $f_{\text{osc}} = 11.0592\text{ MHz} \implies T_{\text{mc}} = \frac{12}{11.0592\text{ MHz}} = 1.0850694\ \mu\text{s}$.

### 2. Timer Reload Calculations:
#### Mode 1 (16-Bit Timer):
$$\text{Counts} = \frac{\text{Required Delay}}{T_{\text{mc}}}$$
$$\text{Reload Value} = 65,536 - \text{Counts}$$
$$\text{THx} = \text{Reload} / 256, \quad \text{TLx} = \text{Reload} \% 256$$

#### Mode 2 (8-Bit Auto-Reload):
$$\text{Counts} = \frac{\text{Required Delay}}{T_{\text{mc}}}$$
$$\text{Reload Value} = 256 - \text{Counts}$$
$$\text{THx} = \text{Reload Value}$$

### 3. UART Baud Rate Reload (Timer 1 Mode 2):
$$\text{Baud Rate} = \frac{2^{\text{SMOD}}}{32} \times \frac{f_{\text{osc}}}{12 \times (256 - \text{TH1})}$$
When $\text{SMOD} = 0$:
$$256 - \text{TH1} = \frac{f_{\text{osc}}}{384 \times \text{Baud Rate}}$$

---

# SECTION 3.5: Top 50 Rapid-Fire Viva & Lab Quiz Questions & Answers

1. **Q: Why is an 11.0592 MHz crystal universally chosen for 8051 serial communication?**
   - **A:** Dividing 11.0592 MHz by 12 yields 921,600 Hz. Dividing by 32 yields exactly 28,800 Hz, which divides into standard UART baud rates (9600, 4800, 2400, 1200) into **exact whole integers with 0.0% timing error**. A 12 MHz crystal yields fractional divisors, causing asynchronous framing errors.

2. **Q: What is the reset value of the Stack Pointer (SP)? Where does the first `PUSH` go?**
   - **A:** On hardware reset, `SP = 07H`. Since 8051 uses pre-increment stack architecture, the first `PUSH` increments SP to `08H` and stores data in RAM location `08H` (which overlaps Register Bank 1!).

3. **Q: Why should the Stack Pointer be re-initialized in application code?**
   - **A:** To prevent stack data from overwriting Register Banks 1, 2, and 3 (`08H` to `1FH`) and bit-addressable RAM (`20H` to `2FH`). Typically, programmers initialize `MOV SP, #2FH` or `MOV SP, #3FH`.

4. **Q: Why does Port 0 require external pull-up resistors for normal I/O?**
   - **A:** Port 0 is an open-drain quasi-bidirectional port without internal pull-ups (to allow it to act as multiplexed low-order address/data bus $A_0 - A_7 / D_0 - D_7$). To output a logic HIGH when used as a standard I/O port, an external 10k resistor network (RESPACK-8) to VCC is required.

5. **Q: What is the function of the $\overline{\text{EA}}$ pin (Pin 31)? What happens if it is grounded?**
   - **A:** External Access pin. When $\overline{\text{EA}} = 1$ (+5V), the 8051 fetches instructions from on-chip internal 4KB Flash ROM (`0000H` to `0FFFH`). When $\overline{\text{EA}} = 0$ (GND), the microcontroller ignores internal Flash and fetches all instructions exclusively from external ROM.

6. **Q: What is the purpose of the $\text{ALE}$ (Address Latch Enable) pin (Pin 30)?**
   - **A:** ALE demultiplexes the shared address/data bus on Port 0. It emits a pulse twice per machine cycle to latch the lower 8-bit memory address ($A_0 - A_7$) into an external latch (such as 74HC573 or 74LS373) before Port 0 converts into a data bus ($D_0 - D_7$).

7. **Q: What is the difference between `MOV`, `MOVX`, and `MOVC`?**
   - **A:**
     - `MOV`: Moves data within fast internal RAM and SFRs.
     - `MOVX`: Moves data to/from external Data RAM (`@DPTR` or `@Ri`) using `\RD` / `\WR` strobes.
     - `MOVC`: Moves constant bytes from Program ROM (`@A+DPTR` or `@A+PC`) using the `\PSEN` strobe.

8. **Q: What happens to flags during `INC` and `DEC` instructions?**
   - **A:** In 8051, `INC` and `DEC` do **NOT** affect any flags in PSW (including Carry `CY` and Zero).

9. **Q: What is the difference between Timer and Counter modes?**
   - **A:** In Timer mode (`C/T = 0`), the register increments every machine cycle ($f_{\text{osc}} / 12$). In Counter mode (`C/T = 1`), the register increments upon receiving a 1-to-0 (falling) transition on external pin `T0` (P3.4) or `T1` (P3.5).

10. **Q: What is the maximum delay achievable by a 16-bit timer in a single run at 12 MHz?**
    - **A:** Maximum counts = 65,536. At 12 MHz ($1\ \mu\text{s}$ per count), $\text{Max Delay} = 65,536\ \mu\text{s} \approx 65.536\text{ milliseconds}$.

11. **Q: How does Mode 2 Auto-Reload work?**
    - **A:** The 8-bit register `TLx` does the counting. `THx` holds the constant reload value. When `TLx` overflows from `FFH` to `00H`, hardware automatically reloads `TLx` with the value in `THx` and sets `TFx` without CPU intervention.

12. **Q: What are the interrupt vector addresses for all 5 interrupts?**
    - **A:**
      - External Interrupt 0 (`INT0`): `0003H`
      - Timer 0 Overflow (`TF0`): `000BH`
      - External Interrupt 1 (`INT1`): `0013H`
      - Timer 1 Overflow (`TF1`): `001BH`
      - Serial Port (`TI`/`RI`): `0023H`

13. **Q: Why is there an 8-byte space between interrupt vector addresses?**
    - **A:** Vector addresses are spaced 8 bytes apart (`0003H`, `000BH`, `0013H`...). Since typical ISR routines are longer than 8 bytes, programmers place an unconditional long jump instruction (`LJMP ISR_NAME`) at the vector location.

14. **Q: Why must `TI` and `RI` be cleared by software in serial communication?**
    - **A:** Unlike timer overflow flags (`TF0`, `TF1`) which are cleared automatically when entering their ISR, UART flags `TI` and `RI` share a single interrupt vector (`0023H`). The CPU cannot know which event caused the interrupt, so the software ISR must inspect them and clear them manually (`TI = 0; RI = 0;`).

15. **Q: What is the role of the `SMOD` bit in `PCON`?**
    - **A:** `SMOD` is bit 7 of `PCON`. When `SMOD = 1`, the internal UART baud rate generator frequency is doubled.

16. **Q: Explain the difference between `LJMP`, `AJMP`, and `SJMP`.**
    - **A:**
      - `SJMP` (Short Jump): 2 bytes, relative offset range of -128 to +127 bytes.
      - `AJMP` (Absolute Jump): 2 bytes, jumps anywhere within the current 2KB memory page.
      - `LJMP` (Long Jump): 3 bytes, jumps anywhere within the full 64KB address space (`0000H` - `FFFFH`).

17. **Q: What is the purpose of the `DA A` (Decimal Adjust Accumulator) instruction?**
    - **A:** Converts the binary sum in Accumulator into packed BCD format after an `ADD` or `ADDC`. If the lower nibble exceeds 9 or `AC = 1`, it adds 6 to the lower nibble. If the upper nibble exceeds 9 or `CY = 1`, it adds 6 to the upper nibble. **Works only after `ADD`/`ADDC`, never after `SUBB` or `MUL`!**

18. **Q: What is the result of `DIV AB` when B = 0?**
    - **A:** Division by zero is undefined. The 8051 sets the Overflow flag `OV = 1`, while the quotient and remainder are unpredictable.

19. **Q: What register holds the upper byte of a product after `MUL AB`?**
    - **A:** Register `B`. Accumulator `A` holds the lower 8 bits, and Register `B` holds the upper 8 bits. If product > 255, `OV` flag is set to 1; otherwise `OV = 0`.

20. **Q: What is the bit address of Port 1 Pin 5 (P1.5)?**
    - **A:** Port 1 base SFR address is `90H`. Bit 5 address is `95H`.

21. **Q: What is the bit address of `CY` (Carry flag)?**
    - **A:** PSW base SFR address is `0D0H`. Bit 7 (`CY`) address is `0D7H`.

22. **Q: How many register banks are present in 8051? How do you switch to Bank 2?**
    - **A:** 4 register banks (Bank 0, 1, 2, 3). To switch to Bank 2, set `RS1 = 1` and `RS0 = 0` in PSW (`SETB PSW.4; CLR PSW.3;`).

23. **Q: What is quasi-bidirectional I/O?**
    - **A:** An I/O port architecture that can act as both input and output without an explicit Data Direction Register (DDR). To read from an external pin, software must first write `1` to the pin latch to turn off the low-impedance pulldown transistor.

24. **Q: What is the function of the `SWAP A` instruction?**
    - **A:** Swaps the upper 4 bits (nibble) with the lower 4 bits (nibble) of the Accumulator (`bits 7-4` swap with `bits 3-0`).

25. **Q: How many machine cycles does `DJNZ` take?**
    - **A:** 2 machine cycles ($2.17\ \mu\text{s}$ at 11.0592 MHz, or $2.00\ \mu\text{s}$ at 12 MHz).

26. **Q: What instruction clears the accumulator without using immediate data?**
    - **A:** `CLR A` (1 byte, 1 machine cycle).

27. **Q: In Keil C51, what is the size of `char`, `int`, and `long`?**
    - **A:**
      - `char`: 1 byte (8 bits, 0 to 255)
      - `int`: 2 bytes (16 bits, 0 to 65535)
      - `long`: 4 bytes (32 bits, 0 to 4,294,967,295)

28. **Q: In Keil C51, what is the keyword `sbit`?**
    - **A:** Defines a single-bit variable mapped to an addressable bit in an SFR (e.g. `sbit LED = P2^0;`).

29. **Q: What is the keyword `interrupt` in Keil C51?**
    - **A:** Specifies that a C function is an Interrupt Service Routine and links it to an interrupt vector (e.g. `void timer0_isr() interrupt 1`).

30. **Q: What is the difference between Common Anode and Common Cathode 7-segment displays?**
    - **A:**
      - Common Anode: All LED anodes are tied to VCC (+5V). Applying logic `0` (GND) turns a segment ON.
      - Common Cathode: All LED cathodes are tied to GND (0V). Applying logic `1` (+5V) turns a segment ON.

31. **Q: Why is display multiplexing used in multi-digit 7-segment displays?**
    - **A:** Multiplexing reduces the required I/O pins. Instead of $4 \times 8 = 32$ pins for 4 digits, multiplexing uses 8 segment data pins plus 4 digit-enable pins (only 12 pins total), flashing digits faster than 50 Hz to exploit the human eye's **persistence of vision**.

32. **Q: How do you configure 8x8 LED matrix pins?**
    - **A:** Rows are connected to one 8-bit port and Columns to another. To light up an LED at Row $R$ and Column $C$ in active-low logic, drive Row $R = 0$ and Column $C = 0$.

33. **Q: In LCD 16x2 interfacing, what are the functions of RS, RW, and E?**
    - **A:**
      - `RS` (Register Select): `0` = Instruction Command register, `1` = Data display register.
      - `RW` (Read/Write): `0` = Write to LCD, `1` = Read status/data from LCD.
      - `E` (Enable): A High-to-Low transition pulse latches data from the bus into the LCD controller.

34. **Q: What command initializes LCD 16x2 in 8-bit, 2-line, 5x7 mode?**
    - **A:** `0x38`.

35. **Q: What command clears the LCD screen and returns the cursor to home?**
    - **A:** `0x01`.

36. **Q: What command moves the LCD cursor to the beginning of Line 2?**
    - **A:** `0xC0` (Line 1 begins at `0x80`).

37. **Q: What is the difference between edge-triggered and level-triggered interrupts on INT0/INT1?**
    - **A:** When `IT0 = 0`, the interrupt is triggered as long as the pin is held at a LOW level (can cause repeated triggers if not released). When `IT0 = 1`, it triggers only on the HIGH-to-LOW falling transition edge.

38. **Q: What is the function of the `RETI` instruction?**
    - **A:** Return from Interrupt. It pops the return address off the stack AND restores the internal interrupt priority logic, enabling lower-priority interrupts to trigger again. Using plain `RET` instead of `RETI` blocks future interrupts!

39. **Q: Can two interrupts of the same priority interrupt each other?**
    - **A:** No. An interrupt can only be pre-empted by an interrupt of higher priority.

40. **Q: How do you assign high priority to Timer 0?**
    - **A:** Set bit `PT0` in the Interrupt Priority register (`IP = 0x02` or `SETB IP.1`).

41. **Q: What is the baud rate if TH1 = -3 (0xFD) at 11.0592 MHz with SMOD = 0?**
    - **A:** 9600 Baud.

42. **Q: What is the baud rate if TH1 = -6 (0xFA) at 11.0592 MHz with SMOD = 0?**
    - **A:** 4800 Baud.

43. **Q: What is the baud rate if TH1 = -12 (0xF4) at 11.0592 MHz with SMOD = 0?**
    - **A:** 2400 Baud.

44. **Q: What is the baud rate if TH1 = -3 (0xFD) at 11.0592 MHz with SMOD = 1?**
    - **A:** 19,200 Baud (doubled!).

45. **Q: What is the memory location of internal RAM bit address `00H`?**
    - **A:** Bit 0 of RAM byte address `20H`.

46. **Q: What is the memory location of internal RAM bit address `7FH`?**
    - **A:** Bit 7 of RAM byte address `2FH`.

47. **Q: What does `JB ACC.7, label` do?**
    - **A:** Jump to `label` if bit 7 of the Accumulator is set (1). Used for negative number detection in 2's complement.

48. **Q: What does `CJNE A, #data, label` do?**
    - **A:** Compare Accumulator with immediate data; if not equal, jump to `label`. If $A < \text{data}$, it sets Carry flag `CY = 1`; if $A > \text{data}$, `CY = 0`.

49. **Q: How does `XCH A, @R0` differ from `XCHD A, @R0`?**
    - **A:**
      - `XCH A, @R0`: Exchanges the entire 8-bit byte between Accumulator and RAM.
      - `XCHD A, @R0`: Exchanges ONLY the lower 4 bits (nibble) between Accumulator and RAM, leaving upper nibbles untouched.

50. **Q: What is the state of all 8051 port pins immediately after hardware reset?**
    - **A:** All port latch registers (`P0`, `P1`, `P2`, `P3`) are loaded with `0xFF` (logic HIGH), configuring all pins as inputs.

