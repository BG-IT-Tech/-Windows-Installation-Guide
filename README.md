# Windows Installation Guide

This repository provides step-by-step instructions and troubleshooting tips for installing Windows 10 and Windows 11 on a PC.

## 1. System Requirements

Before installing or upgrading, make sure your PC meets the minimum requirements for Windows 11:

- 1 GHz or faster 64-bit CPU with 2 or more cores.
- 4 GB RAM (8 GB recommended).
- 64 GB or more storage.
- UEFI firmware with Secure Boot.
- TPM 2.0 enabled.
- DirectX 12 compatible graphics with WDDM 2.x driver.

Use Microsoft’s official tool to quickly check compatibility:

[Download PC Health Check](https://aka.ms/GetPCHealthCheckApp)

## 2. Installation Options

You can install Windows in several ways:

- **Upgrade in-place** from an existing Windows 10 installation using Windows Update or the Installation Assistant.
- **Clean install** using a bootable USB created with the Media Creation Tool or an ISO.
- **Reinstall / Reset this PC** from within Windows if the system is already running but unstable.

For new systems or when changing drives, a clean install is usually the most reliable option.

## 3. Prepare Installation Media

1. Download the Windows 10 or Windows 11 ISO or use the Media Creation Tool from Microsoft.
2. Create a bootable USB (at least 8 GB) with tools like the Media Creation Tool or Rufus.
3. Safely eject the USB and plug it into the target PC.

Make sure the PC is set to boot from USB in BIOS/UEFI before continuing.

## 4. Disk and Partition Setup

During setup, you will be asked where to install Windows:

- For a **clean install**, delete existing partitions on the system drive (after backing up data) until you see “Unallocated space”, then click **Next**.
- On UEFI systems, Windows requires a **GPT** disk; if the disk is MBR, you may need to convert it using `diskpart` (`clean` + `convert gpt`). This will erase the disk.
- Windows will automatically create the necessary system and recovery partitions when installing to unallocated space.

Common error: “Windows cannot be installed to this disk. The selected disk has an MBR partition table…” usually means you booted in UEFI mode but the disk is MBR.

## 5. Common Installation Issues

Some frequent problems and quick checks:

- **Partition errors** (cannot install or delete partitions): use `diskpart` to clean and convert the disk to GPT, or rebuild partitions from unallocated space.
- **“Windows installation has failed”**: check for corrupted USB media, disconnect unnecessary USB devices, and ensure enough free space (Windows 10 ~20 GB, Windows 11 ~64 GB minimum).
- **Driver or device issues**: after install, check Device Manager for exclamation marks and install missing drivers from the manufacturer’s site.

If the installer will not complete, rebuilding Boot Configuration Data with `bootrec /fixmbr`, `bootrec /fixboot`, `bootrec /scanos`, and `bootrec /rebuildbcd` from the setup Command Prompt can fix some boot issues.

## 6. Post-Install Checklist

After Windows finishes installing:

- Install chipset, graphics, network, and storage drivers from your motherboard or OEM support page.
- Run **Windows Update** until there are no important updates left.
- Enable device security features like Secure Boot and BitLocker if supported.
- Install your preferred browser, security tools, and essential apps.

Creating a restore point or system image after you finish setup makes it easier to recover later if something goes wrong.

## 7. OEM Driver, GPU, and BIOS Updates

Always download drivers, firmware, and BIOS updates directly from the official manufacturer websites.

### Major PC & Laptop OEMs

- **Dell** – Drivers, firmware, and SupportAssist:  
  - https://www.dell.com/support/home  
  - https://www.dell.com/support/home?app=drivers

- **HP** – Drivers and downloads:  
  - https://support.hp.com

- **Lenovo** – Drivers, warranty, and troubleshooting:  
  - https://support.lenovo.com

### Motherboard & System Board Vendors

- **MSI** – Motherboard drivers, utilities, and BIOS:  
  - https://www.msi.com/support

- **ASUS / ROG** – Drivers, utilities, BIOS, and tools:  
  - Main support portal: https://www.asus.com/support  
  - ROG support: https://rog.asus.com/support

- **GIGABYTE** – Motherboard support and downloads:  
  - https://www.gigabyte.com/Support

### GPU Drivers (Graphics Cards)

- **NVIDIA GeForce** – Game Ready and Studio drivers:  
  - https://www.nvidia.com/Download/index.aspx

- **AMD Radeon** – Adrenalin drivers for GPUs:  
  - https://www.amd.com/en/support

- **Intel Graphics** – Integrated and Arc drivers:  
  - https://www.intel.com/content/www/us/en/download-center/home.html

> Tip: Always match drivers and BIOS to your exact model number and operating system version, and avoid third‑party “driver updater” tools when possible.

## 8. Health Checks and Compatibility Tools

Before upgrading from Windows 10 to Windows 11, you should verify that your current system is compatible and generally healthy.

### Windows 11 Compatibility Checks

- **PC Health Check (Official Microsoft Tool)**  
  Microsoft’s PC Health Check app verifies Windows 11 requirements like TPM 2.0, Secure Boot, CPU generation, RAM, and storage, and tells you if your device is eligible.  
  - Download: [Download PC Health Check](https://aka.ms/GetPCHealthCheckApp)  
  - Usage: After installation, search for **PC Health Check** in the Start menu and click **Check now** under the Windows 11 section to run the compatibility test.

- **WhyNotWin11 (Community Tool)**  
  WhyNotWin11 is a third‑party utility that provides a detailed breakdown of which specific Windows 11 requirements your PC does or does not meet (CPU, TPM, Secure Boot, etc.).  
  - Site: https://whynotwin.en.softonic.com  
  - Note: Always download from trusted sources and review any executable before running it.

- **Windows 11 Requirements Check Tool (Win11RCT)**  
  Win11RCT is a free tool that checks Windows 11 requirements and also reports on gaming‑related features like Auto HDR and DirectStorage support.  
  - GitHub: https://github.com/ByteJammer/Win11RCT

### General PC Health

Before a major upgrade or clean install, it’s a good idea to:

- Run **Windows Update** and install all important updates.
- Check **Storage** for errors using `chkdsk` and ensure you have enough free space.
- Use **Windows Security** (or your AV) to scan for malware.
- Back up important files to an external drive or cloud storage.

These steps help reduce the chance of issues during installation and make recovery easier if something goes wrong.

## 9. PowerShell Troubleshooting Commands

These PowerShell commands can help diagnose and repair common Windows issues before or after installation or upgrade.  
Run PowerShell **as Administrator** for all of these.

### 9.1 Check Disk Health and File System

```powershell
# List all disks and basic info
Get-Disk

# Check file system and fix errors on C: (may require reboot)
chkdsk C: /f /r
```

### 9.2 System File and Component Repair

```powershell
# Scan for corrupted system files
sfc /scannow

# If SFC fails, repair component store first:
DISM /Online /Cleanup-Image /CheckHealth
DISM /Online /Cleanup-Image /ScanHealth
DISM /Online /Cleanup-Image /RestoreHealth

# Run SFC again after DISM completes
sfc /scannow
```

### 9.3 Check Windows Update and Pending Reboot

```powershell
# Check if a reboot is pending (common cause of failed installs)
Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing\RebootPending" -ErrorAction SilentlyContinue

# Reset Windows Update components (simplified)
net stop wuauserv
net stop bits
net stop cryptsvc

Rename-Item "C:\Windows\SoftwareDistribution" "SoftwareDistribution.old" -ErrorAction SilentlyContinue
Rename-Item "C:\Windows\System32\catroot2" "catroot2.old" -ErrorAction SilentlyContinue

net start wuauserv
net start bits
net start cryptsvc
```

### 9.4 Boot and Partition Information (UEFI / GPT Checks)

```powershell
# Show partition style (GPT vs MBR) for all disks
Get-Disk | Select-Object Number, FriendlyName, PartitionStyle, OperationalStatus

# Show volumes and drive letters
Get-Volume | Select-Object DriveLetter, FileSystemLabel, FileSystem, SizeRemaining, Size
```

### 9.5 Quick Hardware / Requirement Checks

```powershell
# Check total RAM in GB
(Get-CimInstance Win32_ComputerSystem).TotalPhysicalMemory / 1GB

# Check CPU info
Get-CimInstance Win32_Processor | Select-Object Name, NumberOfCores, NumberOfLogicalProcessors, MaxClockSpeed

# Check TPM status
Get-Tpm

# Check Secure Boot status (returns True/False if supported)
Confirm-SecureBootUEFI
```

> Tip: Run the hardware requirement checks before attempting a Windows 11 upgrade so you know if the issue is hardware/firmware or just a software problem.
