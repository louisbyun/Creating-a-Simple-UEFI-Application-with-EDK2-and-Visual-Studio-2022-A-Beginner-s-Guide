# Creating a Simple UEFI Application with EDK2 and Visual Studio 2022: A Beginner's Guide

## Introduction

Creating a UEFI (Unified Extensible Firmware Interface) application may seem daunting at first, but with the right tools and guidance, you can get your first app up and running in no time. This tutorial will guide you on how to create a simple UEFI application that prints “Hello, World!” using EDK2 and Visual Studio 2022. It will also cover how to run your application on real hardware.

## What is UEFI?

UEFI is a specification that defines a software interface between an operating system and platform firmware. It is designed to replace the Basic Input/Output System (BIOS) firmware interface. UEFI offers a more modern, flexible, and secure interface compared to BIOS, and it is also more modular, allowing for greater flexibility and customization.

## What is EDK2?

EDK2 is an open-source development environment for UEFI applications. It is a fork of the TianoCore project and is maintained by the UEFI Forum. More information about EDK2 can be found on the [EDK2 GitHub page](https://github.com/tianocore/edk2).

## Prerequisites

- A UEFI-enabled hardware for testing with Secure Boot disabled.
- Windows 11 development environment.
- EDK2 environment set up.
- Visual Studio 2022.
- Basic knowledge of C/C++ programming language.
- Flash drive with FAT32 partition.

## Step 1: Setup Your Environment

### Install Visual Studio 2022

1. **Download and Install Visual Studio 2022**:
   - Visit the [Visual Studio download page](https://visualstudio.microsoft.com/downloads/) and download the installer.
   - During installation, select the following components:
     - Desktop development with C++
     - Latest Windows SDK

### Setup EDK2 Environment

1. **Install Git**:
   - Download and install Git from the [official website](https://git-scm.com/downloads).

2. **Install Python 3.8**:
   - Download and install Python 3.8 from the [official website](https://www.python.org/downloads/).

3. **Install NASM (Netwide Assembler)**:
   - Download NASM from the [official website](https://www.nasm.us/).
   - Set the `NASM_PREFIX` environment variable to the path where you installed NASM. For example, if you installed NASM to `C:\NASM`, you would set the `NASM_PREFIX` environment variable to `C:\NASM`.

4. **Clone the EDK2 Repository**:
   - Open Command Prompt and run the following commands:
     ```sh
     git clone https://github.com/tianocore/edk2.git
     cd edk2
     git submodule update --init --recursive
     ```
5. **Build or Install the EDK2 BaseTools**:
   - Open the Visual Studio Developer Command Prompt and navigate to the EDK2 directory. Then run the following command:
```sh
edksetup.bat Rebuild
```

## Step 2: Build and Run Your UEFI Application

1. **Set Up the Build Environment**:
   - Open the "x64 Native Tools Command Prompt for VS 2022".
   - Navigate to the EDK2 directory and set up the build environment:
     ```sh
     cd C:\edk2
     edksetup.bat
     set BASE_TOOLS_PATH=C:\edk2\BaseTools
     set EDK_TOOLS_PATH=%BASE_TOOLS_PATH%
     cd %BASE_TOOLS_PATH%
     toolsetup.bat
     nmake -f Makefile
     ```

2. **Configure Your Build Target**:
   - Set up your build target and toolchain:
     ```sh
     set ACTIVE_PLATFORM=MdeModulePkg\MdeModulePkg.dsc
     set TARGET_ARCH=X64
     set TOOL_CHAIN_TAG=VS2022
     ```

3. **Create Your UEFI Application**:
   - Create a directory for your application, e.g., `HelloWorldWithUI`.
   - Create an `.INF` file and a `.C` file for your application:

     `HelloWorldWithUI.inf`:
     ```plaintext
     [Defines]
       INF_VERSION = 1.10
       BASE_NAME = HelloWorldWithUI
       FILE_GUID = A1B2C3D4-E5F6-7890-1234-56789ABCDEF0
       MODULE_TYPE = UEFI_APPLICATION
       VERSION_STRING = 1.0
       ENTRY_POINT = UefiMain

     [Sources]
       HelloWorldWithUI.c

     [Packages]
       MdePkg/MdePkg.dec
       UefiBootServicesTableLib/Include/Library/UefiBootServicesTableLib.h

     [LibraryClasses]
       UefiApplicationEntryPoint
       UefiBootServicesTableLib
       UefiLib
       PrintLib
     ```

     `HelloWorldWithUI.c`:
     ```c
     #include <Uefi.h>
     #include <Library/UefiLib.h>
     #include <Library/UefiBootServicesTableLib.h>
     #include <Library/PrintLib.h>
     #include <Library/UefiApplicationEntryPoint.h>

     EFI_STATUS
     EFIAPI
     UefiMain (
       IN EFI_HANDLE        ImageHandle,
       IN EFI_SYSTEM_TABLE  *SystemTable
       )
     {
       EFI_STATUS Status;
       EFI_INPUT_KEY Key;

       // Clear the screen
       SystemTable->ConOut->ClearScreen(SystemTable->ConOut);

       // Print "Hello, World!"
       SystemTable->ConOut->OutputString(SystemTable->ConOut, L"Hello, World!\nPress any key to continue...\n");

       // Wait for a key press
       SystemTable->BootServices->WaitForEvent(1, &SystemTable->ConIn->WaitForKey, &Index);
       Status = SystemTable->ConIn->ReadKeyStroke(SystemTable->ConIn, &Key);

       return Status;
     }
     ```

4. **Build Your Application**:
   - Navigate to your application directory and build your application:
     ```sh
     cd \path\to\HelloWorldWithUI
     build
     ```

5. **Run Your Application**:
   - Copy the generated `.efi` file to a FAT32-formatted USB drive.
   - Boot your UEFI-enabled hardware from the USB drive and run your UEFI application.

By following these steps, you should be able to create, build, and run a simple UEFI application that prints "Hello, World!" on your screen. If you encounter any issues, make sure all paths are correct and all necessary tools are installed properly.
