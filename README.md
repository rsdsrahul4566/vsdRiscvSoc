# 🛠️ Task 1: RISC-V Toolchain Setup and Verification Using WSL 



## 🎯 Objective

Successfully install the RISC-V toolchain in WSL (Windows Subsystem for Linux), configure the environment variables, and verify that the essential binaries (`gcc`, `objdump`, and `gdb`) function correctly for cross-compilation development.

## 📋 Prerequisites

- ✅ WSL (Windows Subsystem for Linux) installed and configured
- ✅ Downloaded `riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz` in Windows Downloads directory
- ✅ Basic knowledge of Linux command line operations

## 🚀 Step-by-Step Implementation

### Step 1: Navigate to Downloads Directory and Create Installation Path

Access the Windows Downloads directory from WSL and prepare the installation directory.
 Navigate to Windows Downloads directory from WSL
```bash
cd /mnt/c/Users/rsdsr/Downloads
```

Create installation directory with proper permissions
```bash
sudo mkdir -p /opt/riscv
```

Verify the downloaded toolchain archive exists
```bash
ls -la riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz
```


### Step 2: Extract the RISC-V Toolchain

Extract the downloaded toolchain archive to the designated installation directory.

Extract the toolchain archive to /opt/riscv
```bash
sudo tar -xzf riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz -C /opt/riscv --strip-components=1
```
Verify extraction was successful
```bash
ls -la /opt/riscv/
```
Check the nested directory structure (important for PATH configuration)
```bash
ls -la /opt/riscv/riscv/
```

### Step 3: Configure PATH Environment Variable

Add the RISC-V toolchain binaries to the system PATH for persistent access.

Add toolchain binaries to PATH (note the nested riscv/riscv/bin structure)
```bash
echo 'export PATH=/opt/riscv/riscv/bin:$PATH' >> ~/.bashrc
```
Reload the shell configuration to apply changes
```bash
source ~/.bashrc
```
Verify PATH configuration
```bash
echo $PATH | grep riscv
```


### Step 4: Verify Toolchain Installation

Test all essential RISC-V development tools to ensure proper installation and functionality.

Check GCC compiler version
```bash
riscv32-unknown-elf-gcc --version
```
Check objdump utility version
```bash
riscv32-unknown-elf-objdump --version
```

Check GDB debugger version
```bash
riscv32-unknown-elf-gdb --version
```

Verify target architecture support
```bash
riscv32-unknown-elf-gcc -dumpmachine
```

List all available RISC-V binaries
```bash
ls -la /opt/riscv/riscv/bin/ | grep riscv32
```


## 📊 Expected Results

Upon successful completion, you should see output similar to:

- **GCC Version**: `riscv32-unknown-elf-gcc (g04696df096) 14.2.0`
- **Target Architecture**: `riscv32-unknown-elf`
- **Total Binaries**: 64 RISC-V development tools available
- **ISA Support**: `rv32i2p1_m2p0_a2p1_c2p0` (RV32IMAC)

## 📸 Implementation Output

![Task 1 Toolchain Verification](screenshots/task1_toolchain_verification.png)

*Screenshot demonstrating successful RISC-V toolchain installation with version verification of gcc, objdump, and gdb binaries in WSL environment.*

## ⚠️ Troubleshooting Guide

### Common Issues and Solutions:

| Issue | Cause | Solution |
|-------|--------|----------|
| `command not found` | Incorrect PATH configuration | Verify PATH points to `/opt/riscv/riscv/bin` |
| Permission denied | Insufficient permissions | Use `sudo` for operations in `/opt/riscv` |
| Multiple PATH entries | Repeated exports in `.bashrc` | Clean up `.bashrc` manually |
| Extraction errors | Corrupted download | Re-download the toolchain archive |

# 🏗️ Task 2: Cross-Compile "Hello, RISC-V"

