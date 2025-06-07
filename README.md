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

Create a comprehensive RISC-V Application Binary Interface (ABI) reference guide that maps all 32 integer registers to their ABI names and documents the complete calling convention rules for function arguments, return values, and register preservation requirements.[2][3]

## 📋 Prerequisites

- ✅ Tasks 1-4 completed: Understanding of RISC-V assembly and binary analysis
- ✅ Assembly analysis from Task 3 showing register usage patterns
- ✅ Basic understanding of function calling mechanisms and stack operations
- ✅ WSL environment with working RISC-V toolchain

## 🚀 Step-by-Step Implementation

### Step 1: Create Complete Register Reference Table

Create a markdown table documenting all 32 RISC-V integer registers with their ABI names and calling conventions.

Create register reference table
```bash
nano risc_v_register_table.md
```


**Complete Register Mapping Table:**


## 📋 Complete RISC-V Integer Register Reference

| Register | ABI Name | Type | Calling Convention | Preserved Across Calls | Description |
|----------|----------|------|-------------------|------------------------|-------------|
| **x0** | `zero` | Special | Hard-wired zero | — (Immutable) | Always contains 0, writes ignored |
| **x1** | `ra` | Link | Caller-saved | No | Return address |
| **x2** | `sp` | Pointer | Callee-saved | Yes | Stack pointer (16-byte aligned) |
| **x3** | `gp` | Pointer | Unallocatable | — | Global pointer |
| **x4** | `tp` | Pointer | Unallocatable | — | Thread pointer |
| **x5** | `t0` | Temporary | Caller-saved | No | Temporary register 0 |
| **x6** | `t1` | Temporary | Caller-saved | No | Temporary register 1 |
| **x7** | `t2` | Temporary | Caller-saved | No | Temporary register 2 |
| **x8** | `s0/fp` | Saved | Callee-saved | Yes | Saved register 0 / Frame pointer |
| **x9** | `s1` | Saved | Callee-saved | Yes | Saved register 1 |
| **x10** | `a0` | Argument | Caller-saved | No | Function argument 0 / Return value 0 |
| **x11** | `a1` | Argument | Caller-saved | No | Function argument 1 / Return value 1 |
| **x12** | `a2` | Argument | Caller-saved | No | Function argument 2 |
| **x13** | `a3` | Argument | Caller-saved | No | Function argument 3 |
| **x14** | `a4` | Argument | Caller-saved | No | Function argument 4 |
| **x15** | `a5` | Argument | Caller-saved | No | Function argument 5 |
| **x16** | `a6` | Argument | Caller-saved | No | Function argument 6 |
| **x17** | `a7` | Argument | Caller-saved | No | Function argument 7 |
| **x18** | `s2` | Saved | Callee-saved | Yes | Saved register 2 |
| **x19** | `s3` | Saved | Callee-saved | Yes | Saved register 3 |
| **x20** | `s4` | Saved | Callee-saved | Yes | Saved register 4 |
| **x21** | `s5` | Saved | Callee-saved | Yes | Saved register 5 |
| **x22** | `s6` | Saved | Callee-saved | Yes | Saved register 6 |
| **x23** | `s7` | Saved | Callee-saved | Yes | Saved register 7 |
| **x24** | `s8` | Saved | Callee-saved | Yes | Saved register 8 |
| **x25** | `s9` | Saved | Callee-saved | Yes | Saved register 9 |
| **x26** | `s10` | Saved | Callee-saved | Yes | Saved register 10 |
| **x27** | `s11` | Saved | Callee-saved | Yes | Saved register 11 |
| **x28** | `t3` | Temporary | Caller-saved | No | Temporary register 3 |
| **x29** | `t4` | Temporary | Caller-saved | No | Temporary register 4 |
| **x30** | `t5` | Temporary | Caller-saved | No | Temporary register 5 |
| **x31** | `t6` | Temporary | Caller-saved | No | Temporary register 6 |

### 📊 Register Category Summary

#### 🔒 **Special Purpose Registers**
| Registers | Purpose | Notes |
|-----------|---------|-------|
| `x0 (zero)` | Hard-wired zero | Always 0, writes ignored |
| `x1 (ra)` | Return address | Link register for function calls |
| `x2 (sp)` | Stack pointer | Must maintain 16-byte alignment |
| `x3 (gp)` | Global pointer | Unallocatable by compiler |
| `x4 (tp)` | Thread pointer | Unallocatable by compiler |

#### ⚡ **Temporary Registers (Caller-Saved)**
| Registers | ABI Names | Usage |
|-----------|-----------|-------|
| `x5-x7` | `t0-t2` | Temporary computation, not preserved across calls |
| `x28-x31` | `t3-t6` | Additional temporary registers |

