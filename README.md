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


