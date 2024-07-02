# VSD_squadron_research_intern
**ABOUT:** **The program is based on the RISC-V architecture and uses open-source tools to teach people about VLSI chip design and RISC-V.**

###### VSD squadron mini board

<img align="center" width="500" height="300" src="https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/eb05f13c-0acb-4d20-b5cd-9c287286f0ac">


### Features
___

+ 32-bit RISC-V2A processor, supporting 2 levels of interrupt nesting
+ Maximum 48MHz system main frequency
+ 2KB SRAM, 16KB Flash
+ Power supply voltage: 3.3/5V
+ Multiple low-power modes: Sleep, Standby
+ Power on/off reset, programmable voltage detector
+ 1 group of 1-channel general-purpose DMA controller
+ 1 group of op-amp comparator
+ 1 group of 10-bit ADC
+ 1×16-bit advanced-control timer, 1×16-bit general-purpose timer
+ 2 WDOG, 1×32-bit SysTick
+ 1 USART interface, 1 group of I2C interface, 1 group of SPI interface
+ 18 I/O ports, mapping an external interrupt
+ 64-bit chip unique ID
+ 1-wire serial debug interface (SDI)
---
<details>
  <summary><b>
    Task 1: Sample Program and RISC-V compilation
</b></summary>
  
__The task 1 consist of some of the basic installation operation of the necessary tools such as Ubuntu on VMBox, Visual C++. Then we have to write a sample C code and analysing RISC asssemby code for the sample C code.__
___
+ Writing a C code to count sum of numbers from 1 to n using Leafpad as shown below.

![sample C program on Leafpad](https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/1a3edd12-338c-4ba9-9af9-a55d7460c0c1)
---

 #### Running above program in RISC-V Simulator
+ Command for Compiling the Code using RISCV Compiler.
```
$ riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o filename.o filename.c
$ ls -ltr filename.o
```

![RISC-V based simulation](https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/e43f4690-3310-4e73-8f4f-4a27d3f25827)

---
+ Assembly code for C program
```
$ riscv64-unknown-elf-objdump -d filename.o 
$ riscv64-unknown-elf-objdump -d filename.o | less
```

![Assembly code for C](https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/c36ebd0c-0b25-4037-97e6-ad018414bb42)

---

+ Assembly code for Ofast command
```
$ riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o filename.o filename.c
$ riscv64-unknown-elf-objdump -d filename.o | less 
```
![Assembly code for Ofast command](https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/cdea39c3-8202-4fe5-985e-a7c7fcd53955)

---

**END OF TASK-1**

---
</details>

<details>
  <summary><b>Task 2: Compilation of C Program for creating a Smart Elevator Controller and RISC-V compilation</b></summary>

  + __The task 2 involves writing a C Program for creating a Smart Elevator Controller and we have to analyze RISC-V asssemby code for the above C code.__

#### Objective:
The C code must implement a simple smart elevator controller, designed to handle basic elevator operations including floor requests, movement, and stopping at requested floors. This system simulates how a real-world elevator might function, focusing on simplicity and fundamental concepts.

#### Detailed Operation:
1.Initialization:
The elevator starts at floor 0.
An array of boolean values (requests) is used to keep track of which floors have been requested.

2.User Interaction:
The user is continuously prompted to enter a floor number to request.
Valid floor numbers (within the range 0 to 9) are accepted and recorded as requests.
Entering -1 exits the program.

3.Request Handling:
The request_floor function marks a floor as requested.
The move_elevator function processes these requests, moving the elevator to the appropriate floor and changing direction as needed.

4.Simulation of Movement:
The elevator checks for the nearest requested floor in the current direction.
It moves to that floor, stops, and clears the request.
If no requests are pending in the current direction, it changes direction and continues checking for requests.

---

