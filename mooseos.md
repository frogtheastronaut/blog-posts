# Making a Custom OS

### Intro
Recently, I've just finished a custom operating system called [MooseOS](https://github.com/frogtheastronaut/moose-os), and I have recently gotten it running on a real PC.  
I started this project with almost zero experience with C and Assembly, so it really was a learning journey for me.


### What Inspired Me
At the start of the year, I watched a video about the story of Terry A. Davis and his project, TempleOS. I was stunned at his dedication and hard work, not to mention impressed by his end product. Thus, I was inspired to create my own operating system.


### Learning C
C is a programming language known to have a high learning curve. However, I was able to understand its code fairly easily since I had experience with other programming languages such as Python.  

One of the things I learned was about header files — how they contain macros, type definitions, and function prototypes. Moreover, I learned about low-level data structures.  

While Python only has a single `int` type (excluding floats and complex numbers), C has multiple integer types, such as `uint32_t` and `uint16_t`. These can also have specifiers that change their properties — for example, signedness (`unsigned`) can change the range of values a variable can store. Storage specifiers like `static` can also improve efficiency and data encapsulation.


### Small List of Features

![OS Arch Diagram](https://raw.githubusercontent.com/frogtheastronaut/pw-images/refs/heads/main/MOOSE-OS/os-architecture.png)  
*Caption: OS architecture diagram for MooseOS*

Here is a list of features my OS has:

#### **320x200 VGA Graphics Mode (256-colour)**  
  I chose 320x200 because of its simplicity and ease of implementation. I used a retro-inspired colour theme to mimic early operating systems like Macintosh System 1 — though I designed my own palette to make MooseOS unique.

#### **Desktop Interface**  
  ![Dock](https://raw.githubusercontent.com/frogtheastronaut/pw-images/refs/heads/main/MOOSE-OS/moose-dock.png)  
  *Caption: MooseOS’s dock-based application launcher*  
  MooseOS uses a dock-based application launcher for ease of use.

#### **Keyboard and Mouse Support**  
  This operating system supports PS/2 mouse and keyboard input. Implementing this taught me a lot about how computer interrupts work.

#### **Interrupt Descriptor Table (IDT)**  
  MooseOS’s IDT supports all 32 exceptions, as well as keyboard, mouse, and RTC interrupts. All exceptions are handled with proper panic systems (think BSOD), which is extremely helpful for debugging.

#### **Applications**  
  MooseOS includes all the basic applications that provide essential functionality — a file explorer, a text editor, and a terminal emulator that supports basic shell commands.

#### **Audio System**  
  MooseOS can use the PC speaker to play rudimentary sounds.

#### **ATA Disk I/O**  
  This taught me a lot about ATA and how computers physically store data.

#### **Time-Keeping**  
  Like most other operating systems, MooseOS reads from the CMOS port at startup, then uses the PIT to keep track of time. This avoids unnecessary CMOS reads that could slow the system down.

#### **ELF File Loading**  
  MooseOS can load and validate ELF files uploaded through its upload script.

#### **Custom Filesystem**  
  ![File Explorer](https://raw.githubusercontent.com/frogtheastronaut/pw-images/refs/heads/main/MOOSE-OS/moose-explorer.png)  
  *Caption: MooseOS’s file explorer*  
  I didn’t want to use common filesystems like EXT2 because they were overly complex and felt like reinventing the wheel. So, I decided to make my own. Building a custom filesystem taught me a lot about how both modern and retro computers store file information.

#### **Memory Management**  
  MooseOS includes a custom heap allocator and 4 KiB virtual memory management with paging. This not only taught me how computers access memory, but also about physical and virtual addressing.

#### **Multitasking**  
  MooseOS supports cooperative multitasking. I chose this for its simplicity — and it gets the job done.

#### **Miscellaneous**  
  I implemented a CI/CD workflow that automatically builds the project with GitHub Actions after each `git push`. 


### Working on a Real PC

![Real PC running MooseOS](https://raw.githubusercontent.com/frogtheastronaut/pw-images/refs/heads/main/MOOSE-OS/moose-pc.png)  
*Caption: MooseOS running on real hardware*

When a friend gave me an old 2009 HP PC they were going to throw away, I finally had the chance to test MooseOS on real hardware. It worked so well it surprised me — the only issue was a General Protection Fault that QEMU never threw. After some debugging, I had the OS running smoothly within half an hour.


### Challenges
Developing MooseOS came with many challenges — from learning low-level programming in C and Assembly to implementing VGA graphics, multitasking, and paging.  

Overcoming these required patience, research, and a fair bit of trial and error. These experiences didn’t just teach me technical skills, but also critical problem-solving techniques.


### Conclusion
Making this operating system was a fantastic journey. While OSDev is somewhat obsolete, I still highly recommend it — it’s one of the most fun and rewarding programming challenges out there.  

It taught me low-level system design, debugging, and problem-solving — making it one of the most fulfilling projects I’ve ever worked on.

You can view my OS here: [https://github.com/frogtheastronaut/moose-os](https://github.com/frogtheastronaut/moose-os)

Thanks for reading!
