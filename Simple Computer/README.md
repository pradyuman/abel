This ABEL program creates a simple computer that utilizes 7-bit instructions and sports 14 memory SRAM locations.

The target CPLD is: ispMACH LC4256ZE 144-pin TQFP (LC4256ZE-5TN144C)

This simple computer has many different parts:

The instructions to this computer are 7 bits long and consist of a 3 bit opcode field and a 4 bit address.

The commands are:

*(HLT | opcode 000) Halt execution
*(LDA | opcode 001) Load data bus values into A-REGISTER
*(ADD | opcode 010) Add the information on the data bus with the information on the A-REGISTER and store the result in the A-REGISTER
*(SUB | opcode 011) Subtract the information on the data bus with the information on the A-REGISTER and store the result in the A-REGISTER
*(AND | opcode 100) Logical-AND the information on the data bus with the information on the A-REGISTER and store the result in the A-REGISTER
*(STA | opcode 101) Store the information on the A-REGISTER into a specified address location in memory
*(INA | opcode 110) Load the input values (from DIP switch) into the A-REGISTER
*(OUT | opcode 111) Output the A-REGISTER values to LEDs on the CPLD

The ALU is 4 bits wide and utilizes radix arithmetic (two's complement). The ALU functions according to the following description:
