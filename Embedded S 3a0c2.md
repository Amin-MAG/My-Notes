# Embedded Systems

We have two types of computes

- Desktop-produced millions per year (PC, Servers, and notebooks)
- Embedded-produced billions per year (like Military and medical equipment)

## What are embedded systems?

1. Information processing system embedded into a larger product. 
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

![Untitled](Embedded%20S%203a0c2/Untitled.png)

# Big picture of an Embedded system

They Always check the sensors and the state of the environment.

They can use actuators to change the environment.

![Untitled](Embedded%20S%203a0c2/Untitled%201.png)

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

![Untitled](Embedded%20S%203a0c2/Untitled%202.png)

- They can do variety of computational tasks.
- Functional Flexibility
- Low cost at high volumes
- Slow and power hungry: The elements are not optimized. They have more registers.

They have general data-path and ALU.

![Untitled](Embedded%20S%203a0c2/Untitled%203.png)

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

![Untitled](Embedded%20S%203a0c2/Untitled%204.png)

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

# References

[How to Get Started Learning Embedded Systems](https://www.youtube.com/watch?v=aC37UE7edP0)

[Assignment I](Embedded%20S%203a0c2/Assignment%20cf8b1.md)