---
layout: post
title: "Workings Of An Assembly Debugger"
date: 2017-08-14 11:17:00 -0600
categories: blog
---

I learned how to use an assembly debugger when I was learning about Cracking. Software cracking is an interesting subject, and I wrote a few keygens at that time, but I probably won't be able to write an article about those, due to legal implications. In any case, the concept of an assembly debugger is pretty straightforward, and kind of cool.

### Assembly Debuggers

<a href="{{ site.url }}/assets/images/Chip-8/Assembly Debugger.png"><img class="centerimg" src="{{ site.url }}/assets/images/Chip-8/Assembly Debugger.png" ></a>
<p class="caption">OllyDbg, a popular assembly debugger</p>

Any ordinary executable you run on your computer is made of machine code. Machine code and assembly code are subtly different. Assembly is written out as instructions, whereas machine code is represented numerically. An assembly debugger operates like a debugger you might use while writing code, except it's designed to run on any executable you might have on your computer. It abstracts the machine code to assembly, and gives you a view of the processor registers, the memory table, the stack, and, of course, the code itself. Most importantly, like any debugger, it allows you to step through the code one line at a time<sup>1</sup>.

In theory, this could be useful for anyone who's written a program they want to debug, and they've maybe lost the source code or something. In practice they're surely used almost exclusively by hackers and crackers. Software developers have crafted some techniques to exit their program when it detects it's being run inside a debugger, and skilled crackers have to know how to deal with that.

Regardless of its use as an illegal tool, I found an assembly debugger to be an excellent tool for learning. I didn't go through a Computer Science program in college, so I never took a class on computer architectures. But, a couple weeks working with an assembly debugger taught me most of what I need to know.

### Instruction Processing

Every CPU has an in-built instruction set; a set of numbers that represent specific instructions. During execution, it is fed a series of numbers. If the cpu's given, for example, "18", it knows to add the next two numbers it receives together<sup>2</sup>. The result is stored in the processor's internal memory. A processor typically has space to hold about 32 numbers (these slots are called registers), though it depends on the instruction set. Some of these are specialized, and hold information the programmer might want to use, such as a flag indicating whether the last operation overflowed.

The CPU can also read from and write to 2 different memory sources: RAM and the stack<sup>3</sup>. Of these, I would say the stack gets used more during normal program execution. The stack operates as a First In, Last Out data structure, where if you write "1, 2, 3" to the stack, then access all its data, it will come out "3, 2, 1". The most important thing to understand about the stack is that it is accessed using only two primary commands: "push", which places a number onto the top of the stack, and "pop", which pulls the top number off of the stack<sup>4</sup>. It can be hard to understand, immediately, how to use that for anything important, but take as an example function calls.

When writing a program, and declaring a function, the function has a header. This defines the variables it needs to be fed in order to do its job. These variables are used inside of the function, and come out of scope at the end. When making a function call, you need to supply the function with its specified variables, according to its header, using hardcoded data or whatever variables are in scope.

How this gets translated into assembly is that when a function call is about to be made, the processor will typically dump its current registers to the stack, and then it will load each header variable into the stack, in left to right order. Then, it will make a jump to the function. The function unloads as many variables as it accepts off of the stack, into the CPU registers. Then, typically, it stores the register which declares where it jumped from into the stack. Once it reaches the end of the function, it will unload the "jumped from" value back to the cpu, and add one to it (so we will arrive at the instruction just after the jump), and load the return value into the stack, then jump back to wherever we came from. The CPU can load the return value, as well as its important registers, off of the stack.

Secondly, RAM is RAM. It's just a big memory bank where you store all of the bigger data. It's accessed using memory addresses, which are supplied with the calls to access the RAM. Typically, important information is loaded from the program on the hard disk to the reserved area in RAM, and then anything currently being operated on gets passed between the CPU and the stack.

There's a lot more that can be said about instruction processing, but we know enough now to get to [emulation]({{site.url}}/blog/2017/08/22/overview-chip-8.html).

* * *
1. Assuming the code is single-threaded. Many applications are multi-threaded today, and I'm not sure how the debuggers handle that, but I don't think they handle it well.

2. This is specific to how the processor is built, which is specific to the hardware manufacturer, but there are standards in place. Most CPUs are based on the x86 architecture. Most mobile phones use CPUs based on the ARM architecture.

3. The stack physically resides in RAM, but the instructions to access it are specific to the stack, and every thread has its own dedicated stack.

4. For data structures, and anything too large to be represented as a 32- or 64-bit number, the number being passed around is the memory address of where the data structure is stored in RAM.
