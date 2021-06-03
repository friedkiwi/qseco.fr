---
title: "Demystifying AS/400 DASD"
date: 2019-03-11
draft: false
---

There seem to be quite a few misconceptions about AS/400 hard drives, commonly referred to as DASD (directly attached storage device). It is commonly known that only IBM-supplied DASD work, and typically only the FRUs listed. The big question though is “why is this the case?".

Spinning disks die eventually. At the moment, generic SCSI disks are fairly commonly available, however, the FRUs to keep old AS/400 alive for hobbyist and archival purposes are becoming more and more scarce, with the supply eventually drying up.

# Distinction between models

Some of the information here is model-dependant, so a distinction is made between the following systems:

# CISC/IMPI-based AS/400s

These are the old pre-PowerPC-based AS/400s. They will typically run OS revisions up and until V3R2. They operate on a complex interaction of microcode on top of a more generic architecture and several subsystems, not unlike the channel architecture on it’s larger brethren. A more thorough description on how these machines work is a future project. Example model numbers are 9401-P03 or 9401-P02

# Early PowerPC-based AS/400

PowerPC-based AS/400s were gradually introduced to replace the earlier CISC/IMPI-based models. Typically, the high end models were replaced first. PowerPC brought a massive speed boost to the AS/400 product line, however, it’s real potential was only realised later. Early OS revisions (e.g. V3R6) were direct ports of the CISC/IMPI-based code and were quite slow. A thorough rewrite of the kernel in V4 addressed most of these issues. Early PPC-based AS/400s would typically have been able to run V3R6 or later. Example models are e.g. the 9403-53X. I am still not convinced this is due to OS revision or hardware revision.

# PowerPC-based AS/400

These are the models typically supplied with OS/400 V4 or later, all the way up until the Power5-based models (P5-based models are a special case though, but this is not relevant for DASD). Example models are 9406-170 or 9406-250

# 520, 522 and 512 bytes per sector

It is currently commonly known that the AS/400 uses an odd sector size. However, the information that can be found online is quite contradictory - some sources quote 520, some 522. Some sources quote that 522 is only used in systems with a RAID controller. Based on my observations and tests this is not true - the difference does not lie in the use of a RAID controller or not, but in the type of AS/400. PowerPC-based AS/400s will typically have 522 bytes per sector, and CISC/IMPI-based AS/400s will have 520 bytes per sector. There are a few edge cases though, especially in early PowerPC-based AS/400s - if the DASD have been migrated from a CISC/IMPI-based machine they would not typically be re-formatted on those early machines on the OS/revisions at the time.

# So, TL;DR:

- Standard SCSI disks: 512 bytes per sector
- CISC/IMPI-based AS/400: 520 bytes per sector
- Early PowerPC-based AS/400: 520 or 522 bytes per sector
- PowerPC-based AS/400s: 522 bytes per sector.

# Custom VPD

This would be obvious to anyone with any idea on how SCSI disks work, but the vital product data on AS/400 DASD contains entries verified by the firmware. This VPD typically contains serial number, disk type (e.g. 6713) and serial number. When installing SLIC on a PowerPC-based AS/400, this is verified to contain the expected data. Another quirk to note is that the OS does not (always) rely on SCSI READ CAPACITY commands to identify the size of the DASD. As a result, putting the VPD of a 6714 (~18GB) over a 70GB volume does not result in the drive being detected as a 70GB drive.

# Magic commands

Another common misconceptions is that IBM uses proprietary commands to interact with the drive. This is actually not entirely true. There are however a few implementation based quirks that the OS relies on when dealing with drives.

# SCSI FORMAT implementation

The SCSI FORMAT implementation needs to be accurate. The OS relies on the disks being formatted using a SCSI FORMAT command. OS/400 technically doesn’t have a file system (more about that will be documented in another write-up), and the way it deals with data storage requires hard drives to be completely empty upon first use. If the SCSI FORMAT implementation doesn’t fill the drive with the specified pattern (not all drives implement this properly), the OS installer will crash/hang because it tries to read a few sectors to verify if formatting succeeded (it does not just trust the status code returned by the SCSI FORMAT command).

# SKIP READ/SKIP WRITE

To optimise disk read and writes, OS/400 heavily relies on SKIP READ and SKIP WRITE. These commands need to be implemented and need to be implemented properly. More information about this can be found on http://ps-2.kev009.com/rs6000/manuals/SAN/ESS/2105_Model_ExxFxx/ESS_SCSI_Command_Reference_ExxFxx_SC26-7297-01.PDF. The reason why OS/400 relies on this is another story, closely related to the way the file system (or lack thereof) works.

# Conclusion

That’s it really. The only things which makes an AS/400 DASD special are:

- Custom VPD
- 520/522 byte sector size based on model
- SCSI FORMAT being implemented properly
- SKIP READ and SKIP WRITE being implemented properly

I hope this helps some people troubleshooting their failing hard drives.
