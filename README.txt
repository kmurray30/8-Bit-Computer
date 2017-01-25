Author: Kyle Murray
12-10-16


The computer created in Logisim has a word size of 8 and an accessible address space of 16 (since we use at most 3 bits in any instruction to load or store). It has 8 possible instructions and only 2 registers.

Instructions for using the computer:

Open “8-bit computer” main circuit

Right click Memory in bottom left corner and select “View Memory”

Instructions are included inside Memory circuit, along with sample code

Run program in “8-bit computer” by running simulation or manually clicking the clock

For the ADD instruction example provided, you can stop running once the Clock Cycle Counter hits 00010110, then view the memory to see the results

You can hit Ctr+R at any point to reset simulation, just make sure to fill memory with instructions and values before starting the clock


Instruction code with examples are also included below:

ADD	[000][DR][SR1][00][SR2]
ADD	[001][DR][SR][imm3]
AND	[010][DR][SR1][00][SR2]
AND	[011][DR][SR][imm3]
SUB	[100][DR][SR1][00][SR2]
NOT	[101][DR][SR][000]
LD	[110][DR][Address4]
ST	[111][SR][Address4]

Example ADD program (fill top row of memory with hex values, fill bottom right two addresses with values to add)

LD	R0, #1110	ce
LD	R1, #1111	df
ADD	R0, R0, R1	08
ST	R0, #1101	ed

Example AND

LD	R0, #1110	ce
ADD	R0, R0, #5	65
ST	R0, #1101	ed

Example Subtract

LD	R0, #1110	ce
LD	R1, #1111	df
SUB	R0, R1, R0	88
ST	R0, #1101	ed



Instruction Cycle Explained:

Fetch takes 3 clock cycles. First, the address in the PC is loaded into the MAR. Then the address in the MAR is accessed in memory and stored in the MDR. Finally the IR is loaded with the contents of the MDR. This is 3 cycles. Sometimes accessing the computer’s memory can take more than one cycle, so this is variable, but in this example it takes 3. While the is happening the PC is incremented. The reason for the numerous clock cycles is that different components of the computer communicate with each other through the BUS, so having them occur synchronously at different times ensures they do not compete with each other.

The next phases- decoding, evaluate address, fetch operands, and execute all depend on the type of operation being performed. In the add instruction these all take zero clock cycles since the registers can be accessed immediately and ALU result is calculated asynchronously. It then takes one more clock cycle to perform the final phase Store Result by storing that result into the destination register via the BUS.

For a load or store, it takes one cycle to evaluate the address (loading the MAR), then if it is a Store it takes one cycle to Load the MDR and one more to store that result in memory at the address pointed to by the MAR. For Load it takes one cycle to load the MDR with the contents of MEM[MAR], then one more to store those in the destination register. All in all, most ALU operations take about 4 clock cycles, whereas loading and storing take about 6, at least in this model. Again, it depends on the architecture of the computer as well.