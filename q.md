# Embedded Systems (CSN15402) — Master University Examination Guide & Fully Solved Papers
**Target Institution:** IITs / NITs (Department of Computer Science & Engineering)  
**Course Code:** CSN15402 — Embedded Systems (Semester V)  
**Coverage:** Dual Examiner Strategy (Type A & Type B) + Complete Mid-Sem 2024-25, Mid-Sem 2025-26 & End-Sem 2025-26 Solutions

---

## PART 1: The Dual-Examiner Strategy Guide
At premier engineering institutes, examinations alternate between two distinct professor profiles. To secure full marks, you must understand both styles:

```
+-----------------------------------------------------------------------------------+
|                           THE DUAL-EXAMINER PROFILES                              |
+-----------------------------------------------------------------------------------+
| TYPE A EXAMINER (Theoretical & Architectural) | TYPE B EXAMINER (Systems & Low-Level Code) |
| - Focuses on core definitions, metrics, trade- | - Focuses on real-world firmware routines, |
|   offs, and architectural comparisons.        |   buffer manipulations, and pointers.      |
| - Loves hardware pinouts (PSEN, EA, ALE).     | - Tests list file completion, PC tracing.  |
| - Tests PSW flag conditions (OV mechanics).    | - Challenges timer limits (Mode 2 factors).|
| - Tests clean, modular hardware timer math.   | - Tests overlapping memory block copies.   |
+-----------------------------------------------------------------------------------+
```

---

## PART 2: Mid-Semester Examination 2024–25 (Type A Style) — Fully Solved

### Question 1(a) [Marks: 3 + 1 + 1]
**Question:**
> *Illustrate the architectural differences among general-purpose, single-purpose and application-specific processors. Evaluate these processor architectures individually through the metrics defined for various embedded system design metrics.*

#### Architectural Differences:
1. **General-Purpose Processor (GPP — Software):**
   - Built with a generic arithmetic logic unit (ALU), standard general-purpose registers, and a flexible control unit fetching instructions via a Program Counter (PC).
   - Designed to run arbitrary software applications to maximize volume sales across different industries.
2. **Single-Purpose Processor (SPP — Hardware):**
   - A dedicated digital circuit custom-wired (datapath + FSM controller) to execute **exactly one algorithm**.
   - Contains zero instruction fetch engines or general-purpose instruction decoders.
3. **Application-Specific Instruction-Set Processor (ASIP — Hybrid):**
   - A programmable microprocessor whose instruction set and internal datapath are customized for a specific application domain (e.g., DSPs with single-cycle MAC units, network processors).

#### Evaluation Against Design Metrics (Model Answer Table):
| Design Metric | General-Purpose Processor (GPP) | Application-Specific Processor (ASIP) | Single-Purpose Processor (SPP) |
| :--- | :--- | :--- | :--- |
| **NRE Cost** | **Very Low** (Standard off-the-shelf IC) | **Medium–High** (Custom instruction design) | **Extremely High** (Full ASIC tape-out) |
| **Unit Cost** | **Moderate** (Redundant silicon area) | **Low–Medium** (Optimized silicon area) | **Lowest** at ultra-high volume |
| **Performance** | **Lowest** (Instruction fetch/decode cycles) | **High** (Domain-specific instructions) | **Maximum** (Hardwired concurrent logic) |
| **Power Efficiency** | **Poor** (Generic pipeline overhead) | **Good** (Fewer instructions needed) | **Maximum** (Minimal switching gates) |
| **Size / Die Area** | **Large** (Contains unused features) | **Medium** (Tailored functional units) | **Minimal** (Zero redundant gates) |
| **Time-to-Market** | **Fastest** (Immediate C/C++ firmware) | **Moderate** (Toolchain setup takes months)| **Slowest** (6–18 month fabrication) |
| **Flexibility** | **Complete** (Code reprogrammed anytime) | **Moderate** (Software updates permitted) | **Zero** (Cannot modify fabricated IC) |

---

### Question 1(b) [Marks: 2]
**Question:**
> *When does the OV (overflow) flag of the PSW register set in 8-bit arithmetic operations?*

#### Solution:
In 8-bit signed number arithmetic (range: $-128 \text{ to } +127$), the **OV flag is set to 1** if the result of an arithmetic operation exceeds this signed range.

In hardware, this is detected by checking carries at the sign bit ($D_7$):
$$\mathbf{OV = C_6 \oplus C_7}$$
where $C_6$ is the carry from bit $D_6$ to bit $D_7$, and $C_7$ is the carry out of bit $D_7$ (the standard Carry flag `CY`).

