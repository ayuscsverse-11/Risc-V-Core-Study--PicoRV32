# Risc-V-Core-PicoRV32
Architectural Study of the 32-bit PicoRV32 RISC-V core .
#  PicoRV32 Core & Architecture

This repository is a simple guide to track my learning about the **PicoRV32 microprocessor core**, how it processes instructions, and how its registers work. 

---

##  1. What is the PicoRV32 Core?

**PicoRV32** is simply the name of a tiny, open-source computer brain layout designed by an engineer named Claire Wolf. 

It follows the **RISC-V (RV32I)** standard rules:
* **RV:** RISC-V (Reduced Instruction Set Computer).
* **32:** It is a 32-bit processor (it works with 32-bit chunks of numbers).
* **I:** Stands for **Integer** (it handles basic math, simple logic, and moving numbers around).

**Why do we use it?** Because it is very small and uses very few components. It is perfect for small devices like drones, satellites, and smart gadgets.

---

## 2. Core Components (The 3 Main Parts)

Inside the PicoRV32 core, there are three main parts that work together to run your code:

```text
  [ Program Counter ] -------> [ Registers ] -------> [ ALU Calculator ]
   (Tracks line numbers)      (32 Small Storage Slots)   (Does the Math)
```

1. **Program Counter (PC):** Think of this as a bookmark. It simply holds the memory location of the next line of code that the processor needs to read.
2. **Registers (`x0` to `x31`):** These are 32 tiny, ultra-fast storage buckets inside the processor brain to hold your active numbers. 
3. **ALU (Arithmetic Logic Unit):** This is the core's built-in calculator. It takes numbers from your registers, does the math (like `+` or `-`), and gives you the answer.

---

## 3. The 5 Steps to Run Code (Processor Loop)

Every single line of code you write goes through an assembly line of 5 basic steps inside the chip:

```text
 [1. Fetch] ──> [2. Decode] ──> [3. Execute] ──> [4. Memory] ──> [5. Write-Back]
```

* **1. Fetch:** The processor looks at the Program Counter (PC) bookmark, finds the current code location in memory, and grabs that line of code.
* **2. Decode:** The processor reads the code to figure out what it means (Is it an Add command? A Subtract command?). It also looks inside the **Registers** to get the numbers needed for the math.
* **3. Execute:** The **ALU Calculator** does the actual math or logic. If the code says to add two numbers, this is where they get added together.
* **4. Memory:** If the code needs to read a new number from the computer's external memory, or save a number out to the memory, it does it in this step.
* **5. Write-Back:** The final answer from the calculator is saved back into one of the designated registers so the next lines of code can use it.

---

##  4. The Data Rule: Load-Store Architecture

 **Load-Store Architecture** rule. This is a very important rule to remember:

**The PicoRV32 core cannot do math directly inside the computer's main memory (RAM).** 

Why? Because the RAM is too far away and too slow for the fast ALU calculator. Therefore, to do math, the processor must follow a 3-step loop:
1. **Load Word (`lw`):** Move the numbers out of the faraway memory and drop them into the local registers.
2. **Do Math:** The ALU does the calculation instantly on the registers.
3. **Store Word (`sw`):** Take the final answer from the register and send it back out to be saved in the memory.

---

## 5. All 32 Registers :

There are exactly 32 registers (`x0` to `x31`). To remember them easily, just memorize their **4 main families**:

###  The Base Group (`x0` to `x2`)
* **`x0` (zero):** **Hardwired Zero.** This bucket is permanently stuck at `0`. You cannot change it. If you try to write a number into it, the hardware ignores it. It is used as a constant.
* **`x1` (ra):** **Return Address.** Remembers the line number to jump back to when a function or subroutine finishes.
* **`x2` (sp):** **Stack Pointer.** Tracks temporary local variables in memory.

###  The Temporary Group (`t`)
* **`x5` to `x7` (`t0`-`t2`)** and **`x28` to `x31` (`t3`-`t6`):** Quick scribble pads. The CPU uses these to hold throwaway numbers during fast calculations. They can be overwritten anytime.

### The Argument Group (`a`)
* **`x10` to `x17` (`a0`-`a7`):** The delivery trucks. When you want to send numbers into a function, you pack them into these registers. `a0` is also the bucket that brings the final calculated answer back to you.

### The Saved Group (`s`)
* **`x8`, `x9` (`s0`, `s1`)** and **`x18` to `x27` (`s2`-`s11`):** The safe vaults. If you have an important variable that *must not change*, you lock it inside an `s` register. The chip protects data inside these slots.

*(Note: `x3` and `x4` are system pointers used automatically by background software compilers, so you don't need to worry about managing them by hand!)*
