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

Most of real-time systems are embedded and most of embedded systems are real-time.

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