The OV flag is set to 1 if either of the following two mutually exclusive conditions occurs:
1. **Condition I:** There is a carry from bit $D_6$ into bit $D_7$ ($C_6 = 1$), but **no carry out of bit $D_7$** ($C_7 = 0$, so $\text{CY} = 0$).
2. **Condition II:** There is a carry out of bit $D_7$ ($C_7 = 1$, so $\text{CY} = 1$), but **no carry from bit $D_6$ into bit $D_7$** ($C_6 = 0$).

---

### Question 1(c) [Marks: 2]
**Question:**
> *Explain the function of the $\overline{\text{PSEN}}$ and $\overline{\text{EA}}$ pins of the 8051 chip.*

#### Solution:
1. **$\overline{\text{PSEN}}$ (Program Store Enable — Pin 29):**
   - Active-low output control signal.
   - Serves as the **read strobe for External Program Memory (External ROM)**.
   - When the 8051 fetches instructions from external ROM, it pulses $\overline{\text{PSEN}}$ LOW twice per machine cycle. Connected to the Output Enable ($\overline{\text{OE}}$) pin of the external ROM chip. It is never asserted during internal ROM execution or external RAM data accesses (`MOVX`).
2. **$\overline{\text{EA}}$ (External Access Enable — Pin 31):**
   - Active-low input pin that selects whether program execution begins from internal or external ROM:
     - **$\overline{\text{EA}} = 1$ (Tied to $V_{CC}$):** The 8051 executes programs stored in its internal 4 KB ROM (`0000H`–`0FFFH`). If PC exceeds `0FFFH`, execution automatically rolls over to external ROM (`1000H`–`FFFFH`).
     - **$\overline{\text{EA}} = 0$ (Tied to GND):** The 8051 completely ignores internal ROM and fetches **all instructions** (from address `0000H` through `FFFFH`) strictly from external ROM.

---

### Question 2(a) [Marks: 2.5]
**Question:**
> *Write an 8051 assembly level program to add two nibbles of an 8-bit number.*

#### Solution:
```assembly
        ORG 0000H
        MOV A, #43H       ; Example 8-bit number (Upper nibble = 4, Lower nibble = 3)
        MOV 30H, A        ; Save original number in RAM 30H
        PUSH 30H          ; Push onto stack for temporary preservation
        ANL A, #0FH       ; Mask upper nibble: A = 03H (Lower nibble)
        MOV R0, A         ; Save lower nibble in R0
        POP 40H           ; Restore original number into RAM 40H
        MOV R1, #40H      ; R1 points to RAM 40H
        MOV A, @R1        ; A = 43H
        ANL A, #0F0H      ; Mask lower nibble: A = 40H
        SWAP A            ; Swap nibbles: A = 04H (Upper nibble)
        ADD A, R0         ; Add lower nibble: A = 04H + 03H = 07H
        END
```

---

### Question 2(b) [Marks: 3.5]
**Question:**
> *Using the following list file, determine the contents of the Program Counter (PC) for each instruction. Also calculate the jump forward and backward addresses at the blank spaces.*

```assembly
Line  PC    Opcode   Mnemonic  Operand
01    0000           ORG 0000H
02    0000  74 55    BACK:     MOV A, #55H
03    0002  F5 90              MOV P1, A
04    0004  7C 99              MOV R4, #99H
05    0006  7D 67              MOV R5, #67H
06    0008  12 03 00           LCALL DELAY
07    000B  74 AA              MOV A, #0AAH
08    000D  F5 90              MOV P1, A
09    000F  12 03 00           LCALL DELAY
10    0012  80 EC              SJMP BACK
11    0014                     ; Delay subroutine
12    0300           ORG 300H
13    0300  C0 04    DELAY:    PUSH 4
14    0302  C0 05              PUSH 5
15    0304  7C FF              MOV R4, #0FFH
16    0306  7D FF    NEXT:     MOV R5, #0FFH
17    0308  DD FE    AGAIN:    DJNZ R5, AGAIN
18    030A  DC FA              DJNZ R4, NEXT
19    030C  D0 05              POP 5
20    030E  D0 04              POP 4
21    0310  22                 RET
22    0311           END
```

#### Detailed Jump Offset Derivations:
1. **Line 10 (`SJMP BACK` at `0012H`):**
   - Next PC = $0012\text{H} + 2 = 0014\text{H}$. Target `BACK` = $0000\text{H}$.
   - $\text{Offset} = 0000\text{H} - 0014\text{H} = -20_{10} = \mathbf{ECH}$. Emits: `80 EC`.
2. **Line 17 (`DJNZ R5, AGAIN` at `0308H`):**
   - Next PC = $0308\text{H} + 2 = 030A\text{H}$. Target `AGAIN` = $0308\text{H}$.
   - $\text{Offset} = 0308\text{H} - 030A\text{H} = -2_{10} = \mathbf{FEH}$. Emits: `DD FE`.
