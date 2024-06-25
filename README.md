# VSD_squadron_research_intern
**The program is based on the RISC-V architecture and uses open-source tools to teach people about VLSI chip design and RISC-V.**

###### VSD squadron mini board

![WhatsApp Image 2024-06-23 at 12 07 37 PM](https://github.com/sakthispgs/VSDsquadronmini_intern/assets/157115078/eb05f13c-0acb-4d20-b5cd-9c287286f0ac)

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


<details>
  <summary><b>Task 2: Compilation of C Program for creating a Smart Elevator Controller and RISC-V compilation</b></summary>
