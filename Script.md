# Computer Science Extended Essay:

Structure: introduction, background information, methodology, result & analysis, conclusion, bibliography.

## Investigating the Linux x86_64 and Linux aarch64 ISA differences in the context of ISA translation technology

## Introduction

Linux x86_64 and Fedora Linux aarch64 are two different architectures with distinct instruction sets. While x86_64 is based on the x86 architecture, aarch64 is based on the ARMv8-A architecture. As a result, there are no **direct equivalents** for x86_64 instructions in Fedora Linux aarch64.

However, the Linux kernel and many user-space applications provide emulation and translation mechanisms to run x86_64 code on aarch64 platforms. This is achieved through:

- Hardware Virtualization: QEMU, a popular emulator, can run x86_64 code on aarch64 platforms, providing a virtualized environment for x86_64 applications.
- Software Emulation: Some Linux distributions, like Fedora, provide software emulation layers, such as binfmt_misc, to run x86_64 code on aarch64 platforms.

### Instruction Translation Technology

The instruction translation technology used in Fedora Linux aarch64 is primarily based on the ARMv8-A architecture’s instruction set. The ARMv8-A architecture provides a set of instructions that can be used to translate x86_64 code into ARMv8-A code, allowing for efficient execution on aarch64 platforms.

The key instruction translation technology used in Fedora Linux aarch64 is:

- AArch64 Instruction Set: The ARMv8-A architecture provides a set of instructions that can be used to translate x86_64 code into ARMv8-A code. These instructions include:
- AT (Address Translate): This instruction runs a virtual address through a stage of address translation, returning a physical address in PAR_EL1.
- Translation Lookaside Buffer (TLB): This is a cache-like structure that stores recently accessed page table entries. It helps to speed up page table lookups and reduce the number of page faults.
- Page Table Management: The Linux kernel uses page table management mechanisms to manage memory allocation and deallocation on aarch64 platforms. This includes:
- Page Fault Handling: The kernel handles page faults by allocating new pages, invalidating existing pages, and updating the page tables.
- Page Table Walk: The kernel uses a page table walk to traverse the page tables and find the physical address corresponding to a virtual address.

In summary, while there are no direct equivalents for x86_64 instructions in Fedora Linux aarch64, the ARMv8-A architecture provides a set of instructions and mechanisms to translate x86_64 code into ARMv8-A code, allowing for efficient execution on aarch64 platforms.

## Differences

https://paracr4ckbeginnings.wordpress.com/2014/10/03/comparing-x86_64-and-aarch64-assembly-which-do-you-prefer/

### Writing and Debugging in Assembler

Writing assembly language programs, in comparison to writing programs in higher-level languages, requires a much different mind-set. You need to remember that aside from the simple fact that you are now thinking in terms of much simpler operations like load-operate on-store, in any given order, you must also take into account every little detail that you would otherwise take for granted. An example:

During the process of writing the x86_64 assembly programs for this post I encountered a problem when trying to output 2 digit numbers within a printing loop. I won’t be too specific as I discuss this in depth later on in this article, but what was about a day’s worth of time stuck on this problem turned out to be the simple fact that I was underestimating the importance of clearing the rdx register before an unsigned division(via div). Clearing rdx before the div instruction immediately solved this problem and I was done.

This is where gdb can have an unintended beneficial effect on your mindset! Where gdb seems like quite a primitive debugging tool, its repetitive, methodical, minute step by minute step approach can get you used to thinking in terms of the smallest impact that an operation can have. These small impacts can often have a snowball effect, resulting in larger problems later, and A LOT of extra debugging time.

At this point if you are beginning to wonder why it doesn’t take days to write or debug a “large” program at the assembly level, I would propose that it is due to the value of experience in spotting problem areas as you write your assembly code and ensuring that they are working correctly before moving on.

Also, if you aren’t a big fan of lots of GUI views and menus crowding your screen and you tend to rely more on your experience and reasoning to debug your applications then “simple” command-line tools like gdb can actually be more effective than MS Visual Studio’s debugging capabilities.

### x86_64

I began my assembly programming with the Intel IA_32 instruction set so my transition into x86_64 programming has been pretty smooth. Since the x86_64 version of the program that this blog is based upon is rather simple I didn’t have any noticeable problems. However, the Intel x86_64 instruction set is significantly more complex than the aarch64 instruction set. Just as an example, the aarch64 instruction set reference manual is approximately 150 pages long, the x86_64 instruction set reference is approximately 1500 pages long. So you could see how using advanced features or optimized instructions in place of more basic instructions could get very complicated very fast.

In terms of the assembly for both programs, neither contain noticeably more instructions than the other, and neither was more complicated to write.

One final note as to why I prefer the x86_64 instruction set. The main reason is that while I admit that the advanced instructions that really identify it as a CISC architecture can be difficult for a beginner, they do seem very interesting and I would bet that they are exploited by compilers such as gcc, g++, and intel’s c/c++ compiler at higher optimization settings and can produce noticeable performance gains as a result.

### aarch64

This application was the first aarch64 assembly language program I had ever written. Yet, one of the seemingly largest obstacles, learning the instruction set, was one of the least time consuming aspects of the whole process. I attribute this to the RISC architecture of aarch64. The initial problem when learning the x86_64 instruction set is that even when you do not take into account the more advanced, specialized instructions the simpler instructions still have so many addressing modes and other aspects to them that reading the manual can be a little uninspiring.

One final note in regards to my earlier statement about preferring the x86_64 instruction set to the aarch64 instruction set. For someone who wanted to start learning assembly and the lower level problem solving techniques that are involved I would recommend the aarch64 instruction set to the x86_64.

## Binary Translation

Expanding ISA usage with binary translation can reduce application migration costs from an existing ISA to a new one.  

In binary translation, there are two main categories, static and dynamic.
Static mode translates the executable binary into a new ISA binary without actually running it.
If less dynamic information is required during the translation, the static approach gives the best performance as translation time occurs offline.
Dynamic mode requires continuous runtime information to operate correctly.
Translation time forms part of the run time, resulting in reduced performance.
To date, the most successful example of ISA translation is the Rosetta software from Apple, which translates PowerPC to X86, and X86 to ARM.

By developing software tools and specialized hardware IP, our SoC will make application migration from ARM to RISC-V far less problematic and far more affordable.