+ The C code for a Smart Elevator Controller can be elaborated as further:
```
#include <stdio.h>
#include <stdbool.h>

#define MAX_FLOORS 10

void request_floor(int floor);
void move_elevator();
void stop_elevator(int floor);

int current_floor = 0;
bool requests[MAX_FLOORS] = { false };
bool moving_up = true;

int main() {
    int floor_request;

    while (1) {
        printf("Enter the floor number to request (0-%d) or -1 to exit: ", MAX_FLOORS - 1);
        scanf("%d", &floor_request);

        if (floor_request == -1) {
            break;
        } else if (floor_request >= 0 && floor_request < MAX_FLOORS) {
            request_floor(floor_request);
        } else {
            printf("Invalid floor number. Please try again.\n");
        }

        move_elevator();
    }

    return 0;
}

void request_floor(int floor) {
    requests[floor] = true;
    printf("Floor %d requested.\n", floor);
}

void move_elevator() {
    if (moving_up) {
        for (int i = current_floor + 1; i < MAX_FLOORS; i++) {
            if (requests[i]) {
                current_floor = i;
                requests[i] = false;
                stop_elevator(i);
                return;
            }
        }
        moving_up = false;  
    }

    if (!moving_up) {
        for (int i = current_floor - 1; i >= 0; i--) {
            if (requests[i]) {
                current_floor = i;
                requests[i] = false;
                stop_elevator(i);
                return;
            }
        }
        moving_up = true;  
    }
}

void stop_elevator(int floor) {
    printf("Stopping at floor %d.\n", floor);
}
```

---

+ Output for the above C program can be displayed as:
  
![Screenshot 2024-06-25 105635](https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/a2c93947-3765-403e-8a8c-57eb5fbcbca4)

---

#### __Detailed Steps of the Example:__
+ First Input: User requests floor 5.  
  Output: "Floor 5 requested."  
  Output: "Stopping at floor 5."
+ Second Input: User requests floor 2.   
  Output: "Floor 2 requested."  
  Output: "Stopping at floor 2."
+ Exit: User inputs -1 to exit.  
  Program terminates.
---

#### __Code Behavior:__

+ The elevator starts at floor 0.
+ The elevator handles requests sequentially, moving to the next requested floor in the direction it is currently moving.
+ When no further requests are in the current direction, it changes direction and checks for requests in the opposite direction.
+ The process continues until the user decides to exit by entering -1.

---

+ Display the content of a file by using  `cat smart_elevator.c` , where The cat command is a versatile companion for various file-related operations.
  
![Screenshot 2024-06-25 110751](https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/457333e2-4346-4401-ac45-dcf55a0352bc)

---

#### Running above program in RISC-V Simulator
+ Command for Compiling the Code using RISCV Compiler.

```
$ riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o filename.o filename.c
$ ls -ltr filename.o
```

![Screenshot 2024-06-25 110810](https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/47c0e571-0a96-46cd-a249-49faf8d6b30d)

---

+ Assembly code of C program for '/main' module:
```
$ riscv64-unknown-elf-objdump -d filename.o 
$ riscv64-unknown-elf-objdump -d filename.o | less
```

![Screenshot 2024-06-25 111554](https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/58498d90-4712-4220-961b-d183503e4348)

---

+ Assembly code of C program for 'Ofast' command:
```
$ riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o filename.o filename.c
$ riscv64-unknown-elf-objdump -d filename.o | less 
```

![Screenshot 2024-06-25 112152](https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/b32e4d3c-3d41-43e2-9b73-05a389a28185)

---

#### __Conclusion:__

+ The provided C code for a smart elevator controller is a foundational example of how an elevator system can be simulated using basic programming constructs. It offers a practical demonstration of state management, control flow, and user interaction, making it a valuable educational tool and a basis for further development and enhancement in more complex applications.

---

**END OF TASK-2**

---
</details>

<details>
  <summary><b>Task 3: Execution of SPIKE Simulation and verification with O1 and Ofast command along with running the RISC-V.</b></summary>

 + __The task 3 involves the execution of spike simulation and also consisting of debug of the Assembly code that is generated for the previous program.__

---

   First of all we have to execute the program using the step by step process such as, calling the program through ```riscv64-unknown-elf-gcc -o1 -mabi=lp64 -march=rv64i -o smart_elevator.o smart_elevator.c```, then compile it by using commands such as ```gcc smart_elevator.c``` and  ```./a.out```.

   Similarly again run the same program by using the spike simulation command such that ```spike pk smart_elevator.o``` .
   
   + One of the important goal is that output of both the simulation should return the same results as shown bellow.

