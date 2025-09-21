# About AHIR

AHIR is a High-Level Synthesis framework to generate hardware description that can be fed to an ASIC flow or can be deployed on an FPGA.

AHIR framework has its own design entry language called **Aa**. It is an imperative language with emphasis on single assignment programming. AHIR also has a bridge from LLVM compiler's SSA representation to the Aa representation, allowing virtually any programming language that LLVM supports to be used for design entry, with certain reasonable restrictions, such as on recursion and system calls.
