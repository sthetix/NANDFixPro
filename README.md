# NAND Fix Pro


[![Version](https://img.shields.io/badge/version-2.3-blue.svg)](https://github.com/sthetix/NANDFixPro/releases)
[![Platform](https://img.shields.io/badge/platform-Windows-0078D4.svg)](https://www.microsoft.com/windows/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

<p align="center">
  <img src="https://raw.githubusercontent.com/sthetix/NANDFixPro/main/images/preview.jpg" alt="Preview Image" width="75%">
</p>

A comprehensive GUI tool for Nintendo Switch NAND repair, designed to simplify complex recovery processes into a user-friendly, step-by-step interface.


NAND Fix Pro provides three distinct levels of repair, from a simple system file restore that preserves your data to a complete NAND reconstruction for catastrophic failures. It automates the use of essential community tools like **EmmcHaccGen**, **NxNandManager**, and **OSFMount** behind a clean and safe interface.

---

## Features

-   **Three-Tiered Repair System**: Choose the right level of repair for your specific issue.
-   **Live and Offline Modes**: Work directly with a Hekate-mounted eMMC/emuMMC device, or repair a NAND backup file without writing to hardware.
-   **User-Friendly GUI**: A modern, dark-themed interface that guides you through every step. No command-line knowledge required.
-   **Automatic Path Detection**: The application automatically finds its required tools from the `lib` folder.
-   **Safety First**: Critical actions require user confirmation, Hekate target validation, archive integrity checks, and required file preflights before repair operations.
-   **Erista & Mariko Support**: Automatically detects the console model from its PRODINFO and applies the correct settings for boot file generation.
-   **Split NAND and emuMMC Support**: Detects Hekate split NAND backups and emuMMC SD file layouts, then joins and re-splits output when needed.
-   **Automated Dependency Management**: The included launcher ensures Python and required libraries are installed automatically.
-   **Built-in Admin Elevation**: Automatically prompts for administrator rights, which are required for direct eMMC access.
-   **Robust Logging**: All operations are logged to the screen and saved in an `error_log.txt` on crash, making troubleshooting easy.
-   **Disk Space Validation**: Automatically checks for sufficient free space (60GB) before starting any repair process.

---

## The Three Levels of Repair

####  Level 1: System Restore (Data Preserved)
* **What it does**: Dumps the SYSTEM partition from your Switch, replaces the core OS files with clean ones generated from your firmware, and flashes it back.
* **When to use it**: Ideal for fixing boot issues caused by a failed system update, a bad custom theme, or general software corruption where your save games and installed titles are still intact.
* **Outcome**: Your console's OS is repaired, and all your user data (saves, games, profiles) is preserved.

####  Level 2: Full Rebuild (User Data Erased)
* **What it does**: Uses your Switch's unique PRODINFO to rebuild the entire NAND using clean, pre-packaged donor partitions.
* **When to use it**: Use this when multiple partitions (not just SYSTEM) are corrupt, but your console's PRODINFO is still readable and intact.
* **Outcome**: Your console is restored to a factory-like state. **All user data will be erased.**
* **New Advanced Option** - Fix USER Partition: This level now includes a separate, advanced function to repair only a corrupt USER partition without touching the rest of the OS. This is a faster alternative if your problem is isolated to user data corruption (e.g., being unable to boot past the setup screen). Like the full rebuild, this process will also erase all user data.

####  Level 3: Complete Recovery (Last Resort)
* **What it does**: Reconstructs a complete NAND from scratch using a **donor PRODINFO file** and a pre-built NAND skeleton. It automatically detects the eMMC size (32GB/64GB) to use the correct template.
* **When to use it**: This is the final option for a completely dead or lost NAND where even the original PRODINFO is gone or corrupt.
* **Outcome**: A brand new, functional NAND is written to the eMMC. **This is a total overwrite.**

---

## Supported Workflows

-   **Live eMMC repair**: Connect the Switch through Hekate `eMMC RAW GPP` with read-only disabled. The app detects the Hekate hardware ID before writing.
-   **Live emuMMC repair**: Connect a Hekate-exposed emuMMC raw GPP target when working with SD-based emuMMC.
-   **Offline NAND repair**: Select a `RAWNAND.bin` backup and `prod.keys` to repair a file instead of a physical device.
-   **Split NAND repair**: Select `rawnand.bin.00` or emuMMC-style `00` and the app joins the parts for processing. When needed, it prepares re-split output for SD use.
-   **BOOT file handoff**: After generating `BOOT0` and `BOOT1`, use the in-app `Copy BOOT to SD` flow to place them into the detected Hekate restore folder.

---

## Tools & Advanced Options

-   **Get Keys from SD**: Detects a Hekate-mounted SD card and imports `prod.keys`; Level 3 can also import donor `PRODINFO` when available.
-   **Advanced USER Partition Fix**: Repairs only the USER partition from clean donor data. This still erases user data, but avoids a full rebuild when the damage is isolated.
-   **PRODINFO Editor**: Edit serial, WiFi region, and color fields, then recalculate checksums and verify PRODINFO integrity.
-   **Console Type Override**: Manually choose Erista or Mariko if automatic PRODINFO-based model detection is not appropriate.
-   **Settings**: Override tool paths, the NAND partitions folder, temporary directory, and offline mode behavior.
-   **Log Controls**: Save logs for troubleshooting, clear the current log, or reset the app state for a new operation.

---

## Prerequisites

Before using this tool, you will need to provide:

1.  **Your Keys File**: A `prod.keys` file dumped from your own console.
2.  **Firmware Files**: A folder containing the extracted firmware for the version you wish to install.
3.  **(Level 3 Only)**: A decrypted donor `PRODINFO` file if your original is lost.

All other required tools and donor partitions are included in the release package.

---

## Installation & Windows Defender Notice

### Download and Extract

1. Download the `NAND-Fix-Pro-v2.3.exe` file from the [latest release page](https://github.com/sthetix/NANDFixPro/releases).
2. **Important**: The downloaded `.exe` file is actually a **7-Zip self-extracting archive**. When you run it or extract it, Windows Defender may flag the `NANDFixPro.exe` launcher as a potential threat.

### False Positive Warning

**This is a known false positive detection.** The `NANDFixPro.exe` launcher is flagged because it:
- Automatically elevates to administrator privileges
- Installs a portable Python environment
- Executes system-level operations required for eMMC access

The tool is completely safe and open-source. You can review the source code in this repository at any time.

### How to Exclude from Windows Defender

To use NAND Fix Pro, you'll need to add an exclusion to Windows Defender:

#### Method 1: Add Exclusion Before Extracting (Recommended)

1. Open **Windows Security** by searching for it in the Start menu
2. Click on **Virus & threat protection**
3. Under "Virus & threat protection settings", click **Manage settings**
4. Scroll down to **Exclusions** and click **Add or remove exclusions**
5. Click **Add an exclusion** and select **Folder**
6. Navigate to and select the folder where you plan to extract NAND Fix Pro
7. Now extract the `NAND-Fix-Pro-v2.3.exe` to that folder

#### Method 2: Restore Quarantined File

If Windows Defender already quarantined the file:

1. Open **Windows Security**
2. Go to **Virus & threat protection**
3. Click on **Protection history**
4. Find the `NANDFixPro.exe` entry and click on it
5. Click **Actions** → **Restore**
6. Then follow **Method 1** above to add the folder to exclusions to prevent future detection

#### Method 3: Using Windows Defender via Settings

1. Press `Windows + I` to open Settings
2. Go to **Privacy & Security** → **Windows Security**
3. Click **Virus & threat protection**
4. Under "Virus & threat protection settings", click **Manage settings**
5. Scroll to **Exclusions** → **Add or remove exclusions**
6. Click **Add an exclusion** → **Folder**
7. Select your NAND Fix Pro installation folder

---

## Usage

Getting started is designed to be as simple as possible.

### 1. Prepare Your Console and Files
* **CRITICAL: Create a full NAND backup using Hekate before doing anything else.** This is your essential safety net and must be done regardless of which repair level you plan to use. Without this backup, you cannot recover if something goes wrong.
* Get your unique console files. Use **Lockpick RCM** to dump your `prod.keys` file. **You must copy this file from your Switch's SD card to your computer**, as the tool needs it for all operations.
* If you're performing a **Level 3: Complete Recovery**, you will also need a decrypted **donor** `PRODINFO` file. If you used the `prodinfo_gen` tool to create one, **ensure you copy this file to your computer as well**.
* Prepare a USB cable to connect your Switch to your computer. **Use a USB 2.0 port** — connecting via USB 3.0 can cause instability and booting issues during eMMC access.

### 2. Connect and Configure with Hekate
* Boot your Switch into **Hekate**.
* Navigate to the **USB Tools** menu.
* Ensure the **"Read-only" option is disabled**. This will allow the tool to write to your eMMC.
* Select **eMMC RAW GPP**. This makes your Switch's eMMC accessible to your computer.

### 3. Run the Tool
* Download the `NAND-Fix-Pro-v2.3.exe` file from the [latest release page](https://github.com/sthetix/NANDFixPro/releases).
* Run or extract the self-extracting archive to a folder on your computer.
* Simply **double-click the `NANDFixPro.exe`** file.

The launcher will automatically perform a one-time setup:
- It will install the bundled Python runtime if needed.
- It will install the necessary Python dependencies (`wmi`, `psutil`, `pywin32`).
- It will then start the NAND Fix Pro application for you.

### 4. Flash the Generated Boot Files
* After the tool generates the new `boot0` and `boot1` files, you must flash them to your console.
* Copy the generated `boot0` and `boot1` files into the `backup/<your-emmc-id>/restore` folder on your Switch's SD card. The `<your-emmc-id>` is a unique alphanumeric ID based on your eMMC chip, for example, `e4ff5e48`.
* In Hekate, go to the **Tools** section. Select **Restore eMMC**, then choose **eMMC BOOT0 & BOOT1** to flash these files back to your console.

---

## File Structure

The tool relies on a specific folder structure to function correctly. Ensure your extracted folder looks like this:

```
NAND-Fix-Pro/
│
├── NANDFixPro.exe
├── nandfixpro.py
├── Installers/
│   ├── osfmount.exe
│   ├── python-3.13.7-amd64.exe
│   └── windowsdesktop-runtime-8.0.20-win-x64.exe
│
└───lib/
    ├── 7z/
    │   └── 7z.exe
    ├── EmmcHaccGen/
    │   └── EmmcHaccGen.exe
    ├── NxNandManager/
    │   └── NxNandManager.exe
    │
    └───NAND/
        ├── donor32.7z
        ├── donor64.7z
        ├── PRODINFOF.7z
        ├── SAFE.7z
        ├── SYSTEM.7z
        ├── USER-32.7z
        └── USER-64.7z
```

---

## ⚠️ Disclaimer

This tool interacts directly with your console's internal memory (eMMC). While it includes safety measures, any interruption during a write process (e.g., power loss, cable disconnection) or use of incorrect files can lead to a **permanent brick**.

-   **You are using this tool at your own risk.**
-   Always make a full NAND backup with Hekate before attempting any repairs.
-   **Always use a USB 2.0 port.** Using USB 3.0 can cause instability and booting issues during eMMC operations.
-   The author is not responsible for any damage to your console.

---

## Acknowledgements

This application is a graphical frontend that would not be possible without the foundational work of these incredible tools and the teams behind them:

-   **[EmmcHaccGen](https://github.com/suchmememanymuch/EmmcHaccGen)** by suchmememanymuch
-   **[NxNandManager](https://github.com/eliboa/NxNandManager)** by eliboa
-   **[OSFMount](https://www.osforensics.com/tools/mount-disk-images.html)** by PassMark Software
-   **[7-Zip](https://www.7-zip.org/)** by Igor Pavlov

---

### Support My Work

If you find this project useful, please consider supporting me by buying me a coffee!

<a href="https://www.buymeacoffee.com/sthetixofficial" target="_blank">
  <img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" >
</a>