#### 💾 **Saved Registers (Callee-Saved)**
| Registers | ABI Names | Usage |
|-----------|-----------|-------|
| `x8` | `s0/fp` | Saved register 0 / Optional frame pointer |
| `x9` | `s1` | Saved register 1 |
| `x18-x27` | `s2-s11` | Saved registers 2-11 (10 registers total) |

#### 📤 **Argument/Return Registers (Caller-Saved)**
| Registers | ABI Names | Usage |
|-----------|-----------|-------|
| `x10-x11` | `a0-a1` | First 2 arguments / Return values (up to 64-bit) |
| `x12-x17` | `a2-a7` | Additional function arguments |

### 🎯 **Calling Convention Rules**

#### **Function Call Protocol:**
1. **Arguments**: Pass first 8 in `a0-a7`, additional on stack
2. **Return Values**: Use `a0-a1` for up to 64-bit returns
3. **Register Preservation**: 
   - **MUST preserve**: `sp`, `s0-s11` (if used)
   - **MAY modify**: `ra`, `t0-t6`, `a0-a7`
4. **Stack**: 16-byte aligned, grows downward
5. **Frame Pointer**: Optional, uses `s0/fp` if present

#### **Register Usage Guidelines:**
- **Caller-Saved**: Calling function must save if values needed after call
- **Callee-Saved**: Called function must preserve if modified
- **Unallocatable**: `gp` and `tp` should not be modified by user code

- 
### Step 2: Document Calling Convention Rules

Create a comprehensive summary of RISC-V calling convention rules.

Create calling convention summary
```bash
nano calling_convention_summary.md
```

**RISC-V Calling Convention Summary:**

Function Arguments and Return Values:
a0-a7 (x10-x17): Function arguments and return values

a0-a1: Also used for return values (up to 64-bit returns)

Arguments beyond a7: Passed on stack

Callee-Saved Registers (Must preserve if used):
sp (x2): Stack pointer - MUST always be preserved

s0-s11 (x8-x9, x18-x27): Saved registers - function must restore if modified

s0/fp (x8): Often used as frame pointer

Caller-Saved Registers (Can freely modify):
ra (x1): Return address - caller saves if needed across calls

t0-t6 (x5-x7, x28-x31): Temporary registers - no preservation required

a0-a7 (x10-x17): Argument registers - caller saves if values needed after call

Special Purpose Registers:
zero (x0): Always zero, writes ignored

gp (x3): Global pointer - unallocatable by compiler

tp (x4): Thread pointer - unallocatable by compiler


### Step 3: Verify ABI Compliance with Your Assembly Code

Cross-reference the register usage in your Task 3 assembly with the ABI conventions.

Check register usage in your hello.s assembly
```bash
grep -E "(ra|sp|s0|a0|a5)" hello.s
```
Analyze specific register preservation patterns
```bash
grep -A 20 "main:" hello.s | grep -E "(sw|lw|addi.*sp)"
```

### Step 4: Create Register Lookup Utility

Build a practical utility script for quick register reference during development.

Create register lookup script
```bash
nano register_lookup.sh
```

**Register Lookup Script:**
```bash
#!/bin/bash

RISC-V Register Quick Lookup
case $1 in
"x0"|"zero") echo "x0 (zero): Hard-wired zero, always 0" ;;
"x1"|"ra") echo "x1 (ra): Return address, caller-saved" ;;
"x2"|"sp") echo "x2 (sp): Stack pointer, callee-saved, 16-byte aligned" ;;
"x8"|"s0"|"fp") echo "x8 (s0/fp): Saved register 0 / Frame pointer, callee-saved" ;;
"x10"|"a0") echo "x10 (a0): Argument 0 / Return value 0, caller-saved" ;;
"t") echo "Temporary register: caller-saved, can be freely modified" ;;
"s") echo "Saved register: callee-saved, must be preserved if used" ;;
"a"*) echo "Argument register: caller-saved, used for function parameters" ;;
*) echo "Usage: $0 <register_name>" ;;
esac
```
Make script executable and test
```bash
chmod +x register_lookup.sh
./register_lookup.sh ra
./register_lookup.sh s0
```


### Step 5: Create Comprehensive ABI Cheat Sheet

Combine all information into a single, comprehensive reference document.
Create complete cheat sheet
```bash
nano risc_v_abi_cheatsheet.txt
```

**Complete Cheat Sheet Content:**
================================================================================
RISC-V ABI & Register Cheat-Sheet
REGISTER CATEGORIES:
Special: x0 (zero), x1 (ra), x2 (sp), x3 (gp), x4 (tp)
Temporaries: x5-x7 (t0-t2), x28-x31 (t3-t6) [Caller-saved]
Saved: x8-x9 (s0-s1), x18-x27 (s2-s11) [Callee-saved]
Arguments: x10-x17 (a0-a7) [Caller-saved]