![output verification through spike](https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/b14c2769-3d20-46c8-ae93-69a88ddb9a02)

---

+ Now we can access the Assembly code by using the command  ```riscv64-unknown-elf-objdump -d smart_elevator.o | less```.

![Screenshot 2024-06-26 171251](https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/ef8d2dfb-1435-4ed2-9c37-23b9516a478e)

---

__DEBUG PROCESS using ASSEMBLY CODE:__

+ The process of debugging involves the command  ```spike -d pk smart_elevator```, which debug's the assembly code and we can access each of the register by calling the variable name which is provided on the code. For example sp-Stack Pointer , lui - Load Upper Immediate , addi - Add Immediate ,  sw - Store Word etc...
  
  Now to initiate the file location we may use the command  ```(spike) until pc 0 100b0``` and then press enter and also use the command ```(spike) reg 0 sp```.

  ![debug assembly 1](https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/63f936fb-ab10-4156-aa60-478a53d927af)

+ Then the calculation of updation of the stack pointer can be done as following...

   ![stack pointer calculation](https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/645c6785-e4ec-42f3-a671-fa3e8a670257)

---

+ Further proceeding with the debug operation of the Assembly code as following..

  ![Screenshot 2024-06-26 174621](https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/33435ea2-d201-4be4-a129-caa24e750f03)

![Screenshot 2024-06-26 174855](https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/cdea8275-795f-4fee-a076-915ac4e17799)

---

**END OF TASK-3**

---

</details>

<details>
  <summary><b>Task 4: RISC-V Instruction Types and extraction of 32-bit instruction code in the instruction type format.</b></summary>

  + RISC-V instructions are grouped into different types based on their structure and purpose. Each type has a specific format that dictates how the instruction's bits are organized.
    These instruction types allow RISC-V to efficiently handle a wide range of operations, from basic arithmetic to complex control flow and memory access.
 Here are the basic RISC-V instruction types (Below response consist of block of command, inwhich first lines represents **Bit Numbers** and the second line represents the **Instruction set architectures**) :

---

#### 1.R-type (Register):

+ Purpose: Perform arithmetic and logical operations using values from two registers.

+ Format: Includes fields for the operation code (opcode), two source registers (rs1 and rs2), a destination register (rd), and function codes (funct3 and funct7) that specify the exact operation.

  Example: `add x1, x2, x3` (adds the values in `x2` and `x3`, and stores the result in `x1`).
```
 31   25   24  20  19  15  14  12   11  7   6    0
| funct7 |  rs2  |  rs1  | funct3 |  rd  |  opcode  |
```

#### 2.I-type (Immediate):

+ Purpose: Perform operations involving an immediate value (a constant) and a register, or load data from memory.
+ Format: Includes fields for the opcode, one source register (rs1), a destination register (rd), a function code (funct3), and an immediate value.

Example: `addi x1, x2, 10` (adds the value in `x2` and the immediate value 10, and stores the result in `x1`).
```
 31      20   19  15  14  12   11  7   6    0
| imm[11:0] |  rs1  | funct3 |  rd  |  opcode  |
```

#### 3.S-type (Store):

+ Purpose: Store data from a register into memory.
+ Format: Includes fields for the opcode, two source registers (rs1 and rs2), a function code (funct3), and a split immediate value.

Example: `sw x1, 0(x2)` (stores the value in `x1` into the memory address calculated as `0 + x2`).
```
 31      25   24  20  19  15  14    12   11    7    6   0
| imm[11:5] |  rs2  |  rs1  |  funct3  | imm[4:0] | opcode |
```

#### 4.B-type (Branch):

+ Purpose: Perform conditional branches based on comparisons.
+ Format: Includes fields for the opcode, two source registers (rs1 and rs2), a function code (funct3), and a split immediate value that determines the branch target.

