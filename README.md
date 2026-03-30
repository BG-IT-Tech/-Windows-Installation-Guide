# -Windows-Installation-Guide
How to install and trouble shoot windows installation.
# Windows Installation Guide

This comprehensive guide will help you install Windows on your machine with detailed step-by-step instructions, system requirements, hardware specifications, and troubleshooting solutions.

---
## Prerequisites

Before starting the installation, use Microsoft’s official tool to verify that your PC meets the requirements for Windows 11:

[Download PC Health Check](https://aka.ms/GetPCHealthCheckApp)

## Minimum System Requirements

### PC Specifications
Before installing Windows, ensure your computer meets these minimum requirements:

| Component | Windows 10 | Windows 11 |
|-----------|-----------|-----------|
| **Processor** | 1 GHz or faster with SSE2 support | 1 GHz or faster, 2 cores minimum (Intel 8th Gen or AMD Ryzen 2000 series+) |
| **RAM** | 1 GB (32-bit) / 2 GB (64-bit) | 4 GB minimum |
| **Disk Space** | 16 GB (32-bit) / 20 GB (64-bit) | 64 GB minimum SSD |
| **Graphics** | DirectX 9 or later compatible | DirectX 12 compatible GPU |
| **Display** | 800 x 600 minimum | 720p (1280 x 720) minimum for 64-bit |
| **Firmware** | BIOS or UEFI | UEFI firmware with Secure Boot capable |
| **TPM** | Optional | TPM 2.0 required (Windows 11) |

### Recommended Specifications
For optimal performance:
- **Processor**: Multi-core processor, 2.0 GHz or faster
- **RAM**: 4 GB or more (8 GB+ recommended)
- **Storage**: SSD with at least 128 GB
- **Internet**: Broadband connection for updates

---

## Installation Media Requirements

### USB Drive Specifications
- **Capacity**: Minimum 8 GB (16 GB recommended for easier creation)
- **Speed**: USB 3.0 or higher recommended for faster creation
- **Type**: USB flash drive or external USB hard drive
- **Format**: FAT32 or NTFS (will be formatted during creation)
- **Interface**: Standard USB-A connector

**Note**: The drive will be completely wiped during the Media Creation Tool process. Backup any important data before proceeding.

### DVD Specifications
- **Media Type**: Recordable DVD-R or DVD+R
- **Capacity**: Dual-layer DVD-DL (8.5 GB) required
- **Speed**: 8x or higher recommended
- **Drive Type**: Compatible DVD-R drive with write capability
- **Software**: DVD authoring software (Windows Disc Image Burner, Nero, etc.)

**Steps to Create Windows Installation DVD:**
1. Download the Windows ISO file
2. Insert blank DVD into your DVD drive
3. Right-click the ISO file → Select "Burn disc image"
4. Choose your DVD drive
5. Click "Burn" and wait for completion

---

## Step 1: Prepare Installation Media

### Option A: Using USB Drive (Recommended)

**Download Tools:**
- [Windows Media Creation Tool](https://www.microsoft.com/en-us/software-download/windows10)
- [Windows 11 Media Creation Tool](https://www.microsoft.com/en-us/software-download/windows11)

**Detailed Instructions:**

1. **Insert USB Drive**
   - Connect your USB drive to a working computer
   - Ensure the drive has no critical files (they will be deleted)

2. **Run Media Creation Tool**
   - Download the Media Creation Tool from Microsoft's official website
   - Close all other programs before running the tool
   - Right-click the tool → Select "Run as administrator"

3. **Create Installation Media**
   - Accept the license terms
   - Select "Create installation media for another PC"
   - Choose preferred language, Windows edition, and architecture (64-bit recommended)

4. **Select USB Flash Drive**
   - Choose "USB flash drive"
   - Select your USB drive from the list (verify the correct drive!)

5. **Complete Creation Process**
   - Allow the tool to download Windows files
   - Wait for the USB drive creation to complete (15-30 minutes depending on speed)
   - Click "Finish" when complete

### Option B: Using DVD
1. Download Windows ISO file
2. Burn ISO to DVD using DVD authoring software
3. Verify the burnt disc by checking file integrity

## Disk Health Checks
To ensure optimal performance, you may want to check your disk health by:
- Running the built-in Windows tool: `chkdsk`.
- Using third-party tools, such as [CrystalDiskInfo](https://crystalmark.info/en/software/crystaldiskinfo/).

### How to Use `chkdsk`:
1. Open Command Prompt as Administrator.
2. Type `chkdsk C: /f` and press Enter to check the C drive.

## Driver Updates
Keeping your drivers up to date is crucial for system stability. You can:
- Use Windows Update to automatically check for driver updates.
- Manually download drivers from the manufacturer’s website.
  - e.g., [NVIDIA Driver Downloads](https://www.nvidia.com/download/index.aspx)


### Verification Steps
```powershell
# Check USB drive contents (PowerShell as Administrator)
Get-ChildItem -Path "E:\" -Recurse | Measure-Object

# Verify file integrity using hash
Get-FileHash "Path\To\ISO" -Algorithm SHA256
