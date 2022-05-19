# Embedded Systems
Embedded Systems 2022 IUST Course

# Chapter I

We have two types of computes

- Desktop-produced millions per year (PC, Servers, and notebooks)
- Embedded-produced billions per year (like Military and medical equipment)

## What are embedded systems?

1. Information processing system embedded into a larger product. (It is not the main goal)
2. The main reason for buying these systems are not data processing
3. They are such devices other than PCs, servers, and notebooks that:
    1. Electricity running through
    2. Perform something intelligent
4. The hardware/software form a component of a larger system but are concealed from the user.

> Usually, the output of the sensors is analog outputs.
> 

## Characteristics of embedded systems

1. Dependability
2. Energy efficiency
3. Performance
4. Real-time: right answers arriving too late are still wrong.
5. Weight, Cost, Code-Size efficient
6. Dedicated user interface
7. Reactive: Frequently connected to the physical environment through sensors and actuators.
8. Hybrid (Analog + Digital parts)

The goals of designing a specific embedded system can be against each other.

Most real-time systems are embedded and most of the embedded systems are real-time.

![Untitled](Embedded%20Systems/Untitled.png)

# Big picture of an Embedded system

They Always check the sensors and the state of the environment.

They can use actuators to change the environment.

![Untitled](Embedded%20Systems/Untitled%201.png)

### Analog Components

- Sensors
- Actuators
- Non-Digital Controllers

### Digital Components

- Processors and Co-Processors
- Memories
- Digital-Controller, buses
- ASIC

### Software Components

- Application programs
- Exception handlers

## Embedded systems design flow

1. Concept
2. Specification: Software and Hardware components
3. Software and Hardware Partitioning: Consider different scenarios and create prototypes.
    1. What is the cost of this design?
    2. How fast is the system performance?
    3. What are the dimension of the systems?
    4. How they use energy?
4. Compile and load to the hardware

## Processors

- It is an artifacts that runs the algorithms.
- It contains controller and data-path.

### General-Purpose processors

![Untitled](Embedded%20Systems/Untitled%202.png)

- They can do variety of computational tasks.
- Functional Flexibility
- Low cost at high volumes
- Slow and power hungry: The elements are not optimized. They have more registers.

They have general data-path and ALU.

![Untitled](Embedded%20Systems/Untitled%203.png)

### Single-Purpose processors

- Just do on particular computational task
- They are fast and Power efficient
- Functional inflexibility
- High cost at low volume

### ASIPs processors

They are something between single and general purpose processors. ASIPs are designed for a particular class of applications having common characteristics.

They have optimized data-path.

They have special functional units.

### ASICs processors

They are designed to do exactly on program. (like Voice recorders or Bitcoin miners)

### ASICs VS General-Purpose

- Control logic is stored in memory and we need to fetch/decode instruction in the General-Purpose processors.
- the General-Purpose processors have more sets of logical units and registries.
- High NRE/sale-volume in General-Purpose processors.

## Memories

- It is an artifacts stores bits.
- It creates storage fabrics and the access logic

Write ability is about how fast the data is going to be written on that component.

- High end: like RAMs.
- Middle range: like FLASH, EEPROM
- Lower range: needs equipments to write data like EPROM, OTP ROM
- Low end: only stored during fabrication like Mask-programmed ROM

Storage permanence is about how long it holds the bits.

- High end: like Mask-programmed ROM
- Middle range: like NVRAM
- Lower range: the data is gone as soon as you turn off power supply like SRAM
- Low end: like DRAM

![Untitled](Embedded%20Systems/Untitled%204.png)

## Interfaces (Buses)

- It is an artifacts that transfers bits.
- It can be wire, air, fiber, or...
1.  Connectivity Scheme
    1. Serial Communication
2. Protocol
    1. Ports
    2. Timing diagrams
    3. Read and Write cycles

### Serial Communication

- A single wire for transferring data.
- One or more additional wire used for control
- High throughput for long distances
- Higher cost

like ISA, AMBA, PCI,...

### Parallel Communication

- Multiple wires for transferring data.
- One or more additional wire used for control
- Higher throughput for short distances
- Higher Cost

like ISA, AMBA, PCI,...

### Wireless Communication

InfraRed or IR

- Cheap to build
- Need line of sight and limited range
- Like TV-Remotes

Radio Frequency or RF

- More range of access
- Like modes or access points

## Peripherals

- They perform specific computational tasks.
- They use custom single-purpose processors
- Like Convertors, Timers, Counters, Watchdog Timers, UART, PWM, LCD, Keypad, Stepper motor controller

### UART

- Takes and receives parallel data and transmits serially
- Find errors with parity

### PWM

- Generates pulses with specific high/low times
- Control average voltage to electric device

like DC-DC Convertor, Dimmer lights

### LCD

- Having n row and m columns

# Chapter II

We are going to talk about microcontrollers. 

## Processor 

The processor executes serial of instructions. There are three different kinds of instructions:

- Data: Move, arithmetic, or logical operations
- Control: Branch, loop, or subroutine call
- Interface: Load or store

We always want to improve the performance of the processor. 

- Caches
- Branch prediction
- Multiple/Out of order

Embedded Processors

- Optimized and Low performance
- Integrated on a single chip
- Embedded processors are once programmed by manufacturer of system.
- Embedded processors interacts with their environments in many ways. (+Realtime)

## ARM Instruction

- Data processing
	- All operands are 32-bits wide.
	- The result is 32-bit wide.
- Data transfer
- Control flow

## Data Processing

### Arithmetic operations

