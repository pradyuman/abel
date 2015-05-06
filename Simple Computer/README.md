####This ABEL program creates a simple computer that utilizes 7-bit instructions and sports 14 memory SRAM locations.

The target CPLD is: ispMACH LC4256ZE 144-pin TQFP (LC4256ZE-5TN144C)

This simple computer has many different parts:
* **Address bus:** *A data pipeline that transfers address information to the memory unit from the program counter and the instruction register (Implemented using a multiplexer).*
* **Data bus:** *A data pipeline that transfers general information to various parts of the computer (Implemented using a multiplexer).*
* **Program Counter:** *This is a 4-bit binary-UP counter that has an asynchronous reset and a tri-state output. This counter automatically points to the next instruction in the memory unit.*
* **Instruction Register:** *This unit temporarily stages the instruction fetched from memory so that it can be decomposed and executed. It is a series of D flip-slops with two control signals.*
* **Memory:** *This computer has 14 SRAM (static random access memory) locations and has address and data bus connections. Each SRAM location has a memory cell that consists of seven D-latches and a tri-state buffer. The entire memory unit is constructed by combining the memory cells with a large decoder and some additional logic.*
* **Arithmetic Logic Unit:** *This unit is 4 bits wide and performs arithmetic and logical operations as defined by the instruction set. It sets four condition code bits (Carry Flag - CF, Negative Flag - NF, Zero Flag - ZF, Overflow Flag - VF) and includes an accumulator register (A-register) that stores data while it is being processed.*
* **Instruction Decoder and Micro-Sequencer (IDMS):** *This unit orchestrates the sequencing of all the activities on the computer. This simple computer has two types of cycles: fetch and execute. As such, the IDMS includes a single flip-flop that is used as a state counter and is toggled between '0' (fetch) and '1' (execute). The IDMS also completes various other tasks necessary for the computer to operate.*

The instructions to this computer are 7 bits long and consist of a 3 bit opcode field and a 4 bit address. The commands are:

| OPCODE | Mnemonic | Function Performed | Description |
| :------: | :----: | :----------------: | :--------- |
| 000 | HLT 	 | *halt execution* | *retain state* |
| 001 | LDA addr | (A) &#x2190; (*addr*) | Load data at *address* into *A-register* |
| 010 | ADD addr | (A) &#x2190; (A) + (*data bus*) | Add the information at *address* with the information on the *A-register* and store the result in the *A-register* |
| 011 | SUB addr | (A) &#x2190; (A) - (*data bus*) | Subtract the information at *address* with the information on the *A-register* and store the result in the *A-register* |
| 100 | AND addr | (A) &#x2190; (A) &#x2229; (*data bus*) | Logical-AND the information at *address* with the information on the *A-register* and store the result in the *A-register* |
| 101 | STA addr | (*addr*) &#x2190; (A) | Store the information on the *A-register* into a specified address location in memory |
| 110 | INA 	 | (A) &#x2190; [DIP3..DIP0] | Load the input values (from *DIP* switch) into the *A-register* |
| 111 | OUT 	 | [LED27..LED24] &#x2190; (A)| Output the *A-register* values to *LEDs* on the CPLD |

The ALU utilizes radix arithmetic (two's complement) and has four control signals:
* **ALE:** *ALU Enable*
* **ALX:** *Function select*
* **ALY:** *Function select*
* **AOE:** *A-register output to the data bus*

The ALU functions according to the following description:

|  ALE  |  ALX  |  ALY  |  Mnemonic  |           Function Performed           |    CF	 |    NF	|    ZF	   |    VF	  |
| :---: | :---: | :---: | :--------: | :------------------------------------: | :------: | :------: | :------: | :------: |
| 0     | d     | d     | -          | *stay in the same state*   			  | -    	 | -    	| -    	   | - 	      |
| 1     | 0     | 0     | ADD        | (A) &#x2190; (A) + (*data bus*)  	  | &#x2195; | &#x2195; | &#x2195; | &#x2195; |
| 1     | 0     | 1     | SUB        | (A) &#x2190; (A) - (*data bus*)  	  | &#x2195; | &#x2195; | &#x2195; | &#x2195; |
| 1     | 1     | 0     | AND        | (A) &#x2190; (A) &#x2229; (*data bus*) | -    	 | &#x2195; | &#x2195; | -    	  |
| 1     | 1     | 1     | LDA        | (A) &#x2190; (*data bus*)        	  | -    	 | &#x2195; | &#x2195; | -   	  |

Usage:

The first four LEDs on the top LED row correspond to the ALU flags (CF, NF, ZF, VF). The last four LEDs on the top LED row correspond to the *A-register* value.

The first four LEDS on the middle LED row show the cycle state (fetch or execute) of a specific instruction. The last four LEDs on the middle LED row correspond to the values on the instruction register.

The first four LEDs on the bottom LED row act as the output port and show any information that the computer outputs during an output command. The last four LEDs on the bottom LED row correspond to the [DIP3..DIP0] values.

The computer displays a lot of valuable information on the displays. The first display shows the current state of the program counter when in the run mode (and does not display anything in memory edit mode).
The second display shows a hexadecimal number from 1 to D that corresponds to a relative memory location in the computer (memory location 1 to 14) when in memory edit mode (and does not display anything in run mode).
The third display shows the opcode stored at the memory address using a decimal number (first three bits of the instruction). 
The last display will show you what address is stored in the memory location using a decimal number (last 4 bits of the instruction). 

Set DIP7 = 0 to enter memory edit mode so that you can enter in a series of instructions using [DIP6..DIP0] \(this is the 7 bit instruction that has a 3 bit opcode and an optional 4 bit address). This simple computer supports a maximum of 14 instructions. 
Use S1 to save the [DIP6..DIP0] switch values into a memory location and use S2 to cycle through the memory locations.

Set DIP7 = 1 to enter run mode. Clock through the instructions using S2. The computer will automatically perform the instructions that you saved into memory when in memory edit mode. Assert S1 to reset the program counter and start from memory location (and instruction) one.