Example: `beq x1, x2, label` (branches to label if the values in `x1` and `x2` are equal).
```
 31    30   25    24   20    19    15 14      12 11     8    7        6     0
| imm[12] | imm[10:5] |  rs2  | rs1 |  funct3  | imm[4:1] | imm[11] | opcode |
```

#### 5.U-type (Upper Immediate):

+ Purpose: Handle large immediate values, typically for addressing or arithmetic involving large constants.
+ Format: Includes fields for the opcode, a destination register (rd), and a large immediate value.

Example: `lui x1, 0x12345` (loads the immediate value `0x12345000` into `x1`).
```
 31        12 11   7 6      0
| imm[31:12] |  rd  | opcode |
```

#### 6.J-type (Jump):

+ Purpose: Perform jumps to a specified address and optionally link (store the return address).
+ Format: Includes fields for the opcode, a destination register (rd), and a large immediate value that determines the jump target.

Example: `jal x1, label` (jumps to label and stores the return address in `x1`).
```
 31       30         21       20  19       12 11   7 6    0
| imm[20] | imm[10:1] | imm[11] | imm[19:12] |  rd  | opcode |
```

---

+ **Extraction of 32-bit instruction code in the instruction type format.**

**1.ADD(addition)**

```
ADD r1, r2, r3
```
+ **Description:**
  - ADD r1, r2, r3: This instruction performs an arithmetic addition of the values in registers r2 and r3 and stores the result in register r1.
+ **Instruction Type:**
  - Type: R-type (Register)
+ **Format:**
  - R-type Format Fields:
    - opcode: Specifies the operation to be performed (for ADD, it is typically `0110011` in binary).
    - rd: The destination register (in this case, `r1`).
    - funct3: A 3-bit function code that, along with the opcode and funct7, uniquely identifies the instruction (for ADD, it is `000`).
    - rs1: The first source register (in this case, `r2`).
    - rs2: The second source register (in this case, `r3`).
    - funct7: A 7-bit function code that, along with the opcode and funct3, uniquely identifies the instruction (for ADD, it is `0000000`).

**2.SUB(subraction)**

```
SUB r3, r1, r2
```
+ **Description:**
  - SUB r3, r1, r2: This instruction performs an arithmetic subtraction of the value in register r2 from the value in register r1 and stores the result in register r3.
+ **Instruction Type:**
  - Type: R-type (Register)
+ **Format:**
  - R-type Format Fields:
    - opcode: Specifies the operation to be performed (for SUB, it is typically `0110011` in binary).
    - rd: The destination register (in this case, `r3`).
    - funct3: A 3-bit function code that, along with the opcode and funct7, uniquely identifies the instruction (for SUB, it is `000`).
    - rs1: The first source register (in this case, `r1`).
    - rs2: The second source register (in this case, `r2`).
    - funct7: A 7-bit function code that, along with the opcode and funct3, uniquely identifies the instruction (for SUB, it is `0100000`).

**3.AND(operation)**
```
AND r2, r1, r3
```
+ **Description:**
  - AND r2, r1, r3: This instruction performs a bitwise AND operation between the values in registers r1 and r3, and stores the result in register r2.
+ **Instruction Type:**
  - Type: R-type (Register)
+ **Format:**
  - R-type Format Fields:
    - opcode: Specifies the operation to be performed (for AND, it is typically `0110011` in binary).
    - rd: The destination register (in this case, `r2`).
    - funct3: A 3-bit function code that, along with the opcode and funct7, uniquely identifies the instruction (for AND, it is `111`).
    - rs1: The first source register (in this case, `r1`).
    - rs2: The second source register (in this case, `r3`).
    - funct7: A 7-bit function code that, along with the opcode and funct3, uniquely identifies the instruction (for AND, it is `0000000`).
   
**4.OR(operation)**
```
OR r8, r2, r5
```
+ **Description:**
  - OR r8, r2, r5: This instruction performs a bitwise OR operation between the values in registers r2 and r5, and stores the result in register r8.
+ **Instruction Type:**
  - Type: R-type (Register)