3. **Line 18 (`DJNZ R4, NEXT` at `030AH`):**
   - Next PC = $030A\text{H} + 2 = 030C\text{H}$. Target `NEXT` = $0306\text{H}$.
   - $\text{Offset} = 0306\text{H} - 030C\text{H} = -6_{10} = \mathbf{FAH}$. Emits: `DC FA`.

---

### Question 3(a) [Marks: 2]
**Question:**
> *Explain each bit of TCON and TMOD registers in brief.*

*(Refer to Unit 4 Notes, Section 5 for TMOD and Section 6 for TCON for full bit tables).*

---

### Question 3(b) [Marks: 3]
**Question:**
> *Write an assembly level program for 8051 microcontroller to generate a square wave with an ON time of 10 ms and an OFF time of 5 ms on all pins of Port P1. Assume XTAL = 11.0592 MHz. [Note: Make use of only one delay subroutine]*

#### Mathematical Steps:
- $T_{\text{MC}} = \frac{12}{11.0592\text{ MHz}} = 1.08507 \ \mu\text{s}$.
- $\text{Counts for } 5\text{ ms} = \frac{5\text{ ms}}{1.08507\ \mu\text{s}} \approx 4608 \implies \text{Value} = 65536 - 4608 = 60928 = \mathbf{EE00H}$.
- $\text{Counts for } 10\text{ ms} = \frac{10\text{ ms}}{1.08507\ \mu\text{s}} \approx 9217 \implies \text{Value} = 65536 - 9217 = 56319 = \mathbf{DBFFH}$.

#### Assembly Code:
```assembly
        ORG 0000H
BACK:   MOV TMOD, #01H        ; Timer 0 in Mode 1 (16-bit)
        MOV TL0, #00H
        MOV TH0, #0EEH        ; Load 5 ms count
        MOV P1, #00H          ; Set Port 1 pins LOW
        ACALL DELAY
        MOV TL0, #0FFH
        MOV TH0, #0DBH        ; Load 10 ms count
        MOV P1, #0FFH         ; Set Port 1 pins HIGH
        ACALL DELAY
        SJMP BACK

        ORG 0300H
DELAY:  SETB TR0              ; Start Timer 0
AGAIN:  JNB  TF0, AGAIN       ; Wait for overflow
        CLR  TR0              ; Stop Timer 0
        CLR  TF0              ; Clear TF0 flag
        RET
        END
```

---

### Question 3(c) [Marks: 4]
**Question:**
> *Consider an auditorium with a capacity of 500 guests. Provide an 8051-based design (along with assembly code) that counts the entry of persons into the auditorium and displays the count.*

#### Assembly Code:
```assembly
        ORG 0000H
        MOV TMOD, #05H        ; Timer 0 in Counter Mode 1 (16-bit counter on P3.4)
        MOV TL0, #00H
        MOV TH0, #00H
        SETB P3.4             ; Configure P3.4 as input
        SETB TR0              ; Start Counter 0
BACK:   MOV A, TL0            ; Read lower count byte
        MOV P2, A             ; Output to Port 2
        MOV A, TH0            ; Read upper count byte
        MOV P1, A             ; Output to Port 1
        
        ; Check if 500 guests reached (500 = 01F4H)
        CJNE A, #01H, BACK
        MOV A, TL0
        CJNE A, #0F4H, BACK
        CLR TR0               ; Auditorium full: stop counting
HALT:   SJMP HALT
        END
```

---

## PART 3: Mid-Semester Examination 2025–26 (Type B Style) — Fully Solved

### Question 1 (Sections A & B) [Marks: 10 + 4]
**Question:**
> *Ten packed BCD numbers are stored in a ROM lookup table starting at 400H. Retrieve each BCD number, convert it to two ASCII characters, and continuously display them on Port 2 (higher digit) and Port 3 (lower digit) with a 0.5-second delay (XTAL = 22 MHz, software delay without timers).*

