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
