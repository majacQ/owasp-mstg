---
title: Ghidra
platform: generic
source: https://github.com/NationalSecurityAgency/ghidra
---

[Ghidra](https://github.com/NationalSecurityAgency/ghidra) is an open source software reverse engineering (SRE) suite of tools developed by the United State of America's National Security Agency's (NSA) Research Directorate. Ghidra is a versatile tool which comprises of a disassembler, decompiler and a built-in scripting engine for advanced usage. Please refer to the [installation guide](https://ghidra-sre.org/InstallationGuide.html "Ghidra Installation Guide") on how to install it and also look at the [cheat sheet](https://ghidra-sre.org/CheatSheet.html "Cheat Sheet") for a first overview of available commands and shortcuts. In this section, we will have walk-through on how to create a project, view disassembly and decompiled code for a binary.

Start Ghidra using `ghidraRun` (\*nix) or `ghidraRun.bat` (Windows), depending on the platform you are on. Once Ghidra is fired up, create a new project by specifying the project directory. You will be greeted by a window as shown below:

<img src="Images/Chapters/0x04c/Ghidra_new_project.png" width="100%" />

In your new **Active Project** you can import an app binary by going to **File** -> **Import File** and choosing the desired file.

<img src="Images/Chapters/0x04c/Ghidra_import_binary.png" width="100%" />

If the file can be properly processed, Ghidra will show meta-information about the binary before starting the analysis.

<img src="Images/Chapters/0x04c/Ghidra_elf_import.png" width="100%" />

To get the disassembled code for the binary file chosen above, double click the imported file from the **Active Project** window. Click **yes** and **analyze** for auto-analysis on the subsequent windows. Auto-analysis will take some time depending on the size of the binary, the progress can be tracked in the bottom right corner of the code browser window. Once auto-analysis is completed you can start exploring the binary.

<img src="Images/Chapters/0x04c/Ghidra_main_window.png" width="100%" />

The most important windows to explore a binary in Ghidra are the **Listing** (Disassembly) window, the **Symbol Tree** window and the **Decompiler** window, which shows the decompiled version of the function selected for disassembly. The **Display Function Graph** option shows control flow graph of the selected function.

<img src="Images/Chapters/0x04c/Ghidra_function_graph.png" width="100%" />

There are many other functionalities available in Ghidra and most of them can be explored by opening the **Window** menu. For example, if you want to examine the strings present in the binary, open the **Defined Strings** option. We will discuss other advanced functionalities while analyzing various binaries for Android and iOS platforms in the coming chapters.

<img src="Images/Chapters/0x04c/Ghidra_string_window.png" width="100%" />
