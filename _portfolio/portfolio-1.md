<!-- ---
title: "Portfolio item number 1"
excerpt: "Short description of portfolio item number 1<br/><img src='/images/500x300.png'>"
collection: portfolio
--- -->

---
title: "交叉編譯 SPEC CPU2017 for RISC-V"
excerpt: "使用 riscv-gnu-toolchain 交叉編譯 SPEC2017 的完整步驟"
date: 2026-03-17
---

# Environmental Preparation

The previous review comments mentioned that the paper needed more benchmarks and recommended using spec 2017 for testing. Therefore, this document serves as a record of cross-compiling spec 2017 to run on a RISCV virtual machine.

The RISCV cross-compiler has already been installed in previous work, and Speccpu2017 also needs to be installed.

Use the following command to check if the specified cross-compiler exists.

```cpp
riscv64-linux-gnu-gcc
riscv64-linux-gnu-g++
riscv64-linux-gnu-gfortan
```

# Install Speccpu2017

By mounting the ISO image and switching to the /mnt path

```cpp
mount cpu2017-1.0.5.iso /mnt/
cd /mnt/
```

Install using the command in the /mnt directory.
```cpp
./install.sh
```

There you need to enter the file path and confirm.

# Running speccpu2017
## Modify The Configuration File

In the `speccpu2017/config` directory, there are configuration files that come with `speccpu2017`. We can copy them and modify the corresponding code for use. Here, for the RISCV architecture, I copy the `Example-gcc-linux-aarch64.cfg` file and rename the copied file to `riscv.cfg`.

```c++
CC                  = riscv-linux-gnu-gcc -std=c99 %{model}
CXX                 = riscv-linux-gnu-g++ -std=c++03 %{model}
FC                  = riscv-linux-gnu-gfortran %{model}
```

## Compile to Generate The SPEC Executable File

To compile the benchmark within speccpu2017, enter the following command in the terminal.

```c++
source shrc
runcpu --config=riscv --action=setup --size=ref all # Activate the spec environment
```