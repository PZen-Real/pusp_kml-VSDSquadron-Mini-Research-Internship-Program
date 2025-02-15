# RISC-V INSTRUCTION TYPES
## RISC-V features a variety of instruction formats, which can be broadly classified into two categories:

Base Instruction Formats:
The base RV32I ISA defines four primary instruction formats: R-Type, I-Type, S-Type, and U-Type. Each of these instructions is 32 bits in length. The base ISA specifies IALIGN = 32, requiring instructions to be aligned to a four-byte boundary in memory. If an instruction is misaligned, an exception is triggered. This can result in a branch or an unconditional jump, formally referred to as an instruction-address-misaligned exception.


![image](https://github.com/user-attachments/assets/ba58b088-d2c8-4fe9-bf97-87fd1d983aef)

### In the RISC-V architecture, there are source registers (rs) and destination registers (rd):

* Source registers (rs): These registers hold the input values required for a specific operation or instruction.
* Destination register (rd): This register stores the result of the operation performed by the instruction.

### Other key components include:

* funct3: A 3-bit field that refers to a specific portion of an instruction containing essential information for the processor to execute it.
* funct7: A 7-bit field used similarly to funct3, often providing additional operation-specific details.
* opcode: An instruction field that specifies the operation to be performed by the processor.
* imm[x:y]: Represents an immediate value (a constant embedded directly within an instruction), derived from the bits in the instruction ranging from position y to position x.
* 
### The RISC-V ISA simplifies instruction decoding by maintaining the positions of source registers (rs1 and rs2) and the destination register (rd) consistent across all instruction formats.


# Immediate Encoding Variants
 In RISC-V, there are two additional instruction format variants specifically designed for handling immediate values: B-Type and J-Type. 




![image](https://github.com/user-attachments/assets/fda21dc2-3feb-49a3-9fd8-9af8128fd977)
![image](https://github.com/user-attachments/assets/b624e997-6112-40cd-9702-5d9823d0a4b7)

____________________________________________________________________________________________________________________________

### **R-TYPE** : 

![image](https://github.com/user-attachments/assets/5e7a9d07-6329-4bef-921f-fb0dae0cbe1f)


### The R-Type instruction format in the RISC-V architecture is used for arithmetic and logical operations involving registers.

* opcode (bits 6-0): Specifies the operation to be performed (e.g., addition, subtraction).
* rd (bits 11-7): Destination register to store the result of the operation.
* funct3 (bits 14-12): Determines the operation type within the opcode category.
* rs1 (bits 19-15): First source register containing one operand for the operation.
* rs2 (bits 24-20): Second source register containing the other operand.
* funct7 (bits 31-25): Provides additional encoding, allowing variations of instructions with the same opcode and funct3 values.




### **I-Type** :

![image](https://github.com/user-attachments/assets/9e4042df-ee63-4428-b8ba-95d41067dc30)

### I-Type
The I-Type instruction format is designed for operations involving immediate values and registers.

* opcode (bits 6-0): Specifies the operation to be performed (e.g., load, add immediate).
* rd (bits 11-7): Destination register to store the result.
* funct3 (bits 14-12): Determines the operation type within the opcode category.
* rs1 (bits 19-15): First source register containing an operand.
* imm[11:0] (bits 31-20): A 12-bit immediate value embedded directly within the instruction.

### **S-Type** :

![image](https://github.com/user-attachments/assets/0a634506-04d6-48f3-a015-90e53db5c9fd)

The S-Type instruction format is used for store operations, which transfer data from registers to memory.

* opcode (bits 6-0): Specifies the operation to be performed (e.g., SW - store word).
* imm[4:0] (bits 11-7): Lower 5 bits of the immediate value, representing an offset to be added to the base address in rs1.
* funct3 (bits 14-12): Determines the type of storage operation.
* rs1 (bits 19-15): Source register containing the base address.
* rs2 (bits 24-20): Source register containing the data to be stored in memory.
* imm[11:5] (bits 31-25): Upper 7 bits of the immediate value, completing the 12-bit offset.


### **U-Type** :

![image](https://github.com/user-attachments/assets/063ad859-81ec-4f4a-a303-f0f555936882)

The U-Type instruction format is designed for operations involving a 20-bit immediate value, such as loading upper immediate values into registers.

* opcode (bits 6-0): Specifies the operation (e.g., LUI - load upper immediate).
* rd (bits 11-7): Destination register to store the result.
* imm[31:12] (bits 31-12): A 20-bit immediate value loaded into the upper portion of the destination register.


### **B-Type** :

![image](https://github.com/user-attachments/assets/2ca54226-95ae-4225-aed6-a7bbc2363a82)

The B-Type instruction format is used for branch instructions, which control program flow based on conditions.

* opcode (bits 6-0): Specifies the operation (e.g., BEQ - branch if equal, BNE - branch if not equal).
* funct3 (bits 14-12): Determines the type of branch operation.
* rs1 (bits 19-15): First source register containing one operand for comparison.
* rs2 (bits 24-20): Second source register containing the other operand for comparison.
* imm[4:1] (bits 11-7): Lower bits of the immediate value, part of the branch offset.
* imm[10:5] (bits 30-25): Middle bits of the immediate value, completing the offset.
* imm[11] (1 bit): Sign bit of the immediate value, used to determine the direction of the branch.

### **J-Type** :

![image](https://github.com/user-attachments/assets/143c16f6-d955-4954-b870-ba0ef1616d87)

The J-Type instruction format is used for jump instructions, which unconditionally transfer control to a new instruction address.

* opcode (bits 6-0): Specifies the operation (e.g., JAL - jump and link).
* rd (bits 11-7): Destination register to store the return address.
* imm[19:12] (bits 31-12): Middle bits of the immediate value, part of the target jump address.
* imm[11] (1 bit): Sign bit of the immediate value, indicating the jump's direction.
* imm[10:1] (bits 30-21): Lower bits of the immediate value, part of the target address.
* imm[20] (1 bit): Bit used for sign extension when calculating the jump address.
_________________________________________________________________________________________________________________________
# Identifying atleast 15 of these instructions in a typical code

The C code is : 

          #include <stdio.h>

          int main() {
              int x = 5; // Number to compute the factorial
              int i;
              int factorial = 1; // Initialize factorial to 1
          
              for(i = 1; i <= x; i++) {
                  factorial *= i; // Multiply factorial by i in each iteration
              }
          
              printf("Factorial of %d is %d\n", x, factorial);
              return 0;
          }

Now we take the obj-dump of this code which is : 

![Screenshot 2024-12-27 141314](https://github.com/user-attachments/assets/0cb613ca-5116-44bd-ae6a-181309f1ebc1)

We will start to analyze unique instructions ,

### 1)The first instruction

           10184: ff010113 addi sp, sp, -16

the number 10184 represents the address 


the opperation **addi** adds an immediate value to a register ,

**sp** is the stack pointer register (stack pointer is a special-purpose register that holds the memory address of the top of the stack)

and -16 indicates the immidiate value to add to the current value 

Thus we can conclude this is an I-Type instruction

# Instruction Analysis in RISC-V Assembly
1) Instruction: 10184: ff010113 addi sp, sp, -16
* Address: 10184
* Operation: addi (Add Immediate)
Adds an immediate value (-16) to the value in the stack pointer (sp) register.
* Instruction Type: I-Type
* Immediate value -16, destination register sp, and source register sp.
* Hexadecimal: ff010113
* Binary: 1111 1111 0000 0001 0000 0001 0001 0011

These are  assembly language neumonics where the hexamdecimal instruction is given by **ff010113** which when conveted to binary is as follows :

Hex: `f f 0 1 0 1 1 3`

Binary: `1111 1111 0000 0001 0000 0001 0001 0011`

Code : `-16=111111110000, rs1=00010 ,funct3=000 ,rd=00010 ,opcode=0010011`


### 2)Now we move on to next instruction 

       10188: 00113423     sd     ra,8(sp) 

here the address is 10188 , 

**sd** is the store double which as the name suggests stores a double,

**ra** is the source register which contains the value to be stored ,
  
**8(sp)** indicates the offset of 8 to be  added to the surrent value in stack pointer register

Thus, we can conlude it is an s type of instruction 

These are  assembly language neumonics where the hexamdecimal instruction is given by **00113423** which when conveted to binary is as follows :

Hex: `0 0 1 1 3 4 2 3`

Binary: `0000 0000 0001 0001 0011 0100 0010 0011`

Code: `immediate = 0000000 ,rs2=00001 , rs1=00010 , funct3 =011, imm= 01000 ,Opcode = 0100011` 
       

### 3) The next instruction is 

            1018c: 03c00793 li a5,60
here the address is 1018c ,

**li** is a pseudo-instruction *(A pseudo-instruction is an assembly language command that does not correspond directly to a machine instruction. Instead, it simplifies programming by translating into one or more actual machine instructions during assembly.)* to load a constant value directly into a register

**a5** is a destination register where the immediate value is loaded

**60** is the immediate value

Thus we can conclude it is I-Type as the instruction involve one register as destination and one immediate value

These are  assembly language neumonics where the hexamdecimal instruction is given by **03c00793** which when conveted to binary is as follows :

Hex: `0 3 c 0 0 7 9 3`

Binary: `0000 0011 1100 0000 0000 0111 1001 0011`

Code : `immediate = 0000 0011 1100 ,rs1 = 00000,funct3 = 000,rd = 01111,Opcode = 0010011` 

### 4) The next instruction is 

            10190: fff7879b addiw a5,a5,-1  

here the address is 10190 ,

**addiw** indicates add immediate word,which adds a sign-extended 12-bit immediate to a 32-bit register

**a5** is both the source and destination register

**-1** is the immediate value to be added to the contents of register of  a5

Thus, it is a I-type instruction

These are  assembly language neumonics where the hexamdecimal instruction is given by **03c00793** which when conveted to binary is as follows :

Hex: `f f f 7 8 7 9 b`  

Binary: `1111 1111 1111 0111 1000 0111 1001 1011`

Code : `immediate = 1111 1111 1111 , rs1 = 01111 , funct3 = 000 , rd = 01111 , Opcode = 0011011` 


### 5) The next instruction is 

             10194: fe079ee3 bnez a5,10190 <main+0xc>

here the address is 10194 ,

**bnez** stands for branch not equal to zero which checks if the value in register a5 is non zero

if not equal to 0 it branches out to the address 10190 

**<main+0xc>** specifies an address that is 12 bytes beyond the start of the main function.

Thus , it is a B-type

Hex: `f e 0 7 9 e e 3`  

Binary: `1111 1110 0000 0111 1001 1110 1110 0011`

Code : `immmediate = 1 111111, rs2=00000 , rs1 = 01111 , funct3 = 001 ,imm = 1110 1  ,opcode = 1100011` 

### 6) The next unique instruction is 

             101a0: 00021537 lui a0,0x21

**lui** is used to load 20 bit immediate value into the upper 20 bits of a0 register 

**0x21** is the value which will be loaded 

Thus, its an U-type  instruction 

Hex: `0 0 0 2 1 5 3 7`  

Binary: `0000 0000 0000 0010 0001 0101 0011 0111`

Code : `imm = 0000 0000 0000 0010 0001 , rd = 01010  ,Opcode = 0110111 ` 

### 7) The next unique instruction is 

       101a8: 26c000ef jal ra,10414 <printf>

here the address is 101a8 ,

**jal** is "jump and link" , a instruction to jump to a specific address and simultaneously store the return address 

**ra** is the return address and 10414 is the target address(corresponding to the function printf)

Thus,it is a J-type instruction

Hex: `2 6 c 0 0 0 e f`  

Binary: `0010 0100 1100 0000 0000 0000 1110 1111`

Code : `imm = 0 0100100110 , imm = 0 0000 0000 ,rd = 00001 , Opcode = 110 1111 ` 

### 8) The next unique instruction is 

       101b0: 00813083 ld ra,8(sp)

here the address is 101b0 ,

**ld** is used to load a 64-bit value from memory into a specific refister

**ra** is the register where the value at address sp is at with offset of 8

This is I-Type instruction

Hex: `00813083`  

Binary: `0000 0000 1000 0001 0011 0000 1000 0011 `

Code : `imm = 0000 0000 1000, rs1 = 00010 , funct3 = 011 , rd = 00001 , Opcode = 0000011` 

### 9) The next unique code is 

          101b8: 00008067 ret 

here the address is 101b8 ,

**ret** is also an pseudo instruction , which is a short for jalr x0 , ra , 0 . This means that when the ret instruction is executed, it effectively jumps to the address stored in the return address register (ra, which is register x1) and sets the program counter (PC) to that address.

This is a J-Type instruction as it contains an immediate , destination register and an opcode .

Hex: `00008067`  

Binary: `0000 0000 0000 0000 1000 0000 0110 0111  `

Code : `immediate = 0 0000000000 0 00001000 , rd = 00000 , opcode =  1100111` 

### 10) The next unique code is 

              101bc: 00050593   mv  a1,a0

here the address is 101bc ,

The mv (move) instruction is a pseudo-instruction in RISC-V . 

This is an I - Type .

Hex: `0 0 0 5 0 5 9 3 `  

Binary: `0000 0000 0000  0101 0000 0101 1001 0011 `

Code : `immediate = 0000 0000 0000 , rs1 = 01010 , funct3 = 000 , rd = 01011 , Opcode = 0010011 ` 

### 11) The next unique code is 

             101c0: 00000693 li a3,0
here the address is 101c0 ,

**li** is again load immediate which loads 0 to register a3 .

This is an I - Type instruction.

Hex: `0 0 0 0 0 6 9 3 `  

Binary: `0000 0000 0000 0000 0000 0110 1001 0011 `

Code : `immmediate = 0000 0000 0000 , rs1 =  00000 , funct3 = 000 , rd = 01101 , opcode = 0010011` 


### 12) The unique instruction is 

           101cc: 4390206f j 12e04 <__register_exitproc>

The address 101cc .

**j** (jump) instruction is sed to perform an unconditional jump to a specificed address.

12e04 is the memory address where the function is .

<__register_exitproc> is the name of the function being refferd to .


This is an J-Type instruction .

Hex: `4 3 9 0 2 0 6 f`  

Binary: `0100 0011 1001 0000 0010 0000 0110 1111`

Code : `immmediate = 0 1000011100 1 00000010 , rd = 00000 , opcode = 110 1111 ` 

WE WILL NOW CHECK OTHER UNIQUE INSTRUCTIONS : 

![Screenshot 2025-01-02 151224](https://github.com/user-attachments/assets/691bcef8-01d2-43a3-bbe7-359cd13ee172)

### 13) The unique code in this is 

          100e4: ffff0797  auipc a5,0xffff0

The address is 100e4 
**auipc** is used to generate a PC(program counter) - relative address by adding 20-bit immediate value to the upper 20 bits of program couter 

**a5** is the register where the result is stored

**0xffff0** is the immediate value

This is an U-Type of instruction .

Hex: `f f f f 0 7 9 7`  

Binary: `1111 1111 1111 1111 0000 0111 1001 0111`

Code : `imm = 1111 1111 1111 1111 0000 , rd = 01111 , Opcode = 0010111 ` 

### 14) Another unique code is 

                 100ec: 00078863  beqz a5,100fc <register_fini+0x18>

the address here is 100ec ,

**beqz** (branch if equal to zero) is a pseudoinstruction that checks if the value of the given register is 0 or not , in this case a5

If 0 the PC is updated to the target address 100fc 

Thus it is an B-Type instruction.

Hex: `0 0 0 7 8 8 6 3`  

Binary: `0000 0000 0000 0111 1000 1000 0110 0011`

Code : `imm = 0 000000 , rs2 = 00000 , rs1 = 01111 , funct3 =  000 ,  imm =1000 0 ,Opcode = 1100011 ` 

### 15) Another unique code is 

              10114: 40a60633  sub a2,a2,a0

the address here is 10114 , 

**sub** is an instruction used to subtract the value of one register from another and store in a register , here a0 - a2 and the result is stored in a2 itself

Thus its an R-Type instruction .

Hex: ` 4 0 a 6 0 6 3 3 `  

Binary: `0100 0000 1010 0110 0000 0110 0011 0011`

Code : `funct7 = 0100000 , rs2 = 01010 , rs1 = 01100 , funct3 = 000 , rd =  01100 , Opcode =  011 0011 ` 