```assembly
ADD r0, r1, r2 ;	r0 = r1 + r2
SUB r0, r1, #2 ;	r0 = r1 - 2
ADC r0, r1, r2 ;	r0 = r1 + r2 + C
SBC r0, r1, r2 ; 	r0 = r1 - r2 + C - 1
RSB r0, r1, r2 ;	r0 = r2 - r1 + C - 1
RSC r0, r1, r2 ; 	r0 = r2 - r1 + C - 1
```

### Logical operations

```assembly
AND r0, r1, r2 ;	r0 = r1 and r2
ORR r0, r1, r2 ;	r0 = r1 or r2
EOR r0, r1, r2 ;	r0 = r1 xor r2
BIC r0, r1, r2 ;	r0 = r1 and not r2
```

### Register Movement operations

```assembly
MOV r0, r2 ;	r0 = r2
MVN r0, r2 ;	r0 = not r2
```

### Comparsion operation

```assembly
; values for CC are N, V, C, Z
CMP r1, r2 ;
CMN r1, r2 ;
TST r1, r2 ;
TEQ r1, r2 ;
```

# Chapter III

## Arduino

What is Arduino?

- A development board
- A kind of C/C++ framework
- Open-Source hardware platform & development environment
- 

Characterstics:

- Low frequency range: Up to 20 MHz
- Many IO pins: It's good for creating a control program
- Cheep
- Widespread

### Sheilds

- TFT touch screen
- Data logger
- Ethernet
- Audio wave
- GSM
- Wifi
- Xbee: For creating a Wireless sensor network
- Adafruit Servo, Stepper, or DC Motor
- Battery
- Liquidware touch 
- Adafruit wave
- Adafruit GPS
- Adafruit wave

### Pins

![Screen Shot 2022-04-10 at 09.59.05.png](Embedded%20Systems/Screen%20Shot%202022-04-10%20at%2009.59.05.png)

- Digitals: `pinMode()`, `digitalWrite()`, `digitalRead()`.
- Interrupt: `attachInterrupt()`
- PWN: `analogWrite()`
- Analogs: `analogRead()`
- Power: 

USB - FIFI - 


what is PWM (?) You can create analog signals on digirtal ports 

> What is PCB?
> What are the sheilds?

## Automata Programming

The main problem of current programming languages is dealing with complex system behaviors. (Monitoring, Health care, etc.)
A solution is to provide automata for our system and generate the code using the available tools. 

### Components

- Control Object: Control the output amount of the water
- Control System: Check the height of the water

Control State: Like in Truing Machine.

Structure:
- Actions formed simultaneously
- Time and delays are set in the transitions.

### Language Specification for Embedded Systems

Completeness, Avoid the contradiction

- Hierarchy: Behavioral (Super-States), Structural (Like VHDL, Verilog)
- Timing behavior
- State-oriented behavior
- Event handling
	- External events (casued by the environment)
	- Internal Events (caused by the syste components)
- Support for a dependable system
- Exception-handling behavior

Available System: being awake watching for external events.
Safe System: Hardly fails and safely fails

## Model of computation - MOC

They are conceptual notions used to capture the system behavior.

- Sequential program model
- Object oriented - OO
- FSM
- DFG
- Petri net
- KPN
- CSP

## State-charts

Statecharts is a language for describing large, complex, and reactive systems.

![State-chart](Embedded%20Systems/statechart.png)

Statecharts VS FSM:

- Depth (abstraction)
- Orthogonality (concurrency)
- Broadcast communication (between states)

<aside>
💡 Outputs are global. (This message is a kind of communication between states)
</aside>

### Depth (!)

### Bottom-up clustering

![Refinement](Embedded%20Systems/refinement.png)

- D is a super state.
- The semantic of super state D: `A XOR B`

### Top-down refinement

The destination of a transition to a super state should be assigned to a basic state.

### Design modularity

#### Default State Mechanism

We can use the filled circle mechanism to handle this scenario. This super state is a state-less state.

#### History Mechanism

### Orthogonality (Concurrency)

![Orthogonality](Embedded%20Systems/orthogonality.png)

# Chapter IV

## Scheduling(?)

### Earliest Deadline First

EDF says the closest deadline should be executed first. Each time a task arrives, it is inserted into a queue of ready tasks, sorted by their deadlines.

## (?)

### Least Laxity/Slack First

The queue is sorted based on the `slack` parameter.

- Dynamically changing priority
- Preemptive: To allocate the CPU core to the ready task
- It is the optimal solution for uni-processor systems
	- Some OS don't support calculating dynamic priority.
	- Fixed-Priority preemptive scheduling is used commonly in real-time systems.
- Needs the knowledge of execution time.
- Need lots of calculations to specify the priority

> $$SLACK = Deadline - ExecutionTime$$

## RM Scheduling

- RM Algorithm is existed in different operating systems. (Such as windows NT)
- It is based on static priorities.

## EDF 

- It is optimal for periodic scheduling.
- It requires dynamix priority.
- EDF it the mos optimal scheduling that doesn't have idle time.

# Non-Preemptive

## LDF
- LDF can be optimal for uni-processors.

# Multi processor real-time scheduling

- One processor is a one-dimensional issue

Classification of algorithms:

- Partintioned 
- Global
- Semi-Partitioned: It is hybrid

## Partioned scheduling

- Each of two dimentions is dealt with separately
- No task migration
- 


# See more
  
- [Assignment I](Embedded%20Systems/Assignment%201.md)
- [Assignment II](Embedded%20Systems/Assignment%202.md)
- [Arduino](Embedded%20Systems/Arduino.md)

# References

- [How to Get Started Learning Embedded Systems](https://www.youtube.com/watch?v=aC37UE7edP0)
