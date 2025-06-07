# 🔗 Week 1: RISC-V Bare-Metal Toolchain & Debugging

This document outlines the tasks completed in Week 1 of the RISC-V SoC Lab, focusing on setting up the bare-metal RISC-V toolchain, cross-compiling, analyzing assembly, debugging, and understanding core RISC-V concepts.

---

## 1. RISC-V Toolchain Setup and Verification

This section details the process of unpacking the xPack RISC-V toolchain, adding it to the system's PATH, and verifying the functionality of `gcc`, `objdump`, and `gdb` binaries.

---

### ✅ Steps:

---

### 🔽 1. **Download the Toolchain**

Used `wget` to download the `xpack-riscv-none-elf-gcc` archive.

```bash
wget https://github.com/xpack-dev-tools/riscv-none-elf-gcc-xpack/releases/download/v14.2.0-3/xpack-riscv-none-elf-gcc-14.2.0-3-linux-x64.tar.gz



#### 2. Unpack the Archive
Extracted the contents of the downloaded tarball, creating a folder named `xpack-riscv-none-elf-gcc-14.2.0-3`.

tar -xzf xpack-riscv-none-elf-gcc-14.2.0-3-linux-x64.tar.gz


#### 3. Add the Toolchain to PATH (Temporarily)
Added the toolchain binaries to the current session's PATH.

'''bash
export PATH=$PWD/xpack-riscv-none-elf-gcc-14.2.0-3/bin:$PATH
'''


#### 4. Check if Binaries Work
Verified the functionality of `gcc`, `objdump`, and `gdb` by checking their versions.

riscv-none-elf-gcc --version
riscv-none-elf-objdump --version
riscv-none-elf-gdb --version

### Output:
![Toolchain Setup Output](screenshots/task1_toolchain_verification.png)

*Screenshot showing successful installation and version verification of RISC-V toolchain binaries*
