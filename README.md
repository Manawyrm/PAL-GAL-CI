CI for PAL/GAL developmenmt
====================================

This repo contains an example CI GitHub Actions workflow,
which will build PAL/GAL .jed files from .pld definitions.  
  
[POST.PLD](https://github.com/Manawyrm/PAL-GAL-CI/blob/main/POST.PLD) contains an example PLD setup (for an ISA 0x80 POST debug card decoder, written by PC Engines)  
  
The CI pipeline will run [WinCUPL's CUPL.EXE](https://github.com/Manawyrm/PAL-GAL-CI/tree/main/toolchain) via wine and create POST.jed, which can be flashed to any fitting PAL/GAL via normal programmers like the TL866.  

Running 32bit Windows utilities in a CI pipeline requires the installation of Wine and a virtual X server to be running.  
[The main.yml workflow](https://github.com/Manawyrm/PAL-GAL-CI/blob/main/.github/workflows/main.yml) does this by running Xvfb in the background. 

### Additional parameters for CUPL.EXE

```
Usage: cupl -flags <library.dl> <device> file
       Where: flags
       Download file formats
-j   JEDEC output (jedec compatible programming file)
-h   ASCII-hex output (PROM ASCII hex download file)
-i   HL output (Signetics IFL devices only)
-n   download filename=PLD filename
       Output file formats
-a   absolute file (required for simulation)
-l   listing file (generate error listing file)
-e   expanded macro definition file (macros and REPEAT statements)
-x   expanded product terms (generate documentation file)
-f   fuse plot/chip diagram (attach fuse plot in documentation file)
-b   Berkeley PLA file (generate a Berkeley PLA file for fitters)
-o   One-hot-bit state machine (use one-hot bit encoding for state machines)
-d   Deactivate unused OR terms (remove unused product terms in IFL devices)
-r   Disable product term merging (identical p-term generation in IFL devices)
-g   Program security fuse (blow security fuse after programming if supported)
-u   Use specified library, must immediatley preceed library name (override library specified in the environment)
-s   Simulate after compile (simulate after successful compilation)
       Optimization methods
-kb  Optimize product term usage (overrides the DEMORGAN statement in file)
-kd  Demorganize all pins and pinnodes (overrides the DEMORGAN statement)
-ks  Force product term sharing (enable group reduction)
-kx  Do not exapnd XOR equations (virtual or fitter that supports XOR gates)
       Minimization Method
-m0  No minimization (no logic reduction)
-m1  Quick minimization (default. Lowest memory usage and fastest)
-m2  Quine-McCluskey minimization (highest memory usage and slowest.)
-m3  Presto minimization (good trade-off between memory and speed)
-m4  Espresso mimization (high memory usage and slow. Good for fitter designs)
```