# Mòj Driver Blyat

A Windows kernel driver project implementing memory and hook functionality. This driver provides low-level system access capabilities with memory manipulation and function hooking features.

## Overview

This project is a Windows kernel mode driver that implements:

- Memory manipulation utilities
- Function hooking mechanisms
- Kernel-level operations

## Why This Driver?

This driver stands out for several reasons:

1. **Efficient Memory Operations**: Optimized memory manipulation routines for high-performance operations
2. **Robust Hook Implementation**: Reliable function hooking mechanism with minimal overhead
3. **Kernel-Level Access**: Direct access to system resources for advanced operations
4. **Stability**: Carefully implemented error handling and safety checks
5. **Modular Design**: Easy to extend and modify for different use cases
6. **Compatibility**: Works across multiple Windows versions with proper configuration

## Project Structure

```
├── main.cpp           # Driver entry point
├── memory.cpp/h       # Memory manipulation functionality
├── hook.cpp/h         # Function hooking implementation
├── definitions.h      # Common definitions and structures
└── MòjDriverBlyat.inf # Driver information file
```

## Requirements

- Windows SDK
- Visual Studio (with Windows Driver Kit)
- Windows Driver Kit (WDK)
- A Windows machine with test signing enabled for driver testing

## Building

1. Open the solution file `Mòj Driver Blyat.sln` in Visual Studio
2. Ensure you have the Windows Driver Kit installed
3. Select your target architecture (x64)
4. Build the solution

## Installation

### Method 1: Standard Installation

**Warning**: Installing unsigned drivers requires test signing mode to be enabled on the target machine.

1. Enable test signing mode:
   ```powershell
   bcdedit /set testsigning on
   ```
2. Restart your system
3. Use the Windows Device Manager or SC commands to install the driver

### Method 2: Using KDMapper

KDMapper is a tool that allows loading unsigned drivers without enabling test signing mode. Here's how to use it:

1. Download KDMapper from a trusted source
2. Open an elevated command prompt
3. Navigate to the directory containing KDMapper and your driver
4. Run the following command:
   ```powershell
   kdmapper.exe MòjDriverBlyat.sys
   ```

**Important Notes about KDMapper:**

- KDMapper bypasses driver signing requirements
- Use with caution as it may trigger security software
- Not recommended for production environments
- May not work on all Windows versions
- Always verify the integrity of KDMapper before use

## Security Considerations

This driver operates at kernel level and has full system access. Use with caution as improper use can lead to system instability or security vulnerabilities.

## Disclaimer

This driver is for educational and development purposes only. Use at your own risk. Improper use of kernel drivers can lead to system crashes and data loss.

## License

[Add your chosen license here]