CALLING CONVENTION RULES:
Arguments: a0-a7 for first 8 parameters, stack for additional
Returns: a0-a1 for return values (up to 64-bit)
Preservation: Functions MUST preserve sp, s0-s11 if used
Functions CAN modify ra, t0-t6, a0-a7 freely
Stack: 16-byte aligned, grows downward
Frame: s0/fp optional frame pointer

REGISTER USAGE IN YOUR CODE:
From hello.s assembly analysis:

sp: Stack operations (addi sp,sp,-16)

ra: Return address (sw ra,12(sp))

s0: Frame pointer (sw s0,8(sp))

a0: First argument (string pointer)

a5: Temporary calculations

QUICK REFERENCE:
Function Entry: Save ra, s0, allocate stack
Function Body: Use t-regs freely, preserve s-regs
Function Exit: Restore s0, ra, deallocate stack, return


### Step 6: Validate ABI Compliance

Perform final verification of ABI compliance using your actual assembly code.

Verify ABI compliance in your code
```bash
echo "=== ABI Compliance Check ==="
echo "Stack alignment (should be 16-byte):"
grep "addi.sp.-16" hello.s

echo "Return address preservation:"
grep "sw.*ra" hello.s

echo "Frame pointer usage:"
grep "s0" hello.s

echo "Argument register usage:"
grep "a0" hello.s
```


### 📋 **Register Categories Summary:**

| Category | Registers |  Code Usage | ABI Compliance |
|----------|-----------|-----------------|----------------|
| **Special** | zero, ra, sp, gp, tp | ra, sp preserved correctly | ✅ Perfect |
| **Arguments** | a0-a7 (x10-x17) | a0 for string and return value | ✅ Perfect |
| **Saved** | s0-s11 (x8-x9, x18-x27) | s0 saved and restored properly | ✅ Perfect |
| **Temporary** | t0-t6 (x5-x7, x28-x31) | Available for use, a5 used temporarily | ✅ Perfect |

## 📸 Implementation Output

![Task 5 ABI Reference](Screenshot 2025-06-07 193252.png)

*Screenshot demonstrating complete RISC-V ABI compliance verification with register usage analysis and comprehensive reference documentation.*

## ⚠️ Troubleshooting Guide

### Common Issues and Solutions:

| Issue | Symptom | Root Cause | Solution |
|-------|---------|------------|----------|
| **Script Execution Error** | `./register_lookup.sh: Permission denied` | Missing execute permission | Run: `chmod +x register_lookup.sh` |
| **Missing Assembly File** | `grep: hello.s: No such file` | Assembly not generated | Complete Task 3 first |
| **ABI Violations** | Incorrect register usage | Misunderstanding calling convention | Review register preservation rules |

### Debugging Commands:
Verify all task files exist
```bash
ls -la hello.s risc_v_register_table.md calling_convention_summary.md
```
Check register usage patterns
```bash
grep -E "(x[0-9]+|zero|ra|sp|gp|tp|t[0-6]|s[0-9]+|a[0-7])" hello.s
```
Validate script functionality
```bash
./register_lookup.sh --help
```

### **ABI Compliance Benefits:**
- ✅ **Interoperability**: Code works with any RISC-V compiler/system
- ✅ **Debugging**: Standard conventions enable better debugging tools
- ✅ **Performance**: Optimized register allocation reduces memory access
- ✅ **Maintainability**: Consistent patterns simplify code understanding

## 🎉 Success Criteria

Task 5 is considered **complete** when:
- [x] All 32 registers (x0-x31) mapped to ABI names (zero, ra, sp, gp, tp, t0-t6, s0-s11, a0-a7)
- [x] Calling convention summary documented (a0-a7 args/returns, s-regs callee-saved, t-regs caller-saved)
- [x] Working lookup utility created for development reference
- [x] ABI compliance verified with actual assembly code from Task 3
- [x] Comprehensive cheat sheet available for ongoing RISC-V development

## 💡 Key Learning Outcomes

### **ABI Mastery Achieved:**
- ✅ **Complete Register Knowledge**: All 32 integer registers and their purposes
- ✅ **Calling Convention Expertise**: Understanding of argument passing and preservation rules
- ✅ **Stack Management**: 16-byte alignment and frame pointer usage
- ✅ **Code Analysis Skills**: Ability to verify ABI compliance in assembly code

### **Practical Development Skills:**
- ✅ **Reference Creation**: Built comprehensive documentation for ongoing use
- ✅ **Verification Techniques**: Established methods for checking ABI compliance
- ✅ **Tool Development**: Created utilities for daily development workflow
- ✅ **Cross-Reference Ability**: Connected theoretical knowledge to practical implementation

## 🔗 Next Steps

With complete RISC-V ABI mastery achieved:
- **Advanced Assembly Programming**: Hand-optimized routines with full register knowledge
- **System Programming**: Operating system and bare-metal development
- **Performance Optimization**: Register allocation strategies for performance-critical code
- **Debugging Mastery**: Using register knowledge for effective troubleshooting

---