+ **Format:**
  - R-type Format Fields:
    - opcode: Specifies the operation to be performed (for OR, it is typically `0110011` in binary).
    - rd: The destination register (in this case, `r8`).
    - funct3: A 3-bit function code that, along with the opcode and funct7, uniquely identifies the instruction (for OR, it is `110`).
    - rs1: The first source register (in this case, `r2`).
    - rs2: The second source register (in this case, `r5`).
    - funct7: A 7-bit function code that, along with the opcode and funct3, uniquely identifies the instruction (for OR, it is `0000000`).
   
  **5.XOR(operation)**
  ```
  XOR r8, r1, r4
  ```
+ **Description:**
  - XOR r8, r1, r4: This instruction performs a bitwise XOR operation between the values in registers r1 and r4, and stores the result in register r8.
+ **Instruction Type:**
  - Type: R-type (Register)
+ **Format:**
  - R-type Format Fields:
  - opcode: Specifies the operation to be performed (for XOR, it is typically `0110011` in binary).
  - rd: The destination register (in this case, `r8`).
  - funct3: A 3-bit function code that, along with the opcode and funct7, uniquely identifies the instruction (for XOR, it is `100`).
  - rs1: The first source register (in this case, `r1`).
  - rs2: The second source register (in this case, `r4`).
  - funct7: A 7-bit function code that, along with the opcode and funct3, uniquely identifies the instruction (for XOR, it is `0000000`).
 
**6.SLT(set-less-than comparison)**
```
SLT r10, r2, r4
```
+ **Description:**
  - SLT r10, r2, r4: This instruction performs a set-less-than comparison between the values in registers r2 and r4. If the value in r2 is less than the value in r4, it sets the destination register r10 to 1; otherwise, it sets r10 to 0.
+ **Instruction Type:**
  - Type: R-type (Register)
+ **Format:**
  - R-type Format Fields:
    - opcode: Specifies the operation to be performed (for SLT, it is typically 0110011 in binary).
    -  rd: The destination register (in this case, r10).
    - funct3: A 3-bit function code that, along with the opcode and funct7, uniquely identifies the instruction (for SLT, it is 010).
    - rs1: The first source register (in this case, r2).
    - rs2: The second source register (in this case, r4).
    - funct7: A 7-bit function code that, along with the opcode and funct3, uniquely identifies the instruction (for SLT, it is `0000000`).
   
**7.ADDI(add immediate)**
```
ADDI r12, r3, 5
```
+ **Description:**
  - ADDI r12, r3, 5: This instruction performs an addition of the value in register r3 and the immediate value 5, and stores the result in register r12.
+ **Instruction Type:**
  - Type: I-type (Immediate)
+ **Format:**
  - I-type Format Fields:
    - opcode: Specifies the operation to be performed (for ADDI, it is typically `0010011` in binary).
    - rd: The destination register (in this case, `r12`).
    - funct3: A 3-bit function code that, along with the opcode, uniquely identifies the instruction (for ADDI, it is `000`).
    - rs1: The source register (in this case, `r3`).
    - imm[11:0]: A 12-bit immediate value (in this case, `5`).

  **8.SW(Store)**
  ```
  SW r3, r1, 4
  ```
  + **Description:**
    - SW r3, r1, 4: This instruction stores the value in register r3 to the memory address computed by adding the immediate value 4 to the base address in register r1.
  + **Instruction Type:**
    - Type: S-type (Store)
+ **Format:**
  - S-type Format Fields:
    - opcode: Specifies the operation to be performed (for SW, it is typically `0100011` in binary).
    - imm[11:5]: The upper 7 bits of the 12-bit immediate value.
    - rs2: The source register containing the value to be stored (in this case, `r3`).
    - rs1: The base address register (in this case, `r1`).
    - funct3: A 3-bit function code that, along with the opcode, uniquely identifies the instruction (for SW, it is `010`).
    - imm[4:0]: The lower 5 bits of the 12-bit immediate value.
   
+ **9.SRL(Shift Right Logical)**
  ```
  SRL r16, r11, r2
  ```
+ **Description:**
  - SRL r16, r11, r2: This instruction performs a logical right shift on the value in register r11 by the number of positions specified in register r2, and stores the result in register r16.
