## FATINFO.G4B v0.8.2 (20260202), by deomsh
<pre><code>Function: Get FAT-info (no switch), or switch: basic tests or return variable(s)
Use 1:    FATINFO.G4B [--mdbase=sector] [--hex] DEVICE [switch]
Use 2:    FATINFO.G4B [--mdbase=sector] [--start=sector|--skip=N|--partition=p] [--hex] FILE [switch]
Use 3:    FATINFO.G4B [--mdbase=sector] [--start=sector|--skip=N|--partition=p] [--hex] BLOCKLIST [switch]
Use 4:    FATINFO.G4B [--mdbase=m] --start=n|--skip=N [--hex] DISK [switch]
Switch:   /T|/TQ|/F|/FT|/A|/A32|/L|/L32|/B|/BC|/V
Help:     FATINFO.G4B [/?]
Remarks:  DEVICE: existing FAT-devices (always with parentheses, no path allowed!)
            Ram-disk is treated as FILE (rd) or (rd,pt_num)
            Partitioned ram-disk is NOT supported with grub4efi, use '--start=n'
          FILE: existing FAT-imagefile (always at with path! and if without extension no '+' in name-part!
          BLOCKLIST: containing FAT volume/ partition is treated as FILE
          DISK: containing FAT volume/ partition on given offset (no default!)
          --mdbase=sector to change startsecor of memory in use (default 0x3000)
                   max 512 sectors needed
          --start=sector to find bootsector in imagefile (default=0)
          --skip=address to set byte-offset of bootsector in image/ iso/ disk
          --partition=0-62 to get bootsector in blocklist (default: no partition)
                  on MBR-based partitions only (>=4 is logical partition)            
            To get FAT info, size must be a least Hidden + Reserved + FAT sectors
          --hex to view in hex bootsectors and first sector of FAT(s)
         Detected (standard) bootcodes: MS-DOS 3.3/4.0/5.0/7.1, Windows 2k/XP/
              Vista/10, FREEDOS, REACTOS, GRUB (fat); MS-DOS 7.1, Windows 2k/XP/
              Vista/10, FREEDOS, REACTOS, GRUB (fat32)
            If not detected (standard) boot files tried: IBMBIO.COM, IO.SYS,
              NTLDR, BOOTMGR, KERNEL.SYS, FREELDR.SYS and GRLDR
          grub4dos version 20170607 or higher
Switch:   Only ONE switch allowed:
          /T tests: Fit on partition, Last sector valid, Media descriptor
            Signatures, Sectors per FAT, Media descr. in FAT1, Check Dirty bit,
            FAT1/ FAT2 equal, FSInfo equal to free cluster-count (fat32 only)
          /TQ quit tests: if all tests passed variable result=1
            otherwise 0 (if no/ not all tests done variable result not exist)
          /F quiet test if DEVICE is FAT (variable result=1 or 0)
          /FT quiet test if DEVICE is FATxx
            (variable result=fat32/fat16/fat12 or no result)
          /A get quiet free space on device (variable result=n bytes)
          /A32 count quiet free space on FAT32 (variable result=n bytes)
          /L quiet last allocated cluster on device
            (variable result=clusternumber)
          /L32 same, but count on FAT32 (variable result=clusternumber)
          /B quiet number of clusters marked as bad in FAT (variable result=number of bad clusters)
          /BC quiet fat/ fat32 bootcode (variable result=Bootcode/Empty/Unknown)
          /V get quiet fat info on device. variables: FILESYS BYTEPSEC SECPCLUS
            RESERVED NUMFATS SECPFAT SECTRACK NUMHEADS ROOTENTR DEVSECT MEDIABYT
            UUID BOOTCODE + on fat32: CLUSFREE NEXTFREE (FSInfo)
Example:  FATINFO.G4B (fd0)
Example:  FATINFO.G4B (hd0,0)
Example:  FATINFO.G4B (rd)
Example:  FATINFO.G4B (rd,0)
Example:  FATINFO.G4B (rd,4)
Example:  FATINFO.G4B (fd0) /T
Example:  FATINFO.G4B (hd0,0) /T
Example:  FATINFO.G4B (rd) /TQ ;; set result
Example:  FATINFO.G4B (rd) /F ;; set result
Example:  FATINFO.G4B (hd2,0) /FT ;; set result
Example:  FATINFO.G4B (hd1,0) /V ;; set
Example:  FATINFO.G4B (hd1,0) /L ;; set result
Example:  FATINFO.G4B (hd0,0) /A32 ;; set result
Example:  FATINFO.G4B (hd0,0) /BC ;; set result
Example:  FATINFO.G4B (hd0,0)/FDDIMAGE.IMG
Example:  FATINFO.G4B --start=63 /HDDIMAGE.IMG
Example:  FATINFO.G4B --skip=0x7E00 (hd0,0)/ELTORITO.ISO
Example:  FATINFO.G4B (0xe0)0x15+720
Example:  FATINFO.G4B --partition=0 (hd0,0)0x2AE861C+0x400000
Example:  FATINFO.G4B --start=63 (hd0,0)0x2AE861C+0x400000
Example:  FATINFO.G4B --partition=0 /HDDIMAGE.IMG
Example:  FATINFO.G4B --start=1929 (0x9f)</code></pre>    

### HISTORY
Version 0.8.2  
Maintenance update  

Version 0.8.1  
Bugfix: test of volume/ partition starts at begin of a head on disk/ blocklist  
Bugfix: test of hidden sectors on disk/ blocklist  
Bugfix: test of fat32 backup-sector on disk/ blocklist  

Version 0.8  
New: with --hex first sector of root showed too  
New: detection of Boot Codes CRC32-based only. If unknown read-out of (standard) boot-files like IO.SYS etcetera  
New: test of start of FAT to sort-out single boot-sectors  
New: read-out of partition 0-62 with --partition=n MBR-based only (no mapping to ram-disk anymore)  
Change: test of hidden sectors less restrictive on FILE, DISK and BLOCKLIST  
Change: some echos  
Change: dropped 'Number of Root Directory Entries MUST fit in whole number of sectors'  
Bugfix: with --partition=0 'not good' in case 17/ 32 hidden sectors  

Version 0.7.1  
New: recognition of MS-DOS 7 bootcode with WINBOOT.SYS and of bootcode DELL OEM bootfloppy (OSR2)  
Bugfix: Provider MSWIN4.0/ MSWIN4.1 not recognized, fat bootcode always MS-DOS 7.1 (harmless)  
Bugfix: some echos with Switch /TQ (harmless)  
Bugfix: all echos with Switch /A32 and /L32 on fat12/ fat16 (harmless)  
Bugfix: all echos on fat12 with more than 12 sectors per FAT (harmless, but not according Microsoft specs)  
Bugfix: variables not exported with Switch (bug introduced in v0.6)  
Bugfix: argument --partition=n not working anymore

Version 0.7  
New: with --hex always pager on during operation (set to original status afterwards)  
New: display of First Root Cluster on FAT32  
New: recognition of standard FAT/ FAT32 PBR-bootcodes: MSDOS 3.3, MSDOS 4.0, MSDOS 5.0, MSDOS 7.0  
     MSDOS 7.1, Windows 2k/XP, Windows Vista, Windows 7, Windows 8, Windows 10, FREEDOS, ReactOS, GRUB  
New: use of image file names containg spaces or '='  
New: echo of Filesys: 'offset' shown for CDROM if PBR is not on 2k-sectorborder  
Bugfix: last sector on volume not always tested on (rd)  
Bugfix: with switch /T for (rd) and total sector test  
Bugfix: not compatible with partitions >= ~256GB  
Bugfix: set echo of rd-Blocklist to REAL number of sectors in ram-disk  
Bugfix: no export of VARS with /V (result only)  
Bugfix: with switch /T 'number of hidden sectors is not correct' with hd-Blocklist starting at hidden number of sectors  
Bugfix: handling of FILE with incomplete FAT  
Bugfix: bad echo if FAT32 backupsector=0 (no backupsector)  

Version 0.6.1  
New: number of Root Directory Entries must MUST fit in whole number of sectors, or FAT-file system will be rejected as not valid  
Bugfix: --skip=xxxxx bytes not working WITH FILE  

Version 0.6  
NEW: switch '/B' to get silent result 'sectors marked bad' in FAT  
NEW: CD-ROM starts at 0x9f, as (hd31) still accepted as harddrive  
IMPROVED: use of '--start=sectors|--skip=bytes' with blocklist (non-contiguous blocklists too)  
Bugfix: bad dividing to sectors with '--skip=bytes' in case of CD  
Bugfix: echo 'partition starts [not] not at begin of a head' if hidden sectors=0  
Bugfix: bad read-out of padding sectors in certain cases  
Bugfix: bad echo 'Filesystem (...) at sector xxx': correction for CD  
Bugfix: bad echo of 'jump' value if low byte was zero  

Version 0.5  
Bugfix: total sector test on ram-disk  
Bugfix: read-out startsector on partitioned ram-disk (grub4dos only!)  
Blocklist: non-contiguous blocklists supported (max about 510 chars!)  
Blocklist: same switches as FILE '--start=sectors|--skip=bytes'  
Better distinction of volume/ partition in output  
New test: read-out number of bad sectors marked as bad in FAT  
New test: calculate number of padding sectors  

Version 0.4  
Bugfix: if volume name contains wrong FAT-version  
Offset in bytes with argument '--skip=N'  
DISK supported with arguments ''--start=sector|--skip=N' for offset  
Argument '--partition=p' supported on FILE  
Only mapping to (rd) with argument '--partition=p'  
Better compatibility with grub4efi: argument '--partition=p' NOT supported  
Partly detection of partition: 'volume' in output if not  

Version 0.3  
Bugfixes  
Tests related too CHS/LBA  
Test of FSLastFree on fat32  
Fatinfo on a blocklist  
Switch for hex view of bootsector(s) and first sector of FAT1/FAT2  

Version 0.2  
First published version  

### SCREENSHOTS
![FATINFO G4B v0 8 2 VERSION and TEXTSTAT](https://github.com/user-attachments/assets/852c2cdc-904f-48b2-a05f-142ca9df7608)

#### Small Help:
![FATINFO G4B v0 7 1 SmallHelp](https://github.com/user-attachments/assets/afcff970-40bb-4b63-87b5-b0237c85c045)

#### Example of use on harddisk with fat32 partition:
![FATINFO G4B v0 8 (hd0,0)](https://github.com/user-attachments/assets/68a8b60a-fff5-456b-bd3c-4166b8415d26)

#### Example of use on 3840KB floppy in ram-drive:
![FATINFO G4B v0 8 (rd) 3840KB floppy with bootcode reactos](https://github.com/user-attachments/assets/46de809d-943e-46aa-8fc4-3a2a377ea2e0)

#### Example of test of 3840KB fat12 boot-floppy with Switch /T:
![FATINFO G4B v0 7 1 (fd0) -T FAT12 bootfloppy 3840KB](https://github.com/user-attachments/assets/f65e8707-7584-4669-a66d-e7ce10195960)

#### Example of use on image of MS-DOS 6.22 boot-floppy:
![FATINFO G4B v0 8 (hd0,0)-msdos622 img](https://github.com/user-attachments/assets/a1aaf56e-cf3a-4d12-a74c-4ffe8cc25cc6)

#### Example of use on hard-disk image with MS-DOS 7.1 fat12:
![FATINFO G4B v0 8 --start=63 (hd0,0)-hd27mb img](https://github.com/user-attachments/assets/a9b60380-988b-475a-bfdf-29aa3f74829a)

#### Example of use on bootsector of El Torito iso with harddisk emulation (bootsector NOT on 2k-sector border):
![FATINFO G4B v0 8 --skip=0x7e00 (hd0,00-eltorito iso](https://github.com/user-attachments/assets/5c33a07d-ec6d-4157-913c-172b4a5c857d)

#### Example of how to access partition on harddisk image for test, with Long File Name and Switch /T:
![FATINFO G4B v0 7 1 --partition=0 ''(hd0,0)-L F N = Harddisk IMG'' with FAT16 using (rd,0)](https://github.com/user-attachments/assets/89495428-005f-4bf3-adeb-689f4d0a55f5)

#### Example of silent operation retrieving information with Switch /V:
![FATINFO G4B v0 7 1 (hd1,0) -V on FAT32 (with set)](https://github.com/user-attachments/assets/2402e473-e4c5-497c-bc0e-2f96e3827374)

#### Example of use on logical partition on harddisk image testing fat32 partition with --partition=4 and Switch /T:
![FATINFO G4B v0 8 --partition=4 (hd0,0)-35mfat32 img -T](https://github.com/user-attachments/assets/984d628b-db48-448c-94b2-3eceaf50da50)

#### Example of use on El Torito bootfloppy with blocklist and hexview (after finding bootoffset on CD-ROM with ISOINFO.G4B):
![ISOINFO G4B --isobd (0xe0) finding El Torito Emulation and Bootoffset](https://github.com/user-attachments/assets/4682cced-8fdf-4a21-b919-93391995473b)
![FATINFO G4B v0 8 --hex (0xe0)0x15+720 1024 A ](https://github.com/user-attachments/assets/501e2841-1e25-4cf7-b6b2-7346a2f2450c)
![FATINFO G4B v0 8 --hex (0xe0)0x15+720 1024 B](https://github.com/user-attachments/assets/c2e31aa7-ed1f-4cad-9f52-5631db672826)
![FATINFO G4B v0 8 --hex (0xe0)0x15+720 1024 C](https://github.com/user-attachments/assets/756b4898-227a-447b-a998-b506f2596d2c)
