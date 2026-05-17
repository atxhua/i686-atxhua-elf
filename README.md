# i686-atxhua-elf-tools

A multi runtime library ( Newlib + Picolib) cross-compiler ecosystem designed for bare-metal `i686-elf` targets. This repository contains the automated build orchestration framework that integrates **GNU Binutils**, **GCC**, **GDB**, **Newlib**, and **Picolibc** into a single unified toolchain distribution.

## The 2D Compilation Matrix

### The Supported Compilation Grid:
| Machine Profile Flag | Axis 1: Standard Runtime (Newlib) | Axis 2: Low-Footprint Runtime (Picolibc) |
| :--- | :--- | :--- |
| **`.`** (Generic x86 + FPU) | Standard x86 Newlib | Lean Generic Picolibc |
| **`-msoft-float`** (No-FPU Emulation) | Emulated Software-Float Newlib | Emulated Software-Float Picolibc |
| **`-march=pentium4`** (SSE2 Optimization) | Pentium 4 Vectorized Newlib | Pentium 4 Vectorized Picolibc |
| **`-msoft-float -march=pentium4`** | FPU-less Pentium 4 Optimized Newlib | FPU-less Pentium 4 Optimized Picolibc |

---

## Usage: Selecting the C Library Runtime
Because this toolchain embeds both runtimes into a unified system root, switching between standard POSIX **Newlib** and the ultra-lean **Picolibc** requires zero re-configuration of the compiler binaries. The runtime selection is controlled completely by passing or omitting the target specs parameter during the C Compilation and Linking stages.

### Using Newlib
By default, the compiler routes all standard header and linking procedures to the Newlib runtime. No extra specification flags are required.
```bash
i686-atxhua-elf-gcc main.c -o output.elf
````

### Using Picolib
To swap the entire target framework to Picolibc, append the explicit `-specs=picolibc.specs` flag during both the compilation and linking stages.
```bash
i686-atxhua-elf-gcc -specs=picolibc.specs main.c -o output.elf
````


## 📊 ELF Size Comparison
The following figures demonstrate the binary footprint differences compiled for rt-thread/bsp/x86. Swapping to Picolibc shaves off roughly ~40 KB from the final executable image.
### Newlib 
```
text	   data	    bss	    dec	    hex	filename
212900	   3274	  10844	 227018	  376ca	rtthread.elf
```
### Picolib 
```
   text	   data	    bss	    dec	    hex	filename
 174122	   2568	  10528	 187218	  2db52	rtthread.elf
```
