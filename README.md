 🚀Stacks in ARMCortex-M(x) processors>>

 Our ex will be on Cortex-M4 processor - STM32f446 microcontroller (Nucleo board). 



⚡ The processor uses a full descending stack. this means the stack pointer holds the address of the last stacked item in memory.



⭐ processor implements two stacks, the main stack (msp) and the process stack (psp).



>>In Thread mode, the CONTROL register controls whether the processor uses the (msp) or the (psp).

>>In Handler mode, the processor always uses the main stack.



📢 In Bit[1] of CONTROL register.

      • 0 = Main Stack Pointer (MSP). This is the reset value.

      • 1 = Process Stack Pointer (PSP).

       

📌 In our ex we take 1kbyte from stack from the end and divide it into two equal sections:



1️⃣ MSP section which is hold the address of (RAM_END) and it will take 512 byte, because in default initialization processor take the value of 1st address of flash region and put it into MSP.



2️⃣ PSP section which is hold the address of (RAM_END + 512) and it will take 512 byte too.

then by default when the program is running the value in (SP) is the same value in (MSP) we need to link (SP) with (PSP) to do it, 
we must intialize (PSP) with any value once you do it you will can link it with (SP).

**__asm volatile(".equ RAM_END, (0x20000000U + (128*1024)) "); // represents the preprocessor in assembly 

then we link (PSP) with (SP) by Putting 0x02 in CONTROL REG 
__asm volatile ("LDR R0, =#0x02");
	__asm volatile ("MSR CONTROL, R0");

 int func (multiply) >> proccessor is in thread mode and (PSP) Link with (SP),
 once func of generate interrupt executes the processor will be in handelr mode (MSP) will link with (SP).

