# OmegaFile — Smarter File Operations for Windows

**Copy faster. Delete anything. Take control.**

[![YouTube](https://img.shields.io/badge/YouTube-Demo-red?logo=youtube)](https://www.youtube.com/channel/UCksGhlnuOhirbMzMG3yYJmg)
[![Discord](https://img.shields.io/badge/Discord-Join-5865F2?logo=discord)](https://discord.com/invite/jRnaeTJ)
[![GitHub](https://img.shields.io/badge/GitHub-OmegaFile-181717?logo=github)](https://github.com/tetteykn/OmegaFile)

---

## The Problem Windows Doesn't Tell You About

You have probably experienced this: you copy a folder, walk away, come back 20 minutes later and it still isn't done — even though the folder is only a few gigabytes. Meanwhile you copied a single 20 GB video file last week and it finished in under two minutes. Why?

The answer has nothing to do with total size or hard drive speed. It has everything to do with the **number of files**.

When Windows copies files, it processes each one individually — opening it, reading it, writing it, closing it, then moving on to the next. For a single large file this is fine. But when your folder contains thousands or tens of thousands of small files, Windows performs this full sequence for every single one. Each file requires multiple calls into the operating system kernel, plus a separate disk operation to write the file's metadata. With 10,000 files that is potentially 80,000–100,000 individual system operations before a single byte of your actual data has finished moving.

This is why a folder with 50,000 small files totalling just 1 GB can take **over an hour** in Windows — while a single 20 GB video copies in minutes. The bottleneck is not your disk. It is the way Windows handles each file one at a time.

**OmegaFile solves this completely.**

---

## Benchmark: Real-World Test

We ran a direct comparison copying a real folder:

| | Windows Copy | OmegaFile |
|---|---|---|
| **Folder contents** | 3,947 files | 3,947 files |
| **Total size** | 81.1 MB | 81.1 MB |
| **Time to complete** | **26 seconds** | **5 seconds** |
| **Speed improvement** | — | **5.2× faster** |

Same machine, same source, same destination, same files. OmegaFile finished in 5 seconds. Windows took 26 seconds.

> 📹 **YouTube Demo:** [Watch on YouTube](https://youtu.be/o9wJGV7FDTw)

This advantage only grows as file counts increase. At 10,000 files the gap widens dramatically. At 100,000 files, Windows may take hours while OmegaFile completes in minutes.

---

## How OmegaFile Copies So Much Faster

OmegaFile uses an **adaptive copy engine** that automatically selects the best strategy based on what you are copying. You never choose — it detects and decides instantly.

### Strategy A — Single File (CopyFileEx)
When you copy one file, OmegaFile uses Windows' native `CopyFileEx` API directly. This is the most efficient path possible for a single file and provides a real-time progress callback so you always see exactly how far along the copy is.

### Strategy B — Small Collections (Parallel Async, under 1,000 files)
For folders with under 1,000 files, OmegaFile runs 8 parallel copy workers simultaneously. Instead of waiting for file #1 to finish before starting file #2, all 8 workers copy different files at the same time. On a modern SSD, this alone delivers a significant speedup.

### Strategy C — Large Collections (Channel/IOCP, up to 100,000 files)
This is where OmegaFile truly separates itself from Windows. For folders with thousands of files, OmegaFile uses a technique called **IOCP (I/O Completion Ports)** — the same technology used inside high-performance web servers and database engines. Instead of waiting for each disk operation to finish, OmegaFile submits up to 24 copy operations to the operating system simultaneously and processes completions as they arrive. The OS and storage hardware handle the scheduling, the disk queue stays full, and data moves continuously without gaps.

### Strategy D — Extreme Collections (Stream Archive, 100,000+ files)
For truly massive operations — hundreds of thousands of files — OmegaFile switches to a turbo stream mode. Files are processed in large batches that flow through the system as a continuous data stream rather than individual file operations. This mode bypasses most of the per-file overhead entirely and scales to millions of files without degrading.

---

## Force Delete — Remove Files Windows Refuses to Touch

Windows sometimes refuses to delete files. You see errors like *"The file is open in another program"*, *"Access is denied"*, or *"You need administrator permission"* — even when no program is visibly using the file.

OmegaFile's **Force Delete** mode handles all of these cases. It takes ownership of the file, strips all blocking attributes, closes any open handles it can reach, and removes the item. For files that are so deeply locked that even these steps cannot free them, OmegaFile schedules them for automatic deletion the next time Windows restarts — so the file will be gone on next boot, with no manual steps required.

---

## Features at a Glance

### Intelligent Adaptive Copying
OmegaFile scans your selection in under a second, counts the files, measures the total size, and automatically picks the right strategy. No settings to configure, no modes to select — it just works.

### Move Support (Cut & Paste)
OmegaFile handles file moves (cut and paste) as well as copies. When moving files between folders on the same drive, it uses the fastest available path. When moving between drives, it copies then deletes, giving you progress feedback the whole time.

### Windows Context Menu Integration
Once enabled, OmegaFile adds an **OmegaFile** submenu to your right-click menu on any file or folder with Copy, Cut, Delete, and Force Delete options. It also adds an **OmegaFile: Paste** option when you right-click inside any folder.

The context menu is automatically registered during installation and automatically removed on uninstall. Users can also toggle it at any time from the app Settings.

### Live Progress Window
Every operation shows a floating progress window in the bottom-right corner of your screen showing:
- Current file being processed
- Which strategy is active
- Number of files completed out of total
- Data transferred out of total
- Real-time transfer speed in MB/s
- A cancel button that stops the operation cleanly at any point

### Skip Empty Folders Option
When copying folder structures that contain empty subdirectories, you can choose to skip them and only transfer folders that actually contain files.

---

## Who Needs OmegaFile?

**Developers** — copying project folders, `node_modules` directories, build outputs, or source code repositories with thousands of small files. A typical web project's `node_modules` folder can contain over 100,000 files. Windows may take 15–30 minutes to copy it. OmegaFile can do it in under 2 minutes.

**Designers and content creators** — working with asset libraries, texture packs, icon sets, or font collections that contain large numbers of individual files.

**System administrators and IT professionals** — backing up workstations, migrating user profiles, or copying software deployment packages.

**Gamers** — moving game installations between drives, copying mod folders, or managing large collections of game files.

**Anyone who has ever watched a Windows copy dialog sit at "Calculating..." for five minutes** — that delay is Windows counting your files one at a time before it even starts. OmegaFile begins copying almost instantly.

---

## When Windows Copy Is Fine

OmegaFile's advantages become most visible when:
- You are copying **more than a few hundred files**
- Your source folder has **deep subfolder structures**
- You are copying **small files** (documents, code, config files, assets under 1 MB each)
- Windows is showing a copy time estimate of **several minutes or more for data under 1 GB**

For everyday simple copies — moving one document, copying a few photos — Windows copy is perfectly adequate.

---

## Uninstallation Note

Upon uninstallation, all application files and data are completely removed. The parent folder `C:\Users\username\AppData\Local\Zouaouid Tech\` may remain on your device as it is shared across other applications published under the same publisher. If OmegaFile is the only installed application from Zouaouid Tech, this folder can be safely deleted manually.

---

## Support & Contact

| | |
|---|---|
| **Email** | phonetettey@gmail.com |
| **Discord** | https://discord.com/invite/jRnaeTJ |
| **GitHub** | https://github.com/tetteykn/OmegaFile |
| **YouTube** | https://www.youtube.com/channel/UCksGhlnuOhirbMzMG3yYJmg |

---

## Built by Zouaouid Tech

OmegaFile is developed by Zouaouid Tech, an independent software studio focused on building high-performance Windows utilities. Every feature in OmegaFile exists because it solves a real problem that Windows leaves unsolved.

---

*OmegaFile — because your time is worth more than a progress bar.*