#### List File Completion:
```assembly
PC    Opcode    Line  Mnemonic  Operand
                01              ORG 0000H
0000  7B 0A     02    START:    MOV R3, #10
0002  90 04 00  03              MOV DPTR, #0400H
0005  E4        04    BACK:     CLR A
0006  93        05              MOVC A, @A+DPTR
0007  FA        06              MOV R2, A
0008  54 0F     07              ANL A, #0FH       ; Mask upper nibble
000A  44 30     08              ORL A, #30H       ; Convert to ASCII
000C  F5 B0     09              MOV P3, A         ; Output lower digit to P3
000E  EA        10              MOV A, R2
000F  54 F0     11              ANL A, #0F0H      ; Mask lower nibble
0011  C4        12              SWAP A            ; Position upper nibble in lower
0012  44 30     13              ORL A, #30H       ; Convert to ASCII
0014  F5 A0     14              MOV P2, A         ; Output higher digit to P2
0016  71 00     15              ACALL DELAY
0018  A3        16              INC DPTR
0019  DB EA     17              DJNZ R3, BACK
001B  80 E3     18              SJMP START

                19              ORG 0300H
0300  7E 07     20    DELAY:    MOV R6, #7
0302  7D FF     21    HERE1:    MOV R5, #255
0304  7C FF     22    HERE2:    MOV R4, #255
0306  DC FE     23    HERE3:    DJNZ R4, HERE3
0308  DD FA     24              DJNZ R5, HERE2
030A  DE F6     25              DJNZ R6, HERE1
030C  22        26              RET

                27              ORG 0400H
0400  29 32 ... 28              DB 29H, 32H, 33H, 12H, 92H, 34H, 23H, 47H, 42H, 61H
                29              END
```

---

### Question 3 (Sections C & D) [Marks: 10 + 4]
**Question:**
> *Write an assembly routine `COPY_MSG` to copy a string from Code ROM to Upper Internal RAM (`80H`–`FFH`) via indirect addressing, mirror each byte to Port 0, compute LEN (count) at `7EH` and CHK (sum mod 256) at `7FH`, stop on terminator `00H` or if RAM overflows, and preserve all touched registers.*

#### Verified Assembly Code:
```assembly
        ORG 0000H
        SJMP MAIN

        ORG 0030H
MSG:    DB 'HELLO, 8051!', 0

MAIN:   ACALL COPY_MSG
HERE:   SJMP HERE

COPY_MSG:
        ; Save context onto stack
        PUSH ACC
        PUSH B
        PUSH DPL
        PUSH DPH
        PUSH PSW
        PUSH 0        ; R0
        PUSH 1        ; R1
        PUSH 2        ; R2
        PUSH 3        ; R3

        ; Initialize pointers and counters
        MOV DPTR, #MSG    ; DPTR points to ROM string
        MOV R0, #080H     ; R0 points to Upper RAM (80H-FFH)
        MOV R2, #00H      ; R2 = LEN counter
        MOV R3, #00H      ; R3 = Checksum accumulator

CPY_LOOP:
        CLR A
        MOVC A, @A+DPTR   ; Fetch byte from Code ROM
        JZ CPY_DONE       ; If null terminator (00H), finished!

        ; Indirect write to Upper RAM (80H-FFH)
        MOV @R0, A        ; Indirect addressing targets upper RAM!
        MOV P0, A         ; Mirror byte to Port 0

        ; Update checksum: R3 = R3 + A
        ADD A, R3
        MOV R3, A

        ; Increment pointers
        INC DPTR
        INC R0
        INC R2            ; LEN++

        ; Detect overflow past FFH (R0 wraps to 00H)
        CJNE R0, #00H, CPY_LOOP

CPY_DONE:
        MOV 07EH, R2      ; Store LEN at RAM 7EH
        MOV 07FH, R3      ; Store CHK at RAM 7FH

        ; Restore context in reverse order
        POP 3
        POP 2
        POP 1
        POP 0
        POP PSW
        POP DPH
        POP DPL
        POP B
        POP ACC
        RET
```

---

### Question 4 (Sections C & D) [Marks: 10]
**Question:**
> *Assume XTAL = 22 MHz. Write an 8051 assembly code to generate a square wave of frequency 1 kHz on pin P1.2 using timer's Mode 2.*

#### Mathematical Factor Decomposition:
- $T_{\text{MC}} = \frac{12}{22\text{ MHz}} = 0.5455 \ \mu\text{s}$.
- Half-pulse period for 1 kHz $= 0.5\text{ ms} = 500\ \mu\text{s}$.
- Cycles needed $= \frac{500\ \mu\text{s}}{0.5455\ \mu\text{s}} \approx 915 \text{ cycles}$.
- Since max Mode 2 capacity is $256$ cycles, we decompose:
  $$915 = 183 \times 5$$
- Value loaded in `TH0` $= 256 - 183 = 73_{10} = \mathbf{49H}$.