[![RISC-V](https://img.shields.io/badge/Architecture-RISC--V-blue.svg)](https://riscv.org/)
[![C Programming](https://img.shields.io/badge/Language-C-00599C.svg)](https://en.wikipedia.org/wiki/C_(programming_language))
[![Cross Compilation](https://img.shields.io/badge/Build-Cross--Compilation-green.svg)]()
[![Status](https://img.shields.io/badge/Status-✅%20Complete-success.svg)]()

## 🎯 Objective

Create a minimal C "Hello World" program and successfully cross-compile it for the RISC-V RV32 architecture, producing a valid 32-bit RISC-V ELF executable that demonstrates proper toolchain functionality.[1][4]

## 📋 Prerequisites

- ✅ RISC-V toolchain installed and configured (Task 1 completed)
- ✅ PATH environment variable set to `/opt/riscv/riscv/bin`
- ✅ Verified `riscv32-unknown-elf-gcc` functionality
- ✅ Basic understanding of C programming and cross-compilation concepts

## 🚀 Step-by-Step Implementation

### Step 1: Create the Hello World C Program

Create a minimal C program that demonstrates basic functionality and printf usage.
```bash
nano hello.c
```

**C Program Code:**
```bash
#include <stdio.h>

int main() {
printf("Hello, RISC-V!\n");
return 0;
}
```

### Step 2: Cross-Compile for RISC-V Architecture
Compile the C program using the RISC-V toolchain with the default configuration that works with your specific toolchain setup.

 Cross-compile using default toolchain configuration (THIS WORKS!)
```bash
riscv32-unknown-elf-gcc -o hello.elf hello.c
```
✅ Working Solution: Use default compilation without explicit -march or -mabi flags to avoid multilib errors.

### Step 3: Verify the Compiled ELF Binary
Check that the compiled binary is a valid 32-bit RISC-V executable.
```bash
# Check the ELF file properties and architecture
file hello.elf
```
✅ Expected Working Output:

```bash
hello.elf: ELF 32-bit LSB executable, UCB RISC-V, RVC, soft-float ABI, version 1 (SYSV), statically linked, not stripped
```
### Step 4: Additional Working Verification Commands

 Check file exists and basic properties
```bash
ls -la hello.elf
```

 Verify target architecture
```bash
riscv32-unknown-elf-gcc -dumpmachine
```

Basic objdump header check
```bash riscv32-unknown-elf-objdump -h hello.elf
```
📊 Confirmed Working Results
Your Successful Output:
✅ 32-bit RISC-V: Correct target architecture confirmed

✅ RVC: Compressed instructions enabled (equivalent to rv32imc)

✅ Statically linked: Self-contained executable

✅ UCB RISC-V: Standard RISC-V specification

✅ GCC Version: 14.2.0 (g04696df096) working properly

Technical Specifications That Work:
Target: riscv32-unknown-elf

Toolchain Path: /opt/riscv/riscv/bin/

Default ISA: Automatically includes RV32IMAC support

Compilation Method: Default configuration (no explicit flags needed)

🚫 What NOT to Use (Causes Errors):
```bash
# ❌ AVOID - Causes multilib errors in your setup:
riscv32-unknown-elf-gcc -march=rv32imc -mabi=ilp32 -o hello.elf hello.c

# ❌ AVOID - Explicit ISA flags incompatible with your toolchain:
riscv32-unknown-elf-gcc -march=rv32i -mabi=ilp32 -o hello.elf hello.c
```
✅ Success Criteria Met:
 Compilation completes without errors

 Generated hello.elf is valid 32-bit RISC-V executable

 file command shows "UCB RISC-V, RVC"

 Binary ready for Task 3 assembly analysis

# ⚙️ Task 3: From C to Assembly - Generate and Analyze Assembly Code

[![RISC-V](https://img.shields.io/badge/Architecture-RISC--V-blue.svg)](https://riscv.org/)
[![Assembly](https://img.shields.io/badge/Language-Assembly-orange.svg)](https://en.wikipedia.org/wiki/Assembly_language)
[![Compiler](https://img.shields.io/badge/Compiler-GCC%2014.2.0-green.svg)](https://gcc.gnu.org/)
[![Status](https://img.shields.io/badge/Status-✅%20Complete-success.svg)]()

## 🎯 Objective

Generate assembly code from the C "Hello, RISC-V" program and perform detailed analysis of the function prologue and epilogue to understand RISC-V calling conventions, stack frame management, and instruction encoding patterns.[2][3]

## 📋 Prerequisites

- ✅ Task 2 completed: Cross-compiled `hello.elf` binary successfully created
- ✅ RISC-V toolchain accessible at `/opt/riscv/riscv/bin/`
- ✅ `hello.c` source file present in working directory
- ✅ Understanding of basic assembly language concepts

## 🚀 Step-by-Step Implementation

### Step 1: Generate Assembly Code from C Source

Create assembly output with no optimization to ensure clear, readable assembly structure.
Generate assembly file with no optimization for clarity
```bash
riscv32-unknown-elf-gcc -S -O0 hello.c
```

**Command Breakdown:**
- `-S`: Stop after compilation stage, generate assembly code only
- `-O0`: Disable all optimizations for readable output
- `hello.c`: Input C source file
- **Output**: Creates `hello.s` assembly file

### Step 2: View and Examine the Assembly Code

Display the generated assembly file to analyze the complete function structure.

View the complete assembly file
```bash
cat hello.s
```

View assembly with line numbers for detailed analysis
```bash
nl hello.s
```

### Step 3: Verify Assembly File Creation

Confirm the assembly file was generated correctly and contains expected content.
Check if assembly file exists and view its properties
```bash
ls -la hello.s
```

Count lines in the assembly file
```bash
wc -l hello.s
```

Check file type
```bash
file hello.s
```

## 📋 Working Assembly Structure Analysis

Based on  successful WSL implementation, here's the actual assembly structure generated:

### 🔧 **Function Prologue Analysis (Entry Setup):**
```bash
main:
addi sp,sp,-16 # Allocate 16 bytes on stack (RISC-V alignment)
sw ra,12(sp) # Save return address at offset 12
sw s0,8(sp) # Save frame pointer at offset 8
addi s0,sp,16 # Set up new frame pointer
```

**Prologue Explanation:**
- **Stack Allocation**: `addi sp,sp,-16` creates 16-byte aligned stack frame
- **Return Address**: `sw ra,12(sp)` preserves where to return after function
- **Frame Pointer**: `sw s0,8(sp)` saves old frame pointer for restoration
- **Frame Setup**: `addi s0,sp,16` establishes new frame pointer for local variables

### 🎯 **Function Body Analysis:**

lui     a5,%hi(.LC0)   # Load upper immediate of string address
addi    a0,a5,%lo(.LC0) # Complete address in a0 (first argument register)
call    puts           # Call puts function (GCC optimized printf→puts)
li      a5,0           # Load immediate 0 for return value
mv      a0,a5          # Move return value to a0 register


**Body Explanation:**
- **Address Loading**: Two-instruction sequence loads 32-bit string address
- **Argument Setup**: `a0` register contains first function argument (string pointer)
- **Function Call**: `call puts` - GCC optimized `printf` to `puts` for efficiency
- **Return Value**: Sets function return value to 0 in `a0` register

### 🔄 **Function Epilogue Analysis (Exit Cleanup):**

lw      ra,12(sp)      # Restore return address from stack
lw      s0,8(sp)       # Restore original frame pointer
addi    sp,sp,16       # Deallocate stack space (restore stack pointer)
jr      ra             # Jump to return address (return to caller)


**Epilogue Explanation:**
- **Register Restoration**: `lw` instructions restore saved registers from exact stack locations
- **Stack Cleanup**: `addi sp,sp,16` returns stack pointer to pre-call state
- **Function Return**: `jr ra` transfers control back to calling function

## 📊 Expected Assembly Output

Your `hello.s` file should contain these key sections:

### **File Header Information:**

    .file   "hello.c"
    .option nopic
    .attribute arch, "rv32i2p1_m2p0_a2p1_c2p0"
    .attribute unaligned_access, 0
    .attribute stack_align, 16

### **String Constant Section:**
    .section        .rodata
    .align  2
    
.LC0:
.string "Hello, RISC-V!"

### **Complete Main Function:**
    .text
    .align  1
    .globl  main
    .type   main, @function
main:
# [Prologue, Body, and Epilogue as analyzed above]
.size main, .-main


## 📸 Implementation Output

![Task 3 Assembly Generation](screenshots/task3_assembly_generation.png)

*Screenshot demonstrating successful assembly code generation with complete prologue/epilogue analysis and line-numbered output for detailed examination.*

## ⚠️ Troubleshooting Guide

### Common Issues and Solutions:

| Issue | Symptom | Root Cause | Solution |
|-------|---------|------------|----------|
| **No Assembly File** | `hello.s` not created | Missing source file or compilation error | Verify `hello.c` exists: `ls -la hello.c` |
| **Empty Assembly** | `hello.s` exists but empty | Compilation failed silently | Re-run: `riscv32-unknown-elf-gcc -S -O0 hello.c` |
| **Confusing Output** | Complex assembly structure | Optimization enabled | Ensure `-O0` flag is used for clarity |
| **Permission Error** | Cannot write assembly file | Directory permissions | Check write permissions: `ls -la .` |

### Debugging Commands:
Verify source file exists
```bash
ls -la hello.c
```
Check if toolchain is accessible
```bash
which riscv32-unknown-elf-gcc
```
Verify PATH configuration
```bash
echo $PATH | grep riscv
```
Re-generate assembly with verbose output
```bash
riscv32-unknown-elf-gcc -S -O0 -v hello.c
```
Check assembly file was created
```bash
ls -la hello.s && echo "Assembly file created successfully"
```

## 🎉 Success Criteria

Task 3 is considered **complete** when:
- [x] Assembly file `hello.s` generated without errors
- [x] Function prologue shows proper stack allocation and register saving
- [x] Function body demonstrates correct string loading and function call
- [x] Function epilogue properly restores registers and deallocates stack
- [x] GCC optimization (printf→puts) is observed and understood
- [x] 16-byte stack alignment is confirmed
- [x] Ready for binary analysis in Task 4

## 💡 Key Learning Outcomes

### **Assembly Programming Concepts:**
- ✅ **Function Prologue/Epilogue**: Understanding of standard function entry/exit patterns
- ✅ **Stack Management**: RISC-V stack frame setup and teardown procedures
- ✅ **Register Preservation**: Calling convention compliance for register saving
- ✅ **Address Calculation**: Two-instruction sequence for 32-bit address loading

### **RISC-V Specific Knowledge:**
- ✅ **Instruction Encoding**: Understanding of RISC-V assembly syntax
- ✅ **Memory Model**: Stack-based parameter passing and local storage
- ✅ **Compiler Behavior**: GCC optimizations and code generation patterns
- ✅ **ABI Compliance**: Application Binary Interface adherence

## 🔗 Next Steps

With assembly analysis complete, proceed to:
- **Task 4**: Hex dump and disassembly analysis for machine code examination
- **Task 5**: RISC-V ABI and register convention reference guide

---

# 🔍 Task 4: Hex Dump & Disassembly Analysis

[![RISC-V](https://img.shields.io/badge/Architecture-RISC--V-blue.svg)](https://riscv.org/)
[![Binary Analysis](https://img.shields.io/badge/Analysis-Binary%20Disassembly-purple.svg)]()
[![Intel HEX](https://img.shields.io/badge/Format-Intel%20HEX-orange.svg)]()
[![Status](https://img.shields.io/badge/Status-✅%20Complete-success.svg)]()

## 🎯 Objective

Convert the compiled RISC-V ELF binary to Intel HEX format for hardware deployment and perform detailed disassembly analysis to understand machine code structure, instruction encoding, and memory layout of the cross-compiled program.[1]

## 📋 Prerequisites

- ✅ Task 3 completed: Assembly analysis understanding achieved
- ✅ `hello.elf` binary from Task 2 available in working directory
- ✅ RISC-V toolchain accessible at `/opt/riscv/riscv/bin/`
- ✅ Understanding of hexadecimal notation and binary formats

## 🚀 Step-by-Step Implementation

### Step 1: Generate Complete Disassembly

Create a detailed disassembly of the ELF binary to examine machine code and instruction encoding.
Generate comprehensive disassembly and save to file
```bash
riscv32-unknown-elf-objdump -d hello.elf > hello.dump
```
Verify disassembly file was created
```bash
ls -la hello.dump
```
View the complete disassembly output
```bash
cat hello.dump
```

### Step 2: Convert ELF to Intel HEX Format

Generate Intel HEX format suitable for programming embedded systems and hardware simulators.
Convert ELF binary to Intel HEX format
```bash
riscv32-unknown-elf-objcopy -O ihex hello.elf hello.hex
```
Verify HEX file creation
```bash
ls -la hello.hex
```
View the Intel HEX output
```bash
cat hello.hex
```

### Step 3: Analyze Main Function Disassembly

Focus on the main function to understand the column structure and instruction encoding.

Extract main function disassembly for detailed analysis
```bash
grep -A 20 "<main>:" hello.dump
```

View disassembly with context around main function
```bash
grep -B 5 -A 20 "<main>:" hello.dump
```

## 📊 Working Disassembly Output Analysis

Based on your successful WSL implementation, here's the actual disassembly structure:

### 🔧 **Disassembly Column Structure:**

```bash
00010162 <main>:
10162: 1141 addi sp,sp,-16
10164: c606 sw ra,12(sp)
10166: c422 sw s0,8(sp)
10168: 0800 addi s0,sp,16
```

### 📋 **Column Explanation:**

| Column Position | Example | Description | Purpose |
|----------------|---------|-------------|---------|
| **1️⃣ Address** | `10162` | Memory location (hexadecimal) | Shows where instruction is loaded in memory |
| **2️⃣ Opcode** | `1141` | Machine code bytes (hex) | Actual binary instruction encoding |
| **3️⃣ Mnemonic** | `addi` | Assembly instruction name | Human-readable instruction type |
| **4️⃣ Operands** | `sp,sp,-16` | Registers and immediate values | Instruction parameters and targets |

### 🎯 **Compressed Instruction Analysis:**

**Key Observations from Your Output:**
- **2-byte Opcodes**: `1141`, `c606`, `c422` demonstrate RV32C (compressed) instructions
- **Memory Addresses**: Sequential 2-byte increments (10162, 10164, 10166, 10168)
- **Instruction Density**: Compressed instructions provide ~30% code size reduction
- **Mixed Encoding**: Some instructions may use 4-byte encoding for complex operations

### 🔄 **Function Structure in Machine Code:**

Prologue (Stack Setup):
10162: 1141 addi sp,sp,-16 # Compressed stack allocation
10164: c606 sw ra,12(sp) # Compressed return address save
10166: c422 sw s0,8(sp) # Compressed frame pointer save
10168: 0800 addi s0,sp,16 # Frame pointer setup

Body (String Loading and Function Call):
1016a: 67c9 lui a5,0x12 # Load upper immediate
1016c: 45c78513 addi a0,a5,1116 # Complete address calculation
10170: 26bd jal 104de <puts> # Compressed jump and link

Epilogue (Cleanup and Return):
10172: 4781 li a5,0 # Load immediate zero
10174: 853e mv a0,a5 # Move return value
10176: 40b2 lw ra,12(sp) # Restore return address
10178: 4422 lw s0,8(sp) # Restore frame pointer
1017a: 0141 addi sp,sp,16 # Restore stack pointer
1017c: 8082 ret # Compressed return


## 📄 Intel HEX Format Analysis

Based on successful conversion, the Intel HEX output structure:

### **Sample HEX Output:**
```bash
:10390000F8380100F83801000039010000390100E1
:10391000083901000839010010390100103901008F
:10392000183901001839010020390100203901003F
...
:00000001FF
```

### **HEX Format Structure:**
- **`:10`**: Record marker and byte count (16 bytes)
- **`390000`**: Load address for this data block
- **`F8380100...`**: Actual program data (machine code)
- **`E1`**: Checksum for data integrity
- **`:00000001FF`**: End-of-file record

## 📸 Implementation Output

![Task 4 Hex Dump and Disassembly](screenshots/task4_hex_disassembly.png)

*Screenshot demonstrating successful binary disassembly with detailed column analysis and Intel HEX conversion showing machine code structure and memory layout.*

## ⚠️ Troubleshooting Guide

### Common Issues and Solutions:

| Issue | Symptom | Root Cause | Solution |
|-------|---------|------------|----------|
| **Empty Disassembly** | `hello.dump` empty or no output | ELF file missing or corrupted | Verify: `ls -la hello.elf` and re-compile if needed |
| **No HEX Output** | `hello.hex` not created | objcopy failed | Check: `which riscv32-unknown-elf-objcopy` |
| **Permission Denied** | Cannot write output files | Directory permissions | Verify write access: `ls -la .` |
| **Malformed HEX** | Invalid HEX format | Conversion error | Re-run objcopy with verbose: `-v` flag |

### Debugging Commands:
Verify input file exists and is valid ELF
```bash
file hello.elf
```
Check if objdump is accessible
```bash
which riscv32-unknown-elf-objdump
```
Test basic disassembly without file output
```bash
riscv32-unknown-elf-objdump -d hello.elf | head -20
```
Verify objcopy functionality
```bash
riscv32-unknown-elf-objcopy --version
```
Re-generate files with verification
```bash
riscv32-unknown-elf-objdump -d hello.elf > hello.dump && echo "Disassembly created"
riscv32-unknown-elf-objcopy -O ihex hello.elf hello.hex && echo "HEX file created"
```

### **Intel HEX Applications:**
- **Embedded Programming**: Direct flash memory programming
- **Simulator Input**: RISC-V simulator and emulator loading
- **Hardware Testing**: FPGA and ASIC verification
- **Bootloader**: System initialization and program loading

## 🎉 Success Criteria

Task 4 is considered **complete** when:
- [x] Disassembly file `hello.dump` contains readable machine code analysis
- [x] Intel HEX file `hello.hex` generated with proper format and checksums
- [x] Column structure (address, opcode, mnemonic, operands) is understood
- [x] Compressed instruction encoding (RV32C) is identified and analyzed
- [x] Main function machine code mapping to assembly is verified
- [x] Binary is ready for hardware deployment or simulation
- [x] Ready for ABI and register analysis in Task 5

## 💡 Key Learning Outcomes

### **Binary Analysis Skills:**
- ✅ **Disassembly Interpretation**: Reading and understanding machine code output
- ✅ **Instruction Encoding**: Recognition of compressed vs. standard instruction formats
- ✅ **Memory Layout**: Understanding address organization and code placement
- ✅ **Format Conversion**: ELF to Intel HEX transformation for deployment

### **RISC-V Architecture Insights:**
- ✅ **Compressed Extensions**: Benefits and encoding of RV32C instruction set
- ✅ **Address Calculation**: Multi-instruction sequences for 32-bit addresses
- ✅ **Code Density**: Impact of instruction compression on program size
- ✅ **Hardware Interface**: Preparation of binaries for embedded deployment

### **Development Workflow:**
- ✅ **Tool Proficiency**: Advanced usage of objdump and objcopy utilities
- ✅ **Debug Techniques**: Binary analysis for troubleshooting and verification
- ✅ **Deployment Preparation**: Format conversion for target hardware
- ✅ **Quality Assurance**: Verification of compilation and linking results

## 🔗 Next Steps

With binary analysis complete, proceed to:
- **Task 5**: RISC-V ABI and register convention comprehensive reference
- **Advanced Topics**: Debugging with GDB, performance analysis, and optimization

---

# 📚 Task 5: RISC-V ABI & Register Cheat-Sheet

[![RISC-V](https://img.shields.io/badge/Architecture-RISC--V-blue.svg)](https://riscv.org/)
[![ABI](https://img.shields.io/badge/Standard-ABI%20Reference-green.svg)]()
[![Calling Convention](https://img.shields.io/badge/Convention-RISC--V%20Standard-orange.svg)]()
[![Status](https://img.shields.io/badge/Status-✅%20Complete-success.svg)]()

## 🎯 Objective

Create a comprehensive RISC-V Application Binary Interface (ABI) reference guide that maps all 32 integer registers to their ABI names and documents the complete calling convention rules for function arguments, return values, and register preservation requirements.[1][2]

## 📋 Prerequisites

- ✅ Tasks 1-4 completed: Understanding of RISC-V assembly and binary analysis
- ✅ Assembly analysis from Task 3 showing register usage patterns
- ✅ Python3 available in WSL environment for table generation
- ✅ Understanding of function calling mechanisms and stack operations

## 🚀 Step-by-Step Implementation

### Step 1: Create Register Reference Generator

Create a Python script to generate the complete RISC-V register mapping table.
```bash
nano risc_v_registers.py
```

**Python Code for Register Table:**
```bash
import pandas as pd

Define complete RISC-V register information
registers = [
{"Register": "x0", "ABI Name": "zero", "Type": "Special", "Calling Convention": "Hard-wired zero", "Description": "Always contains value 0, writes ignored"},
{"Register": "x1", "ABI Name": "ra", "Type": "Link", "Calling Convention": "Return address", "Description": "Caller-saved, holds return address"},
{"Register": "x2", "ABI Name": "sp", "Type": "Pointer", "Calling Convention": "Stack pointer", "Description": "Callee-saved, points to top of stack"},
{"Register": "x3", "ABI Name": "gp", "Type": "Pointer", "Calling Convention": "Global pointer", "Description": "Thread-local, points to global data"},
{"Register": "x4", "ABI Name": "tp", "Type": "Pointer", "Calling Convention": "Thread pointer", "Description": "Thread-local, points to thread data"},
{"Register": "x5", "ABI Name": "t0", "Type": "Temporary", "Calling Convention": "Caller-saved", "Description": "Temporary register 0"},
{"Register": "x6", "ABI Name": "t1", "Type": "Temporary", "Calling Convention": "Caller-saved", "Description": "Temporary register 1"},
{"Register": "x7", "ABI Name": "t2", "Type": "Temporary", "Calling Convention": "Caller-saved", "Description": "Temporary register 2"},
{"Register": "x8", "ABI Name": "s0/fp", "Type": "Saved", "Calling Convention": "Callee-saved", "Description": "Saved register 0 / Frame pointer"},
{"Register": "x9", "ABI Name": "s1", "Type": "Saved", "Calling Convention": "Callee-saved", "Description": "Saved register 1"},
{"Register": "x10", "ABI Name": "a0", "Type": "Argument", "Calling Convention": "Arg 0 / Return 0", "Description": "Function argument 0 / Return value 0"},
{"Register": "x11", "ABI Name": "a1", "Type": "Argument", "Calling Convention": "Arg 1 / Return 1", "Description": "Function argument 1 / Return value 1"},
{"Register": "x12", "ABI Name": "a2", "Type": "Argument", "Calling Convention": "Argument 2", "Description": "Function argument 2"},
{"Register": "x13", "ABI Name": "a3", "Type": "Argument", "Calling Convention": "Argument 3", "Description": "Function argument 3"},
{"Register": "x14", "ABI Name": "a4", "Type": "Argument", "Calling Convention": "Argument 4", "Description": "Function argument 4"},
{"Register": "x15", "ABI Name": "a5", "Type": "Argument", "Calling Convention": "Argument 5", "Description": "Function argument 5"},
{"Register": "x16", "ABI Name": "a6", "Type": "Argument", "Calling Convention": "Argument 6", "Description": "Function argument 6"},
{"Register": "x17", "ABI Name": "a7", "Type": "Argument", "Calling Convention": "Argument 7", "Description": "Function argument 7"},
{"Register": "x18", "ABI Name": "s2", "Type": "Saved", "Calling Convention": "Callee-saved", "Description": "Saved register 2"},
{"Register": "x19", "ABI Name": "s3", "Type": "Saved", "Calling Convention": "Callee-saved", "Description": "Saved register 3"},
{"Register": "x20", "ABI Name": "s4", "Type": "Saved", "Calling Convention": "Callee-saved", "Description": "Saved register 4"},
{"Register": "x21", "ABI Name": "s5", "Type": "Saved", "Calling Convention": "Callee-saved", "Description": "Saved register 5"},
{"Register": "x22", "ABI Name": "s6", "Type": "Saved", "Calling Convention": "Callee-saved", "Description": "Saved register 6"},
{"Register": "x23", "ABI Name": "s7", "Type": "Saved", "Calling Convention": "Callee-saved", "Description": "Saved register 7"},
{"Register": "x24", "ABI Name": "s8", "Type": "Saved", "Calling Convention": "Callee-saved", "Description": "Saved register 8"},
{"Register": "x25", "ABI Name": "s9", "Type": "Saved", "Calling Convention": "Callee-saved", "Description": "Saved register 9"},
{"Register": "x26", "ABI Name": "s10", "Type": "Saved", "Calling Convention": "Callee-saved", "Description": "Saved register 10"},
{"Register": "x27", "ABI Name": "s11", "Type": "Saved", "Calling Convention": "Callee-saved", "Description": "Saved register 11"},
{"Register": "x28", "ABI Name": "t3", "Type": "Temporary", "Calling Convention": "Caller-saved", "Description": "Temporary register 3"},
{"Register": "x29", "ABI Name": "t4", "Type": "Temporary", "Calling Convention": "Caller-saved", "Description": "Temporary register 4"},
{"Register": "x30", "ABI Name": "t5", "Type": "Temporary", "Calling Convention": "Caller-saved", "Description": "Temporary register 5"},
{"Register": "x31", "ABI Name": "t6", "Type": "Temporary", "Calling Convention": "Caller-saved", "Description": "Temporary register 6"}
]

print("=" * 100)
print("RISC-V 32-bit Integer Register Mapping")
print("=" * 100)

Create and display the main table
df = pd.DataFrame(registers)
print(df.to_string(index=False))

print("\n" + "=" * 100)
print("RISC-V Calling Convention Summary")
print("=" * 100)

print("\n🔹 Function Arguments and Return Values:")
print(" - a0-a7 (x10-x17): Function arguments and return values")
print(" - a0-a1: Also used for return values (up to 64-bit returns)")
print(" - Arguments beyond a7 passed on stack")

print("\n🔹 Callee-Saved Registers (functions must preserve these if used):")
print(" - sp (x2): Stack pointer - must always be preserved")
print(" - s0-s11 (x8-x9, x18-x27): Saved registers - function must restore if modified")
print(" - s0/fp (x8): Often used as frame pointer")

print("\n🔹 Caller-Saved Registers (functions can freely modify these):")
print(" - ra (x1): Return address - caller saves if needed across calls")
print(" - t0-t6 (x5-x7, x28-x31): Temporary registers - no preservation required")
print(" - a0-a7 (x10-x17): Argument registers - caller saves if values needed after call")

print("\n🔹 Special Purpose Registers:")
print(" - zero (x0): Always zero, writes ignored")
print(" - gp (x3): Global pointer for accessing global variables")
print(" - tp (x4): Thread pointer for thread-local storage")

print("\n" + "=" * 100)
print("Task 5 Complete: RISC-V ABI & Register Cheat-Sheet Generated")
print("=" * 100)
```


### Step 2: Install Required Dependencies

Ensure Python pandas is available for table formatting.

Check if pandas is installed
```bash
python3 -c "import pandas; print('pandas is available')"
```
Install pandas if not available
```bash
pip3 install pandas
```

### Step 3: Generate the Register Reference Table

Execute the Python script to generate the complete RISC-V register reference.

Run the register table generator
```bash
python3 risc_v_registers.py
```
Save output to file for reference
```bash
python3 risc_v_registers.py > risc_v_register_reference.txt
```
View the saved reference
```bash
cat risc_v_register_reference.txt
```
### Step 4: Verify Register Usage in Your Assembly

Cross-reference the generated table with your Task 3 assembly output to confirm register usage patterns.

Compare register usage in your assembly with ABI conventions
```bash
grep -E "(ra|sp|s0|a0|a5)" hello.s
```
Analyze register preservation in your main function
```bash
grep -A 20 "main:" hello.s | grep -E "(sw|lw|mv|li)"
```

## 📊 Complete RISC-V Register Reference

Based on your successful implementation, here's the comprehensive register mapping:

### 🔧 **Register Categories and Usage:**

#### **🔒 Special Purpose Registers:**
| Register | ABI Name | Purpose | Convention |
|----------|----------|---------|------------|
| x0 | zero | Hard-wired zero | Always 0, writes ignored |
| x1 | ra | Return address | Caller-saved link register |
| x2 | sp | Stack pointer | Callee-saved, 16-byte aligned |
| x3 | gp | Global pointer | Thread-local data access |
| x4 | tp | Thread pointer | Thread-local storage |

#### **📤 Function Argument & Return Registers:**
| Register | ABI Name | Purpose | Usage in Your Code |
|----------|----------|---------|-------------------|
| x10 | a0 | Argument 0 / Return 0 | String pointer to printf, return value |
| x11 | a1 | Argument 1 / Return 1 | Additional args/64-bit returns |
| x12-x17 | a2-a7 | Arguments 2-7 | Function parameter passing |

#### **💾 Callee-Saved Registers (Must Preserve):**
| Register | ABI Name | Purpose | Observed in Assembly |
|----------|----------|---------|---------------------|
| x8 | s0/fp | Saved 0 / Frame pointer | Used in your prologue/epilogue |
| x9 | s1 | Saved register 1 | Available for local variables |
| x18-x27 | s2-s11 | Saved registers 2-11 | Long-term storage across calls |

#### **⚡ Caller-Saved Registers (Free to Modify):**
| Register | ABI Name | Purpose | Usage Pattern |
|----------|----------|---------|---------------|
| x5-x7 | t0-t2 | Temporary 0-2 | Short-term computation |
| x28-x31 | t3-t6 | Temporary 3-6 | Additional temporaries |

### 🎯 **Calling Convention Rules:**

#### **Function Call Sequence:**
1. **Arguments**: Pass in a0-a7, additional on stack
2. **Call**: `call` instruction saves return address in ra
3. **Prologue**: Callee saves ra, fp, allocates stack
4. **Body**: Function execution with preserved registers
5. **Epilogue**: Restore saved registers, deallocate stack
6. **Return**: `ret` instruction (equivalent to `jr ra`)

#### **Register Preservation Requirements:**

MUST PRESERVE (Callee responsibility):

sp (x2): Stack pointer

s0-s11 (x8-x9, x18-x27): Saved registers

gp (x3), tp (x4): Thread-local pointers

MAY MODIFY (Caller responsibility):

ra (x1): Return address

t0-t6 (x5-x7, x28-x31): Temporaries

a0-a7 (x10-x17): Arguments/returns


## 📸 Implementation Output

![Task 5 Register Reference](screenshots/task5_register_reference.png)

*Screenshot demonstrating complete RISC-V register mapping table generation with ABI names, calling conventions, and comprehensive usage documentation.*

## ⚠️ Troubleshooting Guide

### Common Issues and Solutions:

| Issue | Symptom | Root Cause | Solution |
|-------|---------|------------|----------|
| **Pandas Not Found** | `ModuleNotFoundError: No module named 'pandas'` | Missing dependency | Install: `pip3 install pandas` |
| **Table Format Issues** | Misaligned output | Terminal width | Use: `python3 script.py \| less -S` |
| **Permission Denied** | Cannot save output file | Directory permissions | Check: `ls -la .` and adjust permissions |
| **Python Not Found** | `python3: command not found` | Python not installed | Install: `sudo apt install python3` |

### Debugging Commands:
Verify Python installation
```bash
python3 --version
```
Check pandas availability
```bash
python3 -c "import pandas as pd; print(pd.version)"
```
Test script syntax
```bash
python3 -m py_compile risc_v_registers.py
```
Run with error handling
```bash
python3 risc_v_registers.py 2>&1 | tee register_output.log
```

## 🎉 Success Criteria

Task 5 is considered **complete** when:
- [x] Complete 32-register mapping table generated successfully
- [x] All ABI names (zero, ra, sp, gp, tp, t0-t6, s0-s11, a0-a7) documented
- [x] Calling convention rules clearly explained and categorized
- [x] Register preservation requirements understood and documented
- [x] Cross-reference with Tasks 1-4 assembly code completed
- [x] Comprehensive reference available for future RISC-V development

## 💡 Key Learning Outcomes

### **ABI Knowledge Mastery:**
- ✅ **Register Allocation**: Complete understanding of RISC-V register purposes
- ✅ **Calling Conventions**: Mastery of argument passing and return mechanisms
- ✅ **Stack Management**: Understanding of frame pointer and stack operations
- ✅ **Code Analysis**: Ability to analyze assembly for ABI compliance

### **Development Skills:**
- ✅ **Reference Creation**: Building comprehensive documentation and lookup tables
- ✅ **Cross-Platform Tools**: Using Python for development utility creation
- ✅ **Assembly Correlation**: Connecting theoretical ABI to practical implementation
- ✅ **Debugging Capability**: Understanding register usage for troubleshooting

### **Professional Competencies:**
- ✅ **Technical Documentation**: Creating clear, comprehensive reference materials
- ✅ **System Architecture**: Deep understanding of processor calling conventions
- ✅ **Code Quality**: Ensuring ABI compliance in development work
- ✅ **Knowledge Transfer**: Building reusable reference tools for team use

## 🔗 Next Steps

With complete ABI mastery achieved, explore advanced topics:
- **Performance Optimization**: Register allocation strategies
- **Debugging Techniques**: Using GDB with RISC-V register knowledge
- **Advanced Assembly**: Hand-optimized assembly routines
- **System Programming**: Operating system and bare-metal development

---

