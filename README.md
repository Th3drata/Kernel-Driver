<div align="center">

# KDriver

### Windows Kernel Driver for Memory Manipulation & Function Hooking

[![Platform](https://img.shields.io/badge/Platform-Windows%20x64-blue.svg)](https://www.microsoft.com/windows)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Language](https://img.shields.io/badge/Language-C%2B%2B-orange.svg)](https://isocpp.org/)
[![WDK](https://img.shields.io/badge/WDK-Required-red.svg)](https://docs.microsoft.com/en-us/windows-hardware/drivers/download-the-wdk)
[![Build](https://img.shields.io/badge/Build-Visual%20Studio-purple.svg)](https://visualstudio.microsoft.com/)

---

**⚠️ Educational & Research Use Only** | **🔬 Kernel-Level Operations** | **🛡️ Use at Your Own Risk**

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Architecture](#-architecture)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Building](#-building)
- [Deployment](#-deployment)
- [Usage](#-usage)
- [Project Structure](#-project-structure)
- [Development](#-development)
- [Security Considerations](#-security-considerations)
- [FAQ](#-faq)
- [Contributing](#-contributing)
- [License](#-license)
- [Disclaimer](#-disclaimer)

---

## 🎯 Overview

**KDriver** is a lightweight Windows kernel-mode driver designed for low-level system operations. It provides a clean, maintainable foundation for:

- **Memory Operations**: Safe read/write primitives for kernel and process memory
- **Function Hooking**: Minimal scaffolding for research-grade kernel hooks
- **System Research**: Educational tool for understanding Windows kernel internals

The codebase prioritizes clarity and auditability, making it ideal for security researchers, reverse engineers, and systems programmers learning kernel development.

### Why KDriver?

✅ **Minimal & Readable** - No bloat, just essential components  
✅ **Well-Structured** - Clear separation of concerns (entry, memory, hooks)  
✅ **Safe Defaults** - Input validation and error handling built-in  
✅ **Extensible** - Easy to adapt for custom use cases  
✅ **Educational** - Perfect for learning Windows kernel programming  

---

## 🚀 Features

### Core Capabilities

| Feature | Description |
|---------|-------------|
| **Memory Helpers** | Safe wrappers for kernel memory operations with validation |
| **Hook Framework** | Minimal scaffolding for implementing kernel function hooks |
| **IOCTL Support** | Ready for user-mode communication (implement your own handlers) |
| **Clean Architecture** | Modular design with clear component boundaries |
| **Debug Support** | PDB generation and debug output for development |

### Technical Highlights

- 🎯 **x64 Architecture** - Native 64-bit Windows support
- 🔧 **WDM Compatible** - Standard Windows Driver Model
- 📦 **INF-Based Deployment** - Standard installation metadata
- 🛠️ **Visual Studio Integration** - Full IDE support with IntelliSense
- 🔍 **Kernel Debugging Ready** - WinDbg compatible

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────┐
│              KDriver.sys                    │
├─────────────────────────────────────────────┤
│  main.cpp                                   │
│  ├─ DriverEntry()      (Initialization)     │
│  ├─ DriverUnload()     (Cleanup)            │
│  └─ Dispatch Routines  (IRP Handling)       │
├─────────────────────────────────────────────┤
│  memory.cpp / memory.h                      │
│  ├─ Read/Write Helpers                      │
│  ├─ Process Memory Access                   │
│  └─ Validation Logic                        │
├─────────────────────────────────────────────┤
│  hook.cpp / hook.h                          │
│  ├─ Hook Installation                       │
│  ├─ Hook Removal                            │
│  └─ Detour Management                       │
├─────────────────────────────────────────────┤
│  definitions.h                              │
│  ├─ Shared Structures                       │
│  ├─ IOCTL Codes                             │
│  └─ Compile-time Flags                      │
└─────────────────────────────────────────────┘
```

### Component Responsibilities

#### **main.cpp**
- Driver lifecycle management (load/unload)
- IRP dispatch registration
- Device object creation
- Symbolic link management

#### **memory.h / memory.cpp**
- Safe memory read/write primitives
- Process address space operations
- Parameter validation
- Error handling wrappers

#### **hook.h / hook.cpp**
- Function hook installation/removal
- Detour management
- Hook validation and safety checks
- Restore original function logic

#### **definitions.h**
- Shared data structures
- IOCTL command codes
- Configuration macros
- Version information

---

## 📦 Prerequisites

### Required Software

| Component | Version | Download |
|-----------|---------|----------|
| **Windows** | 10/11 x64 | [Microsoft](https://www.microsoft.com/windows) |
| **Visual Studio** | 2019/2022 | [Download](https://visualstudio.microsoft.com/) |
| **Windows SDK** | Latest | Included with VS |
| **Windows Driver Kit (WDK)** | Latest | [Download](https://docs.microsoft.com/en-us/windows-hardware/drivers/download-the-wdk) |

### Visual Studio Workloads

During Visual Studio installation, select:
- ✅ Desktop development with C++
- ✅ Windows Driver Kit (install separately after VS)

### Test Machine Setup

For safe testing, use:
- 🖥️ **Virtual Machine** (VMware, Hyper-V, VirtualBox)
- 🔌 **Kernel Debugging** (WinDbg over network or serial)
- 📸 **VM Snapshots** (before each driver load)

---

## 💾 Installation

### Quick Start

```bash
# Clone the repository
git clone https://github.com/th3drata/Kernel-Driver.git
cd KDriver

# Open solution in Visual Studio
start KDriver.sln
```

---

## 🔨 Building

### Build Steps

1. **Open Solution**
   ```
   Open KDriver.sln in Visual Studio
   ```

2. **Select Configuration**
   - Configuration: `Debug` or `Release`
   - Platform: `x64`

3. **Build**
   - Menu: `Build → Build Solution`
   - Shortcut: `Ctrl+Shift+B`

### Build Output

After successful build, find outputs in:
```
x64/Debug/   or   x64/Release/
├── KDriver.sys       # Driver binary
├── KDriver.pdb       # Debug symbols
└── KDriver.inf       # Installation metadata
```

---

## 🚀 Deployment

### ⚠️ Warning
Only deploy on **non-production test machines** you fully control!

### Option A: Test Signing (Recommended for Development)

```powershell
# 1. Enable test signing (requires admin)
bcdedit /set testsigning on

# 2. Reboot
shutdown /r /t 0

# 3. Create service
sc create KDriver type= kernel binPath= C:\Path\To\KDriver.sys

# 4. Start driver
sc start KDriver

# 5. Verify status
sc query KDriver
```

### Option B: KDMapper (Research/Testing)

For unsigned driver loading without test signing:

```powershell
# Download KDMapper from trusted source
# Run as administrator
kdmapper.exe KDriver.sys
```

**Note**: This method bypasses driver signature enforcement and may trigger security software.

### Uninstallation

```powershell
# Stop driver
sc stop KDriver

# Delete service
sc delete KDriver

# Disable test signing (optional)
bcdedit /set testsigning off
```

---

## 📘 Usage

### User-Mode Communication

To communicate with the driver from user-mode:

1. **Define IOCTLs** in `definitions.h`
2. **Implement handlers** in `main.cpp` dispatch routines
3. **Create user-mode client** using `CreateFile` + `DeviceIoControl`

Example IOCTL definition:
```cpp
#define IOCTL_KDRIVER_READ_MEMORY \
    CTL_CODE(FILE_DEVICE_UNKNOWN, 0x800, METHOD_BUFFERED, FILE_ANY_ACCESS)
```

### Kernel Debugging

Connect WinDbg to your test VM:
```
# On host machine
windbg -k net:port=50000,key=1.2.3.4

# On VM (as admin, before booting)
bcdedit /debug on
bcdedit /dbgsettings net hostip:YOUR_IP port:50000 key:1.2.3.4
```

---

## 📂 Project Structure

```
KDriver/
├── 📄 KDriver.sln                    # Visual Studio solution
├── 📄 KDriver.vcxproj                # Project file
├── 📄 KDriver.vcxproj.filters        # VS filters
├── 📄 KDriver.inf                    # Driver installation metadata
├── 📄 README.md                      # This file
├── 📁 Source Files
│   ├── main.cpp                      # Driver entry point
│   ├── memory.cpp                    # Memory operations
│   └── hook.cpp                      # Hook implementation
├── 📁 Header Files
│   ├── definitions.h                 # Shared definitions
│   ├── memory.h                      # Memory interface
│   └── hook.h                        # Hook interface
└── 📁 x64/                           # Build outputs
    ├── Debug/
    │   ├── KDriver.sys
    │   └── KDriver.pdb
    └── Release/
        ├── KDriver.sys
        └── KDriver.pdb
```

---

## 🛠️ Development

### Best Practices

#### 🔒 Safety First
- Always test in isolated VMs
- Take snapshots before loading drivers
- Keep kernel debugger attached
- Never deploy to production systems

#### 📝 Code Guidelines
- Validate all user inputs
- Use SAL annotations (`_In_`, `_Out_`, etc.)
- Check return values from kernel APIs
- Avoid long operations in critical paths
- Document hook modifications thoroughly

#### 🐛 Debugging Tips
```cpp
// Use DbgPrint for kernel logging
DbgPrint("[KDriver] Operation completed: %d\n", status);

// Enable verbose logging in Debug builds
#ifdef DEBUG
    #define LOG(fmt, ...) DbgPrint("[KDriver] " fmt "\n", __VA_ARGS__)
#else
    #define LOG(fmt, ...)
#endif
```

### Adding New Features

1. **Define structures** in `definitions.h`
2. **Implement logic** in appropriate module
3. **Add dispatch handlers** in `main.cpp`
4. **Test thoroughly** in VM environment
5. **Document behavior** in comments

---

## 🔐 Security Considerations

### ⚠️ Critical Warnings

| Risk | Mitigation |
|------|------------|
| **System Instability** | Test only in VMs with snapshots |
| **Security Software** | May flag driver loading as malicious |
| **BSOD / Crashes** | Keep debugger attached, validate inputs |
| **Data Loss** | Never test on systems with important data |
| **Legal Issues** | Use only for authorized research/education |

### Safe Usage Guidelines

1. ✅ Use test signing on development machines
2. ✅ Validate all parameters from user-mode
3. ✅ Implement proper error handling
4. ✅ Use try-except blocks for memory access
5. ✅ Clean up resources on unload
6. ❌ Never deploy to production
7. ❌ Don't hook critical system functions
8. ❌ Avoid operations that can't be rolled back

---

## ❓ FAQ

<details>
<summary><b>Q: Can I use this in production?</b></summary>
<br>
<b>No.</b> KDriver is for educational and research purposes only. Production kernel drivers require extensive testing, certification, and code signing.
</details>

<details>
<summary><b>Q: Which Windows versions are supported?</b></summary>
<br>
KDriver targets modern Windows 10/11 x64. Specific compatibility depends on your WDK version and any custom hooks you implement.
</details>

<details>
<summary><b>Q: Do I need to sign the driver?</b></summary>
<br>
Yes. Use test signing for development or KDMapper for research. Production deployment requires a valid EV certificate and Microsoft attestation signing.
</details>

<details>
<summary><b>Q: Can I use this on Windows 11?</b></summary>
<br>
Yes, but Windows 11 has stricter driver requirements. Ensure Secure Boot and VBS settings are compatible with your test setup.
</details>

<details>
<summary><b>Q: How do I debug a BSOD?</b></summary>
<br>
Use WinDbg with kernel debugging enabled. Analyze crash dumps with <code>!analyze -v</code> and check the call stack.
</details>

<details>
<summary><b>Q: Is user-mode communication included?</b></summary>
<br>
No. You need to implement your own IOCTLs and user-mode client. The driver provides the foundation.
</details>

---

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Contribution Areas

- 🐛 Bug fixes
- 📝 Documentation improvements
- ✨ New features (with tests)
- 🔍 Code review and auditing
- 🎓 Educational examples

---

## 📄 License

This project is licensed under the **MIT License** - see below for details:

```
MIT License

Copyright (c) 2025 KDriver Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## ⚠️ Disclaimer

**READ CAREFULLY BEFORE USING THIS SOFTWARE:**

- ⚠️ This driver operates at **kernel level** with **unrestricted system access**
- 🔴 Improper use can cause **system crashes, data loss, or permanent damage**
- 🎓 Intended for **educational and research purposes only**
- 🚫 **Not suitable for production environments**
- ⚖️ You are **solely responsible** for compliance with local laws and regulations
- 🛡️ The authors assume **no liability** for damages caused by using this software
- 📚 Use only in **authorized, isolated test environments**

**By using KDriver, you acknowledge understanding these risks and agree to use it responsibly.**

---

<div align="center">

### 🌟 Star this repo if you find it useful!

**Built with ❤️ for the security research community**

[Report Bug](https://github.com/th3drata/Kernel-Driver/issues) · [Request Feature](https://github.com/th3drata/Kernel-Driver/issues) · [Documentation](https://github.com/th3drata/Kernel-Driver/wiki)

</div>
