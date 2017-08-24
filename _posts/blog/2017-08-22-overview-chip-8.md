---
layout: overview
title: "Chip-8 Interpreter - Overview"
date: 2017-08-22 10:03:00 -0600
categories: blog
name: java-chip8
description: Implementation of the CHIP-8 Interpreter specification
planguage: Java
otherapis: JApplet
codebase: https://github.com/cjpeterson/java-chip8
topimage: Chip-8/Emulator Screenshot.png
---

In November of 2013, I wrote a Chip-8 emulator over the course of a few days. I became interested in writing an emulator after messing around with an [assembly debugger]({{site.url}}/blog/2017/08/14/assembly-debugger.html). 

### Writing An Emulator

It is the beauty of computation that it doesn't depend on how it's implemented, just that it's implemented. Computation operates in between physical objects; it is the semantic layer on top of physical objects. This is tied to the importance of object-oriented programming. Objects can represent things, which is the only important part, for the purposes of computation.

When I was in the midst of watching a program run through an assembly debugger, it occurred to me that emulators probably work by having virtual CPUs, virtual stacks, virtual RAM, and virtual hard drives. Everything would be represented as an object, and everything would interact with the other virtual objects just as they would with real components in a physical system. I wanted to test this by writing an emulator. Specifically, I wanted to do it with as little information as possible, cause anyone can follow a tutorial, and I wanted to figure this out on my own.

I was initially planning on writing an NES emulator, as that seemed like a pretty simple system, and I grew up playing NES games, but some googling showed that people recommended a Chip-8 interpreter for one's first emulator. The NES is more complicated, and less satisfying, for reasons that I will get into.

Chip-8 is a protocol, rather than a specific system. Chip-8 defines an instruction set so that games could be written for this instruction set, and then someone could implement a Chip-8 interpreter for any specific computer, and you would be able to play Chip-8 games on that computer. It was originally an attempt to simplify the programming of certain gaming computers, but grew in popularity. It is pretty obsolete today, as it's capabilities are very limited, but it's a nice learning tool.

### Inside The Program

I found some technical references, and began working. I only knew Java at the time, so this was the language I went with. In addition, my only experience writing GUIs was by using the JApplet class from the JDK. This is probably considered a poor choice for a GUI, on account of there's not much control over it, and computational efficiency is not a priority, but it has worked for my experiments so far. At some point, I'd like to delve into better windowing systems.

<a href="{{ site.url }}/assets/images/Chip-8/Chip-8 Running.webm"><video class="centerimg" width="640" height="360" autoplay loop><source src="{{ site.url }}/assets/images/Chip-8/Chip-8 Running.webm" type="video/webm"></video></a>
<p class="caption">The emulator, running</p>

The program starts by initializing the virtual modules. An array for RAM, 4096 bytes; an array for the CPU registers, 16 bytes; a boolean array<sup>1</sup> to hold the pixel data for the screen, 64*32 bits; and an array for the stack, 16 shorts<sup>2</sup>. It also initializes any other necessary data, like the keystate tracker.

The program has you open a file through a dialog window. Then, after loading the program into virtual RAM, and initializing the sound system, it enters the main loop. The main loop simply handles any sound events, processes a single opcode, updates the screen if necessary, and then sleeps for 2 milliseconds<sup>3</sup>.

The opcode function is where the magic happens. It pulls the current instruction from RAM at the program counter and looks up what to do with that. The whole function is a series of "if", "elseif" statements. This is roughly equivalent to a "case" structure, and I like it better. It's still a pretty ugly layout, but there doesn't seem to be a better way to do it. The opcodes are supposed to be fast implementations of the behaviors outlined in the opcode specification. They were pretty fun to write, but there are ~35 of them, and a lot of them are similar, so it got tedious towards the end. I think it took me 3 hours, not including debugging time.

The program runs fairly well, at this point. I'm able to play most of the non-broken ROMs I found without any hiccups. There are two extant bugs I know of. One is with the sound. I process the sound by turning on a midi note, counting down the sound timer in the main loop, and turning it off when it hits zero. However, there's one cpu instruction that pauses execution until it receives a button press. Since I actually pause the thread inside the opcode function, I'm not able to count down the sound timer. Consequently, if there's a note playing when it hits this opcode, the sound will play until a button is pressed. The fix to this would be to run the sound handler in a separate thread.

<a href="{{ site.url }}/assets/images/Chip-8/Pac-Man Screenshot.png"><img class="centerimg" src="{{ site.url }}/assets/images/Chip-8/Pac-Man Screenshot.png" ></a>
<p class="caption">Pac-Man misbehaving</p>

The other bug must be in one of the opcodes. When loading pac-man, the maze generation gets messed up about halfway in. This is not a broken ROM, as I've tested it on another interpreter. It would take some time to track down the faulty opcode, since I have no easy means of testing them. I'm just gonna leave this as it is, because there are other things I'd rather work on these days.

### The Future

This project gave me a taste of working on emulators, which is a really cool concept: simulating hardware with software. I have a strong interest in creating and running simulations that I'd like to dive into sometime. As for emulation, specifically, there are a couple routes available to me right now.

The first is to write an NES emulator. I did in fact look into that at the end of this project. The main problem with emulating the NES is that it's a cartridge-based system, and those cartridges were allowed to have extra hardware inside of them. Meaning, you need to have special code for each type of cartridge. This is annoying to me, because the code is not generic. The fun of writing an emulator is that you can code one program, and then play thousands of games. This would be more akin to porting the games to the PC. It's not quite that laborious, but that concept is what stopped me from wanting to write an NES emulator.

The second is to work on an emulator that hasn't been done before. I've had an especial interest in RPCS3, the open-source PlayStation 3 emulator. The PS3 was an excellent gaming system (one that I actually played a fair bit), and yet it doesn't have a running emulator. I dove into the source code for RPCS3 a couple of times, to try and see where they were at, and what I could do with it. The first major issue is acquiring the games themselves. They're encrypted, and they get decoded through a complicated process when the system loads the games. This has been figured out by a bunch of very dedicated people, with information tucked away in random corners of the web, but the gist of it is that currently, you need a file that gets loaded onto the PS3 when you insert the game, and the only way of acquiring that file is to transfer it from a modded PS3. So, it's currently tough for me to even test a game.

The other major issue is that supposedly the development team sort of disbanded after they decided that the core of the emulator had been coded poorly, relative to how the PS3 actually works, and would probably have to be re-written before development could continue. No one had actually done that yet, and they were not specific on _how_ it had to be re-written. This seemed to leave me out of luck.

At this point, however, the two lead developers have set up a patreon, and patreon has turned out to be a very successful model (for now), so they are getting paid full-time wages, and they are making rapid progress on the emulator. I may try to fix some bugs at some point, but I don't think the emulator needs my help very much right now.

So, that's where my work with emulators sits, for the time being.

* * *
1. The pixels can only be black or white, so I used a binary data structure.

2. The specification does actually specify these are 16-bit elements. The stack is only used to reference memory addresses, which requires at least 12 bits (a byte will not suffice).

3. The clock rate is ~500Hz. If I wanted to be fancy, I could calculate how long my loop took, and subtract that from the sleep time, but I expect it's negligible on a modern machine that's not under heavy load, so I didn't bother programming that in.