+ **Instruction Type:**
  - Type: R-type (Register)
+ **Format:**
  - R-type Format Fields:
    - opcode: Specifies the operation to be performed (for SRL, it is typically `0110011` in binary).
    - rd: The destination register (in this case, r16).
    - funct3: A 3-bit function code that, along with the opcode and funct7, uniquely identifies the instruction (for SRL, it is `101`).
    - rs1: The first source register (in this case, r11).
    - rs2: The second source register (in this case, r2).
    - funct7: A 7-bit function code that, along with the opcode and funct3, uniquely identifies the instruction (for SRL, it is `0000000`).
   
+ **10.BNE(Branch on Not Equal)**
 ```
BNE r0, r1, 20
```
+ **Description:**
  - BNE r0, r1, 20: This instruction compares the values in registers r0 and r1. If the values are not equal, it branches to the address specified by the immediate value 20 (which is a byte offset from the current program counter).
+ **Instruction Type:**
  - Type: B-type (Branch)
+ **Format:**
  - B-type Format Fields:
    - opcode: Specifies the operation to be performed (for BNE, it is typically `1100011` in binary).
    - imm[12|10:5]: The immediate value split into upper bits (bit 12 and bits 10 to 5).
    - rs2: The second source register (in this case, r1).
    - rs1: The first source register (in this case, r0).
    - funct3: A 3-bit function code that, along with the opcode, uniquely identifies the instruction (for BNE, it is `001`).
    - imm[4:1|11]: The immediate value split into lower bits (bits 4 to 1 and bit `11`).
   
+ **11.BEQ(branch if equal)**
  ```
  BEQ r0, r0, 15
  ```
+ **Description:**
  - BEQ r0, r0, 15: This instruction compares the values in registers r0 and r0. Since both are the same, it checks if they are equal, and if true, it branches to the address specified by the immediate value 15 (which is a byte offset from the current program counter).
+ **Instruction Type:**
  - Type: B-type (Branch)
+ **Format:**
  - B-type Format Fields:
    - opcode: Specifies the operation to be performed (for BEQ, it is typically 1100011 in binary).
    - imm[12|10:5]: The immediate value split into upper bits (bit 12 and bits 10 to 5).
    - rs2: The second source register (in this case, r0).
    - rs1: The first source register (in this case, r0).
    - funct3: A 3-bit function code that, along with the opcode, uniquely identifies the instruction (for BEQ, it is `000`).
    - imm[4:1|11]: The immediate value split into lower bits (bits 4 to 1 and bit `11`).
   
+ **12.LW(load)**
  ```
  LW r13, r11, 2
  ```
+ **Description:**
  - LW r13, r11, 2: This instruction loads a 32-bit word from memory at the address calculated by adding the immediate value 2 to the base address in register r11, and stores the result in register r13.
+ **Instruction Type:**
  - Type: I-type (Immediate)
+ **Format:**
  - I-type Format Fields:
    - opcode: Specifies the operation to be performed (for LW, it is typically `0000011` in binary).
    - rd: The destination register (in this case, r13).
    - funct3: A 3-bit function code that, along with the opcode, uniquely identifies the instruction (for LW, it is `010`).
    - rs1: The base address register (in this case, r11).
    - imm[11:0]: A 12-bit immediate value (in this case, 2).
      
---

#### CONCLUSION:

+ **The final declaration of instruction type format for given specific 32-bit instruction code of RISC-V instructions:**
  
| Instruction | Type | Instruction | Type |
|-------------| ------| -------------| ------|
| ADD r1, r2, r3 | R | ADDI r12, r3, 5 | I |
| SUB r3, r1, r2 | R | SW r3, r1, 4 | S |
| AND r2, r1, r3 | R | SRL r16, r11, r2 | R |
| OR r8, r2, r5 | R | BNE r0, r1, 20 | B |
| XOR r8, r1, r4 | R | BEQ r0, r0, 15 | B |
| SLT r10, r2, r4 | R | LW r13, r11, 2 | I |
| SLL r15, r11, r2 | R | 

---

**END OF TASK-4**

---
</details>

