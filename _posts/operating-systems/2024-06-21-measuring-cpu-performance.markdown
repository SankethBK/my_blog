---
layout: post_with_categories
title:  "Measuring CPU Performance"
date:   2024-06-21 01:14:25 +0530
categories: cpu
author: Sanketh
---


CPU Manufacturers publish several metrics related to CPU like clock speed, number of cores, cache sizes, ISA, performance per Watt, number of transistors and more. Measuring CPU performance is complex, and it cannot be summarized by a single metric. In this post, I'll explore each of these metrics and discuss some standard benchmarking software and their limitations.

## What is Clock Speed, How does it Affects CPU Performance?

All Synchronous digital electronic circuits require an externally generated time reference. This is usually a square wave signal provided to the circuit called as clock. A **clock cycle** is the fundamental unit of time measurement for a CPU. A clock cycle is a single electrical pulse in a CPU, during which the CPU can execute a fundamental operation such as accessing memory, writing data, or fetching a new set of instructions. A clock cycle is measured as the amount of time between two pulses of an oscillator. The clock speed of a CPU is measured in Hertz (Hz), which signifies the number of clock cycles it can complete in one second. Common units are Megahertz (MHz) and Gigahertz (GHz).

### Major functionalities of CPU clocks

-  The clock signal ensures that all parts of the circuit change their state in a coordinated manner, ensuring reliable and predictable operation.
-  In sequential logic circuits, where the output depends not just on the current inputs but also on the history of inputs, a clock is necessary to sequence the operations correctly.
-  The clock signal allows precise control over the timing of data transfers between different parts of the circuit. The clock ensures that the data is stable and settled before being transferred to the next stage of the circuit.
  
### Does Higher Clock Speed Means Higher CPU Performance?

Let's say we have 2 CPU's with A and B with clock speed 3.4 GHz and 3.6 GHz respectively. While CPU B can generate more clock pulses compared to A, it does not necessarily mean that it has better performance than A. There are several other factors which come into play while determining the overall performance. If an instruction took "x" number of cycles in CPU A and if it takes same number of cycles in CPU B, then CPU B is indeed faster as it means that the instruction is taking less time to complete on CPU B. But in real life it's difficult to decrease the time of instruction execution. Some things like register access are very fast but things like memory access, ALU operations, floating point operations are complex in nature and cannot be optimized by memrely increasing clock speed. 

Let's say we have two CPUs, A and B, with clock speeds of 3.4 GHz and 3.6 GHz respectively. While CPU B can generate more clock pulses compared to CPU A, this doesn't necessarily mean it has better performance. There are several other factors that determine overall performance.

If an instruction takes the same number of cycles on both CPUs, CPU B will indeed execute it faster as it means that the instruction is taking less time to complete on CPU B. However, in real life, it's difficult to decrease the time of instruction execution purely by increasing clock speed. Simple tasks like register access are very fast and can be optimized, but complex operations such as memory access, ALU operations, and floating-point calculations are more complex in nature and cannot be optimized by memrely increasing clock speed. Thus, comparing the clock speeds of two CPUs is only relevant if the CPUs belong to the same family, like Intel i5 and i7, or Mac M1 and M4.

### Why CPU Clock speed has barely changed in past two decades?

The clock speed of consumer-grade CPUs has barely changed in the past two decades. For example, the Intel Pentium 4, which came out in the early 2000s, achieved a clock speed of 3.8 GHz, while the recent Apple M3 released in 2023 runs at 4.05 GHz. Despite the similar clock speeds, the Apple M3 is far more powerful than the Intel Pentium 4.

The primary reason manufacturers haven't increased clock speed is that higher frequency CPUs consume more power and generate more heat. This can lead to thermal throttling, where the CPU reduces its frequency to prevent overheating, negating the benefits of the higher frequency. As a result, manufacturers have shifted focus to other parameters to enhance performance:

1. **Cycles per Instruction (CPI):** Cycles per Instruction refers to the average number of cycles taken by a particular instruction to complete. Ideal value of CPI is 1, as each instruction needs at least one clock cycle to complete. But in real world scenario, even the simplest instruction will take at  cycles to complete, depending on the architecture. Techniques such as pipelining and out-of-order execution are used to optimize CPI and improve overall performance.

2. **Number of Cores and Threads:** A multi-core CPU can perform several tasks in parallel, with each core functioning as an independent processing unit. Technologies like simultaneous multithreading (SMT), or Intel's Hyper-Threading, allow a CPU core to support multiple hardware threads, each with its own Program Counter (PC) and registers allowing independent execution. Hyper-threading works by sharing functional units of a core among multiple hardware threads. Hardware threads are different from threads provided by OS which are known as software threads. In case of software threads, concurrency is achieved by context switching between multiple threads. In SMT, hardware threads may appear as independent cores to the operating system and applications, even though they share some physical resources. 

3. **Instruction Set Architecture (ISA):** ISA is a set of instructions that a processor can understand and execute. ISA are classified into two types: Reduced Instruction Set Computer (RISC) and Complex Instruction Set Computer (CISC). RISC architectures use a small, highly optimized set of instructions that are all of uniform length and are executed in a single clock cycle. This simplicity and efficiency make RISC architectures easier to pipeline and more power-efficient. Some examples RISC ISA's are MIPS, ARM. CISC architectures have a large and complex set of instructions, some of which can execute multi-step operations in a single instruction. This allows for more functionality per instruction but often results in more complex hardware and potentially slower performance for individual instructions, example: x86. 

4. **Dynamic Frequency Scaling:** Dynamic frequency scaling allows the CPU to adjust its clock speed based on the current workload and thermal conditions. By lowering the clock speed when full performance is not needed, the CPU can save power and reduce heat generation, which helps in maintaining performance efficiency and longevity. In some modern day processors like Intel I7, it's possible to increase the clock frequency beyond the base clock frequency. This will help in executing CPU intensive workloads efficiently provided there is enough thermal as well as power capacity. This is also known as overclocking. 

## The CPU Performance Equation

The CPU performance can be calculated using the equation 


$$
T = N_{\text{instr}} \cdot \text{CPI} \cdot t_{\text{cycle}}
$$


where:
- N<sub>instr</sub> = Number of instructions executed
- CPI = Cycles per Instruction
- t<sub>cycle</sub> = Duration of a clock cycle
- T = CPU time consumed by the program

Cycles per Instruction (CPI) is a critical metric in evaluating CPU performance. Different types of instructions take different numbers of cycles to complete, and even the same type of instruction can take varying numbers of cycles depending on factors like pipeline stalls or cache hits/misses. Because of this variability, CPI is often represented as an average value for each type of instruction. This average CPI helps in estimating the overall performance of a CPU when executing a mix of different instructions.

From the above equation we can see that CPU time consumed by a program can be decreased by decreasing one of the three paramaters N<sub>instr</sub>, CPI or t<sub>cycle</sub>. However, these three parameters are not completely independent of each other. For example, if we try to reduce N<sub>instr</sub> by optimizing the Assembly code to produce fewer instructions, we may have to increase the complexity of instructions which would in turn increase CPI for those instructions. 
