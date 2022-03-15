DAY 17 - REVERSE ELFNEERING [REVERSE ENGINEERING]
----

# STORY :
----

>>McSkidy has never really touched low-level languages - this is something they must learn in their quest to defeat the Christmas monster.

----

# Introduction to x86-64 Assembly
----

>>Computers execute machine code, which is encoded as bytes, to carry out tasks on a computer. Since different computers have different processors, the machine code executed on these computers is specific to the processor. 
>>In this case, we’ll be looking at the Intel x86-64 instruction set architecture which is most commonly found today. Machine code is usually represented by a more readable form of the code called assembly code. This machine is code is usually produced by a compiler, which takes the source code of a file, and after going through some intermediate stages, produces machine code that can be executed by a computer.

>>Without going into too much detail, Intel first started out by building a 16-bit instruction set, followed by 32 bit, after which they finally created 64 bit. All these instruction sets have been created for backward compatibility, so code compiled for 32-bit architecture will run on 64-bit machines. As mentioned earlier, before an executable file is produced, the source code is first compiled into assembly(.s files), after which the assembler converts it into an object program(.o files), and operations with a linker finally make it an executable. 

>>The best way to actually start explaining assembly is by diving in. We’ll be using radare2 to do this - radare2 is a framework for reverse engineering and analysing binaries. It can be used to disassemble binaries(translate machine code to assembly, which is actually readable) and debug said binaries(by allowing a user to step through the execution and view the state of the program).

>>Let's proceed to run through how Radare2 works exactly. Although you shouldn't do this if the program is unknown, it is safe for us to execute to see what should be happening like so:

>>The above program shows that there are 3 variables(a, b, c) where c is the sum of a and b. 

>>Time to see what’s happening under the hood! Run the command r2 -d ./file1

>>This will open the binary in debugging mode. Once the binary is open, one of the first things to do is ask r2 to analyze the program, and this can be done by typing in: aa

>>Note, when using the aa command in radare2, this may take between 5-10 minutes depending on your system.

>>>Which is the most common analysis command. It analyses all symbols and entry points in the executable. The analysis, in this case, involves extracting function names, flow control information, and much more! r2 instructions are usually based on a single character, so it is easy to get more information about the commands.

>>I.e. For general help, we can run: ? or if we wish to understand more about a specific feature, we could provide a?

----

# Computer says...Done?!
----

>>Once the analysis is complete, you would want to know where to start analysing from - most programs have an entry point defined as main. To find a list of the functions run: afl

>>As seen here, there actually is a function at main. Let’s examine the assembly code at main by running the command pdf @main Where pdf means print disassembly function. Doing so will give us the following view:

----

## Register me this, register me that...
The core of assembly language involves using registers to do the following: 

   * Transfer data between memory and register, and vice versa
   * Perform arithmetic operations on registers and data
   * Transfer control to other parts of the program Since the architecture is x86-64, the registers are 64 bit and Intel has a list of 16 registers:

Initial Data Type			Suffix		Size (bytes)
		Byte				     b				1
		Word				     w				2
		Double Word	 		  1 	     	  	4
		Quad				     q				8
		Single Precision	  s				4
		Double Precision    1    			8

>>When dealing with memory manipulation using registers, there are other cases to be considered:

   * (Rb, Ri) = MemoryLocation[Rb + Ri]
   * D(Rb, Ri) = MemoryLocation[Rb + Ri + D]
   * (Rb, Ri, S) = MemoryLocation(Rb + S * Ri]
   * D(Rb, Ri, S) = MemoryLocation[Rb + S * Ri + D]

----

# Read the instructions!

>>Some other important instructions are:

   * leaq source, destination: this instruction sets destination to the address denoted by the expression in source
   * addq source, destination: destination = destination + source
   * subq source, destination: destination = destination - source
   * imulq source, destination: destination = destination * source
   * salq source, destination: destination = destination << source where << is the left bit shifting operator
   * sarq source, destination: destination = destination >> source where >> is the right bit shifting operator
   * xorq source, destination: destination = destination XOR source
   * andq source, destination: destination = destination & source
   * orq source, destination: destination = destination | source

>>Now let’s actually walk through the assembly code to see what the instructions mean when combined.

>>The line starting with sym.main indicates we’re looking at the main function. The next 3 lines are used to represent the variables stored in the function. The second column indicates that they are integers(int), the 3rd column specifies the name that r2 uses to reference them and the 4th column shows the actual memory location.