#### Assembly Code:
```assembly
        ORG 0000H
        MOV TMOD, #02H        ; Timer 0 in Mode 2 (8-bit auto-reload)
RPT:    CPL P1.2              ; Toggle P1.2
        MOV R0, #05           ; Multiplier = 5
AGAIN:  MOV TH0, #49H         ; Auto-reload count
        SETB TR0              ; Start Timer 0
BACK:   JNB TF0, BACK         ; Wait for 183-count overflow
        CLR TR0
        CLR TF0
        DJNZ R0, AGAIN        ; Repeat 5 times (Total = 183 * 5 = 915 cycles = 0.5 ms)
        SJMP RPT
        END
```

---

### Question 5(a) [Common for All Sections]
**Question:**
> *Write an 8051 assembly code to copy data from a source block to a destination block given that the destination block overlaps with the source block within internal RAM.*

#### Engineering Strategy:
- If $\text{Destination} > \text{Source}$: Copying from start to end would overwrite source bytes before they are read. **Must copy backwards (from end to start)!**
- If $\text{Destination} < \text{Source}$: **Must copy forwards (from start to end)!**

```assembly
; Overlapping Block Transfer Routine in Internal RAM
; Assumes Source = 30H, Destination = 35H, Length = 10 bytes (Dest > Source -> Copy Backward)
        ORG 0000H
        MOV R0, #(30H + 10 - 1) ; R0 points to end of source (39H)
        MOV R1, #(35H + 10 - 1) ; R1 points to end of destination (3EH)
        MOV R2, #10             ; Block length

BACK:   MOV A, @R0              ; Read source from tail
        MOV @R1, A              ; Write to destination tail
        DEC R0
        DEC R1
        DJNZ R2, BACK           ; Loop 10 times safely without clobbering!
        SJMP $
        END
```

---

### Question 5(b) [Common for All Sections]
**Question:**
> *Given the mnemonics LJMP and CJNE, tabulate details for each instruction row-wise.*

| Complete Syntax | Instruction Size | Machine Cycles | Brief Description |
| :--- | :---: | :---: | :--- |
| `LJMP 16-bit_address` | **3 Bytes** | **2 Cycles** | Transfers control unconditionally to any address location within the 64 KB code space. |
| `CJNE A, #data, target`<br>`CJNE A, direct, target`<br>`CJNE Rn, #data, target`<br>`CJNE @Ri, #data, target` | **3 Bytes** | **2 Cycles** | Compares destination and source operands. If not equal, branches to target. Updates Carry flag: **$CY = 1$ if Destination $<$ Source**, else **$CY = 0$**. |

---

### Question 5(c) [Common for All Sections]
**Question:**
> *Upon reset, internal RAM locations are cleared. State what changes are made in the general-purpose registers (bank registers) after executing the following code:*

```assembly
        CLR   C
        MOV   R2, #04H
        MOV   R0, #40H
        MOV   R1, #50H
        SETB  PSW.3          ; Switches to Register Bank 1
        MOV   R0, #60H       ; R0 of Bank 1 (RAM 08H) = 60H
        CLR   PSW.3          ; Switches back to Register Bank 0
BACK:   MOV   A, @R0
        ADDC  A, @R1
        INC   R0             ; Bank 0 R0 increments
        INC   R1             ; Bank 0 R1 increments
        SETB  PSW.3          ; Switches to Register Bank 1
        MOV   @R0, A
        INC   R0             ; Bank 1 R0 increments
        CLR   PSW.3          ; Switches to Register Bank 0
        DJNZ  R2, BACK       ; Loops 4 times
```

#### Final Answer:
- **`R0` of Bank 0** $= 40\text{H} + 4 = \mathbf{44\text{H}}$ (RAM location `00H`).
- **`R1` of Bank 0** $= 50\text{H} + 4 = \mathbf{54\text{H}}$ (RAM location `01H`).
- **`R0` of Bank 1** $= 60\text{H} + 4 = \mathbf{64\text{H}}$ (RAM location `08H`).

---

## PART 4: End-Semester Examination 2025–26 — Fully Solved

### Question 1(a) [Marks: 4]
**Question:**
> *Show the stack and stack pointer for each line of the following program. Does the POP sequence restore the original values of R0, R3, and R7? If not, show the correct sequence.*

```assembly
Line  Instruction      SP Value    RAM Address & Content
01    ORG 0            SP = 07H    (Reset default)
02    MOV R0, #66H     SP = 07H    R0 (00H) = 66H
03    MOV R3, #7FH     SP = 07H    R3 (03H) = 7FH
04    MOV R7, #5DH     SP = 07H    R7 (07H) = 5DH
05    PUSH 0           SP = 08H    RAM[08H] = 66H
06    PUSH 3           SP = 09H    RAM[09H] = 7FH
07    PUSH 7           SP = 0AH    RAM[0AH] = 5DH
08    CLR A            SP = 0AH    A = 00H
09    MOV R3, A        SP = 0AH    R3 = 00H
10    MOV R7, A        SP = 0AH    R7 = 00H
11    POP 3            SP = 09H    R3 = 5DH  (WRONG! Was R7!)
12    POP 7            SP = 08H    R7 = 7FH  (WRONG! Was R3!)
13    POP 0            SP = 07H    R0 = 66H  (Restored)
```

