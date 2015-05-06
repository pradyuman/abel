####This ABEL program creates a simple computer that utilizes 7-bit instructions and sports 14 memory SRAM locations.

The target CPLD is: ispMACH LC4256ZE 144-pin TQFP (LC4256ZE-5TN144C)

This simple computer has many different parts:
* **Address bus:** *A data pipeline that transfers address information to the memory unit from the program counter and the instruction register.*
* **Data bus:** *A data pipeline that transfers general information to various parts of the computer.*
* **Program Counter:** *This is a 4-bit binary-UP counter that has an asynchronous reset and a tri-state output. This counter automatically points to the next instruction in the memory unit.*
* **Instruction Register:** *This unit temporarily stages the instruction fetched from memory so that it can be decomposed and executed. It is a series of D flip-slops with two control signals.*
* **Memory:** *This computer has 14 SRAM (static random access memory) locations and has address and data bus connections. Each SRAM location has a memory cell that consists of seven D-latches and a tri-state buffer. The entire memory unit is constructed by combining the memory cells with a large decoder and some additional logic.*
* **Arithmetic Logic Unit:** *This unit is 4 bits wide and performs arithmetic and logical operations as defined by the instruction set. It sets four condition code bits (Carry Flag, Negative Flag, Zero Flag, Overflow Flag) and includes an accumulator register (A-REGISTER) that stores data while it is being processed.*
* **Instruction Decoder and Micro-Sequencer (IDMS):** *This unit orchestrates the sequencing of all the activities on the computer. This simple computer has two types of cycles: fetch and execute. As such, the IDMS includes a single flip-flop that is used as a state counter and is toggled between '0' (fetch) and '1' (execute). The IDMS also completes various other tasks necessary for the computer to operate.*

The instructions to this computer are 7 bits long and consist of a 3 bit opcode field and a 4 bit address. The commands are:

| Mnemonic | Opcode | Description |
| :------: | :----: | :---------: |
| HLT | 000 | Halt execution |
| LDA | 001 | Load data bus values into *A-REGISTER* |
| ADD | 010 | Add the information on the data bus with the information on the *A-REGISTER* and store the result in the *A-REGISTER* |
| SUB | 011 | Subtract the information on the data bus with the information on the *A-REGISTER* and store the result in the *A-REGISTER* |
| AND | 100 | Logical-AND the information on the data bus with the information on the *A-REGISTER* and store the result in the *A-REGISTER* |
| STA | 101 | Store the information on the *A-REGISTER* into a specified address location in memory |
| INA | 110 | Load the input values (from *DIP* switch) into the *A-REGISTER* |
| OUT | 111 | Output the *A-REGISTER* values to *LEDs* on the CPLD* |

The ALU utilizes radix arithmetic (two's complement) and has four control signals:
* **ALE:** *ALU Enable*
* **ALX:** *Function select*
* **ALY:** *Function select*
* **AOE:** *A-REGISTER output to the data bus*

The ALU functions according to the following description:

|  ALE  |  ALX  |  ALY  |  Mnemonic  |           Function Performed           |    CF	 |    NF	|    ZF	   |    VF	  |
| :---: | :---: | :---: | :--------: | :------------------------------------: | :------: | :------: | :------: | :------: |
| 0     | d     | d     | -          | *stay in the same state*   			  | -    	 | -    	| -    	   | - 	      |
| 1     | 0     | 0     | ADD        | (A) &#x2190; (A) + (*data bus*)  	  | &#x2195; | &#x2195; | &#x2195; | &#x2195; |
| 1     | 0     | 1     | SUB        | (A) &#x2190; (A) - (*data bus*)  	  | &#x2195; | &#x2195; | &#x2195; | &#x2195; |
| 1     | 1     | 0     | AND        | (A) &#x2190; (A) &#x2229; (*data bus*) | -    	 | &#x2195; | &#x2195; | -    	  |
| 1     | 1     | 1     | LDA        | (A) &#x2190; (*data bus*)        	  | -    	 | &#x2195; | &#x2195; | -   	  |