>>The first 3 instructions are used to allocate space on that stack (ensures that there’s enough room for variables to be allocated and more). We’ll start looking at the program from the 4th instruction (movl $4). We want to analyse the program while it runs and the best way to do this is by using breakpoints.

>>A breakpoint specifies where the program should stop executing. This is useful as it allows us to look at the state of the program at that particular point. So let’s set a breakpoint using the command db  in this case, it would be db 0x00400b55 To ensure the breakpoint is set, we run the pdf @main command again and see a little b next to the instruction we want to stop at.

>>Now that we’ve set a breakpoint, let’s run the program using dc

>>Running dc will execute the program until we hit the breakpoint. Once we hit the breakpoint and print out the main function, the rip which is the current instruction shows where execution has stopped. From the notes above, we know that the mov instruction is used to transfer values.  This statement is transferring the value 4 into thelocal_ch variable. To view the contents of the local_ch variable, we use the following instruction px @memory-address In this case, the corresponding memory address for local_ch will be rbp-0xc ( from the first few lines of @pdf main) This instruction prints the values of memory in hex:

>>This shows that the variable currently doesn’t have anything stored in it (it’s just 0000). Let’s execute this instruction and go to the next one using the following command (which only goes to the next instruction) ds If we view the memory location after running this command, we get the following:

>>We can see that the first 2 bytes have the value 4! If we do the same process for the next instruction, we’ll see that the variable local_8h has the value 5.

>>If we go to the instruction movl local_8h, %eax, we know from the notes that this moves the value from local_8h to the %eax register. To see the value of the %eax register, we can use the command:

>>If we execute the instruction and run the dr command again, we get:

>>This technically skips the previous instruction movl local_ch, %edx but the same process can be applied to it. Showing the value of rax (the 64 bit version) to be 5. We can do the same for similar instructions and view the values of the registers changing. When we come to the addl %edx, %eax, we know that this will add the values in edx and eax and store them in eax. Running dr shows us the rax contains 5 and rdx contains 4, so we’d expect rax to contain 9 after the instruction is executed:

>>Executing ds to move to the next instruction then executing dr to view register variable shows us we are correct:

>>The next few instructions involve moving the values in registers to the variables and vice versa:

>>After that, a string (which is the output is loaded into a register and the printf function is called in the 3rd line. The second line clears the value of eax as eax is sometimes used to store results from functions. The 4th line clears the value of eax. The 5th and 6th lines are used to exit the main function.

----

# To finalise our workflow...

>>The general formula for working through something like this is:

  * set appropriate breakpoints
  * use ds to move through instructions and check the values of register and memory
  * if you make a mistake, you can always reload the program using the ood command

>>You may find this radare2 cheatsheet useful in your adventures...

----

# Challenge
----

Use your new-found knowledge of Radare2 to analyse the "challenge1" file in the Instance MACHINE_IP that is attached to this task to answer the questions below.

----

#  MY DOCUMENTATION :
----

>>EXECUTING THE CHALLENGE1 FILE TO radare2 AND ANALYZING IT. 

```bash
elfmceager@tbfc-day-17:~$ file challenge1 
challenge1: ELF 64-bit LSB executable, x86-64, version 1 (GNU/Linux), statically linked, for GNU/Linux 3.2.0, BuildID[sha1]=884f57a67cddb0fc0104f1d556ab051183952324, not stripped
elfmceager@tbfc-day-17:~$ r2 -d ./challenge1 
Process with PID 1531 started...
= attach 1531 1531
bin.baddr 0x00400000
Using 0x400000
Warning: Cannot initialize dynamic strings
asm.bits 64
[0x00400a30]> aa
[ WARNING : block size exceeding max block size at 0x006ba220
[+] Try changing it with e anal.bb.maxsize
 WARNING : block size exceeding max block size at 0x006bc860
[+] Try changing it with e anal.bb.maxsize
[x] Analyze all flags starting with sym. and entry0 (aa)
```

>>NOW LISTING THE MAIN FILES AFTER ANALZING THE FILE.

```bash
[0x00400a30]> afl | grep main
0x00400b4d    1 35           sym.main
0x00400de0   10 1007 -> 219  sym.__libc_start_main
0x00403840   39 661  -> 629  sym._nl_find_domain
0x00403ae0  308 5366 -> 5301 sym._nl_load_domain
0x00415ef0    1 43           sym._IO_switch_to_main_get_area
0x0044ce10    1 8            sym._dl_get_dl_main_map
0x00470430    1 49           sym._IO_switch_to_main_wget_area
0x0048f9f0    7 73   -> 69   sym._nl_finddomain_subfreeres
0x0048fa40   16 247  -> 237  sym._nl_unload_domain
```

>>PRINTING THE MAIN FILE AND SEEING ITS DATA.

```bash 
[0x00400a30]> pdf @main
            ;-- main:
/ (fcn) sym.main 35
|   sym.main ();
|           ; var int local_ch @ rbp-0xc
|           ; var int local_8h @ rbp-0x8
|           ; var int local_4h @ rbp-0x4
|              ; DATA XREF from 0x00400a4d (entry0)
|           0x00400b4d      55             push rbp
|           0x00400b4e      4889e5         mov rbp, rsp
|           0x00400b51      c745f4010000.  mov dword [local_ch], 1
|           0x00400b58      c745f8060000.  mov dword [local_8h], 6
|           0x00400b5f      8b45f4         mov eax, dword [local_ch]
|           0x00400b62      0faf45f8       imul eax, dword [local_8h]
|           0x00400b66      8945fc         mov dword [local_4h], eax
|           0x00400b69      b800000000     mov eax, 0
|           0x00400b6e      5d             pop rbp
\           0x00400b6f      c3             ret
```

>>EXECUTING THE MAIN FILE dc AND TO SEE THE FURTHER VALUE THE MEMORY ADDRESS USE ds TO GO TO THE NEXT MEMORY ADDRESS AND USE dr TO SEE THE %eax VALUES.
>>THIS THE VALUE OF THIS MEMORY ADDRESS " 0x00400b51  c745f4010000. mov dword [local_ch], 1 ".

```bash

[0x00400b51]> ds
[0x00400b51]> dr
rax = 0x00000001
rbx = 0x00400400
rcx = 0x0044b9a0
rdx = 0x7fff935aecb8
r8 = 0x00000000
r9 = 0x00000001
r10 = 0x00000002
r11 = 0x00000000
r12 = 0x004018e0
r13 = 0x00000000
r14 = 0x006b9018
r15 = 0x00000000
rsi = 0x7fff935aeca8
rdi = 0x00000001
rsp = 0x7fff935aeb80
rbp = 0x7fff935aeb80
rip = 0x00400b62
rflags = 0x00000246
orax = 0xffffffffffffffff
```

>>THIS THE VALUE OF THIS MEMORY ADDRESS " 0x00400b58  c745f8060000. mov dword [local_8h], 6 ".

```bash

[0x00400b51]> ds
[0x00400b51]> dr
rax = 0x00000006
rbx = 0x00400400
rcx = 0x0044b9a0
rdx = 0x7fff935aecb8
r8 = 0x00000000
r9 = 0x00000001
r10 = 0x00000002
r11 = 0x00000000
r12 = 0x004018e0
r13 = 0x00000000
r14 = 0x006b9018
r15 = 0x00000000
rsi = 0x7fff935aeca8
rdi = 0x00000001
rsp = 0x7fff935aeb80
rbp = 0x7fff935aeb80
rip = 0x00400b66
rflags = 0x00000206
orax = 0xffffffffffffffff
```

>>THIS THE VALUE OF THIS MEMORY ADDRESS "  0x00400b62  0faf45f8  imul eax, dword [local_8h] ".

```bash
[0x00400b51]> ds
[0x00400b51]> dr
rax = 0x00000006
rbx = 0x00400400
rcx = 0x0044b9a0
rdx = 0x7fff935aecb8
r8 = 0x00000000
r9 = 0x00000001
r10 = 0x00000002
r11 = 0x00000000
r12 = 0x004018e0
r13 = 0x00000000
r14 = 0x006b9018
r15 = 0x00000000
rsi = 0x7fff935aeca8
rdi = 0x00000001
rsp = 0x7fff935aeb80
rbp = 0x7fff935aeb80
rip = 0x00400b69
rflags = 0x00000206
orax = 0xffffffffffffffff
[0x00400b51]> 

```

----

# ANSWER THE FOLLOWING :
----

1.What is the value of local_ch when its corresponding movl instruction is called (first if multiple)?
>>1

2.What is the value of eax when the imull instruction is called?
>>6

3.What is the value of local_4h before eax is set to 0?
>>6

----