#### Analysis:
- **NO, the POP sequence does NOT restore original values!** Values of `R3` and `R7` are swapped because the stack operates in **LIFO (Last-In, First-Out)** order.
- **Correct Restoration Sequence:**
  ```assembly
  POP 7    ; Pops top of stack (5DH) back into R7!
  POP 3    ; Pops next byte (7FH) back into R3!
  POP 0    ; Pops base byte (66H) back into R0!
  ```

---

### Question 1(c) [Marks: 3]
**Question:**
> *Indicate the status of the CY flag after CJNE is executed in each case:*

1. `MOV A, #25H; CJNE A, #44H, OVER` $\implies 25\text{H} < 44\text{H} \implies \mathbf{CY = 1}$.
2. `MOV A, #0FFH; CJNE A, #6FH, NEXT` $\implies \text{FFH} \ge 6\text{FH} \implies \mathbf{CY = 0}$.
3. `MOV A, #34; CJNE A, #34, NEXT` $\implies 34 = 34 \implies \mathbf{CY = 0}$.
4. `MOV R1, #0; CJNE R1, #0, NEXT` $\implies 0 = 0 \implies \mathbf{CY = 0}$.
5. `MOV R5, #54H; CJNE R5, #0FFH, NEXT` $\implies 54\text{H} < \text{FFH} \implies \mathbf{CY = 1}$.
6. `MOV A, #0AAH; ANL A, #55H; CJNE A, #00, NEXT` $\implies A = 00\text{H} \implies \mathbf{CY = 0}$.

---

### Question 2(a) [Marks: 4]
**Question:**
> *Find the contents of register A after executing each sequence:*

#### (a)
```assembly
MOV A, #56H    ; A = 0101 0110
SWAP A         ; A = 0110 0101 (65H)
RR A           ; Rotate Right: 1011 0010 (B2H)
RR A           ; Rotate Right: 0101 1001 (59H)
```
**Final A = 59H**

#### (b)
```assembly
MOV A, #39H    ; A = 0011 1001
CLR C          ; CY = 0
RL A           ; Rotate Left: 0111 0010 (72H)
RL A           ; Rotate Left: 1110 0100 (E4H)
```
**Final A = E4H**

#### (c)
```assembly
CLR C          ; CY = 0
MOV A, #4DH    ; A = 0100 1101
SWAP A         ; A = 1101 0100 (D4H)
RRC A          ; Rotate Right thru CY: Bit 0 (0) goes to CY, CY (0) goes to D7 -> 0110 1010 (6AH), CY=0
RRC A          ; Rotate Right thru CY: Bit 0 (0) goes to CY, CY (0) goes to D7 -> 0011 0101 (35H), CY=0
```
**Final A = 35H, CY = 0**

#### (d)
```assembly
SETB C         ; CY = 1
MOV A, #7AH    ; A = 0111 1010
SWAP A         ; A = 1010 0111 (A7H)
RLC A          ; Rotate Left thru CY: D7 (1) goes to CY, old CY (1) goes to D0 -> 0100 1111 (4FH), CY=1
RLC A          ; Rotate Left thru CY: D7 (0) goes to CY, old CY (1) goes to D0 -> 1001 1111 (9FH), CY=0
```
**Final A = 9FH, CY = 0**

---

### Question 2(b) [Marks: 6]
**Question:**
> *ROM has bytes 25H, 62H, 3FH, 52H at 0250H. Write an assembly program to: (I) Copy to RAM starting at 30H, (II) Compute checksum byte (2's complement of sum mod 256), (III) Test checksum: if sum + checksum == 0, send 'G' to Port 1, else send 'B'.*

#### Assembly Code:
```assembly
        ORG 0000H
        ; Step I: Copy 4 bytes from ROM to RAM
        MOV DPTR, #0250H
        MOV R0, #30H
        MOV R2, #04
COPY_LP:CLR A
        MOVC A, @A+DPTR
        MOV @R0, A
        INC DPTR
        INC R0
        DJNZ R2, COPY_LP

        ; Step II: Calculate Checksum Byte
        MOV R0, #30H
        MOV R2, #04
        CLR A
SUM_LP: ADD A, @R0
        INC R0
        DJNZ R2, SUM_LP
        CPL A
        ADD A, #01H       ; A now holds Checksum Byte!
        MOV @R0, A        ; Store Checksum at 34H

        ; Step III: Test Checksum
        MOV R0, #30H
        MOV R2, #05       ; Sum all 5 bytes (4 data + 1 checksum)
        CLR A
TEST_LP:ADD A, @R0
        INC R0
        DJNZ R2, TEST_LP
        
        JZ PASS
        MOV P1, #'B'      ; Failed (Bad)
        SJMP HALT
PASS:   MOV P1, #'G'      ; Passed (Good)
HALT:   SJMP HALT

        ORG 0250H
        DB 25H, 62H, 3FH, 52H
        END
```

---

### Question 3(c) [Marks: 4]
**Question:**
> *Find (i) the frequency and (ii) duty cycle of the wave generated by the code. Assume XTAL = 12 MHz.*

```assembly
        MOV  TMOD, #02H
        MOV  TH0, #-150
AGAIN:  SETB P1.3
        ACALL DELAY
        ACALL DELAY
        ACALL DELAY
        CLR  P1.3
        ACALL DELAY
        SJMP AGAIN
DELAY:  SETB TR0
BACK:   JNB  TF0, BACK
        CLR  TR0
        CLR  TF0
        RET
```

#### Mathematical Derivation:
- At $12\text{ MHz}$, $T_{\text{MC}} = 1.0\ \mu\text{s}$.
- Reload value $= -150 \implies \text{Timer counts } 150 \text{ cycles per overflow}$.
- Duration of one subroutine call:
  $$\text{Delay} = 150 \times 1.0\ \mu\text{s} = 150\ \mu\text{s}$$
- **High-Time ($T_{\text{HIGH}}$):** 3 calls to `DELAY` $= 3 \times 150\ \mu\text{s} = \mathbf{450\ \mu s}$.
- **Low-Time ($T_{\text{LOW}}$):** 1 call to `DELAY` $= 1 \times 150\ \mu\text{s} = \mathbf{150\ \mu s}$.
- **Total Period ($T$):** $450\ \mu\text{s} + 150\ \mu\text{s} = \mathbf{600\ \mu s}$.
- **Frequency ($f$):**
  $$f = \frac{1}{600\ \mu\text{s}} = \frac{10^6}{600} \approx \mathbf{1.667\text{ kHz}}$$
  *(Note: If calculated using 11.0592 MHz crystal where $T_{\text{MC}} = 1.085 \ \mu\text{s}$, $T = 651\ \mu\text{s} \implies f = 1.536\text{ kHz}$).*
- **Duty Cycle:**
  $$\text{Duty Cycle} = \frac{T_{\text{HIGH}}}{T} \times 100\% = \frac{450}{600} \times 100\% = \mathbf{75\%}$$

---

### Question 4(a) [Marks: 2]
**Question:**
> *If SMOD in PCON is set, find TH1 value (decimal and hex) to set Baud Rate to (i) 9600, (ii) 4800 at XTAL = 11.0592 MHz.*

#### Derivation:
With $\text{SMOD} = 1$:
$$\text{Base Frequency} = \frac{11.0592\text{ MHz}}{12 \times 16} = \mathbf{57,600\text{ Hz}}$$
$$256 - \text{TH1} = \frac{57,600}{\text{Baud Rate}}$$
1. **For 9600 Baud:**
   $$256 - \text{TH1} = \frac{57,600}{9600} = 6 \implies \text{TH1} = 256 - 6 = \mathbf{250_{10} = FAH \ (-6)}$$
2. **For 4800 Baud:**
   $$256 - \text{TH1} = \frac{57,600}{4800} = 12 \implies \text{TH1} = 256 - 12 = \mathbf{244_{10} = F4H \ (-12)}$$

---

### Question 4(b) [Marks: 5]
**Question:**
> *8051 serial port connected to PC at 4800 baud. (I) Send message "We Are Ready" to PC once, (II) Receive characters from PC and display on Port 1 LEDs continuously, (III) Read switches on Port 2 and transmit to PC serially continuously.*

#### Assembly Code:
```assembly
        ORG 0000H
        ; Configure Timer 1 for 4800 Baud (SMOD=0, TH1 = -6 = FAH)
        MOV TMOD, #20H        ; Timer 1 in Mode 2
        MOV TH1, #0FAH        ; 4800 baud at 11.0592 MHz
        MOV SCON, #50H        ; Mode 1, 8-bit UART, REN=1
        SETB TR1              ; Start Timer 1

        ; Part I: Send "We Are Ready" once
        MOV DPTR, #MSG
STR_LP: CLR A
        MOVC A, @A+DPTR
        JZ MAIN_LOOP
        MOV SBUF, A
WAIT_TX:JNB TI, WAIT_TX
        CLR TI
        INC DPTR
        SJMP STR_LP

        ; Part II & III: Continuous Echo and Switch Transmission
MAIN_LOOP:
        ; Check if PC sent data:
        JNB RI, READ_SW
        MOV A, SBUF           ; Read received byte
        MOV P1, A             ; Output to LEDs on P1
        CLR RI

READ_SW:
        MOV A, P2             ; Read switches on P2
        MOV SBUF, A           ; Send to PC serially
TX_SW:  JNB TI, TX_SW
        CLR TI
        SJMP MAIN_LOOP

MSG:    DB 'We Are Ready', 0
        END
```

---

### Question 5(b) [Marks: 7]
**Question:**
> *Trace memory map changes in internal RAM (00H to FFH) while T1ISR executes after 1 second at RETI statement.*

```assembly
              ORG 0000H
              LJMP MAIN
              ORG 001BH
              LJMP T1ISR
              ORG 0030H
MAIN:         MOV TMOD, #10H
              MOV IE, #88H
              MOV R0, #28
              MOV TL1, #00H
              MOV TH1, #00H
              SETB TR1
HERE:         SJMP HERE

              ORG 0200H
T1ISR:        DJNZ R0, START
              CPL P1.2
              MOV R0, #28
              SETB 42H
              SETB 67H
              PUSH 0
              SETB PSW.4
              POP 0
START:        CLR TR1
              MOV TL1, #00H
              MOV TH1, #00H
              SETB TR1
              RETI
              END
```

#### Detailed Memory Map Trace:
1. **Bit `42H` Set:**
   - Bit `42H` is bit $D_2$ of RAM byte `28H`.
   - $\mathbf{RAM[28H] = 04H}$ ($0000 \ 0100_2$).
2. **Bit `67H` Set:**
   - Bit `67H` is bit $D_7$ of RAM byte `2CH`.
   - $\mathbf{RAM[2CH] = 80H}$ ($1000 \ 0000_2$).
3. **Register Bank Switching (`SETB PSW.4`):**
   - Sets bit `RS1` (`PSW.4`), switching active bank to **Bank 2 (RAM `10H`–`17H`)**.
   - `PSW` register holds `10H` (address `D0H`).
4. **Stack & Register Context Transfer (`PUSH 0` followed by `POP 0`):**
   - When `PUSH 0` executed, active bank was Bank 0 $\implies$ pushed contents of `R0` (Bank 0, RAM `00H` = $28_{10} = 1\text{CH}$) onto stack!
   - Next, `SETB PSW.4` switches to Bank 2.
   - When `POP 0` executes, `R0` now refers to **Bank 2's `R0` (RAM location `10H`)**!
   - Therefore, the popped value ($1\text{CH}$) is written into **RAM `10H`**!
   - $\mathbf{RAM[10H] = 1CH}$.
5. **Stack Pointer (SP):**
   - Interrupt entry pushed 2-byte PC return address onto stack ($\text{SP}: 07\text{H} \to 09\text{H}$).
   - `PUSH 0` incremented SP to `0AH`.
   - `POP 0` decremented SP back to `09H`.
   - When execution hits `RETI`, the return address is popped and SP returns to **`07H`**.

---

## PART 5: Professor's High-Yield Memory Mnemonics & Formula Cheat Sheet

```
+-------------------------------------------------------------------------------+
|                      QUICK REVISION FORMULA CHEAT SHEET                       |
+-------------------------------------------------------------------------------+
| Machine Cycle Clock:    f_MC = f_XTAL / 12                                    |
| Machine Cycle Time:     T_MC = 12 / f_XTAL                                    |
| Timer Counts Required:  N = Desired_Delay / T_MC                              |
| Mode 1 Loaded Value:    M = 65,536 - N                                        |
| Mode 2 Loaded Value:    TH = 256 - N                                          |
| UART Baud Rate Formula: Baud = (2^SMOD / 32) * [ f_XTAL / (12 * (256-TH1)) ]  |
| Mode 1 Standard Th1:    9600 Baud -> TH1 = -3 (FDH)                           |
|                         4800 Baud -> TH1 = -6 (FAH)                           |
| CJNE Carry Status:      Dest < Src  ===> CY = 1                               |
|                         Dest >= Src ===> CY = 0                               |
| Signed Overflow (OV):   OV = C6 XOR C7                                        |
| Parity Bit (P):         P = 1 if Accumulator has ODD number of 1s             |
| RAM Bit to Byte Map:    Byte_Addr = 20H + (Bit_Addr / 8)                      |
|                         Bit_Pos   = Bit_Addr % 8                              |
+-------------------------------------------------------------------------------+
```
