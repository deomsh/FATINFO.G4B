## FATINFO.G4B v0.6.1 (20240728), by deomsh
<pre><code>Function: Get FAT-info (no switch), or switch: basic tests or return variable(s)
Use 1:    FATINFO.G4B [--mdbase=sector] [--hex] DEVICE [switch]
Use 2:    FATINFO.G4B [--mdbase=sector] [--start=sector|--skip=N|--partition=p] [--hex] FILE [switch]
Use 3:    FATINFO.G4B [--mdbase=sector] [--start=sector|--skip=N|--partition=p] [--hex] BLOCKLIST [switch]
Use 4:    FATINFO.G4B [--mdbase=m] --start=n|--skip=N [--hex] DISK [switch]
Switch:   /T|/TQ|/F|/FT|/A|/A32|/L|/L32|/B|/V
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
            FILE/ BLOCKLIST mapped to (rd) if fit in memory
            With --partition=0-62 FILE/ BLOCKLIST mapped to (rd) if fit in memory
            Sizes above 2GB loaded in top memory if available
            Afterwards blocklist is available in (rd), partitions (rd,0) and up
            To get FAT info, size must be a least Hidden + Reserved + FAT sectors
          --hex to view in hex bootsectors and first sector of FAT(s)
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
          /V get quiet fat info on device. variables: FILESYS BYTEPSEC SECPCLUS
            RESERVED NUMFATS SECPFAT SECTRACK NUMHEADS ROOTENTR DEVSECT MEDIABYT
            UUID + on fat32: CLUSFREE NEXTFREE (FSInfo)
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
Example:  FATINFO.G4B (hd0,0) /B ;; set result
Example:  FATINFO.G4B (hd0,0)/FDDIMAGE.IMG
Example:  FATINFO.G4B --start=63 /HDDIMAGE.IMG
Example:  FATINFO.G4B (0xe0)0x2C+720
Example:  FATINFO.G4B --partition=0 (hd0,0)44992028+2g
Example:  FATINFO.G4B --start=63 (hd0,0)44992028+2g
Example:  FATINFO.G4B --partition=0 /HDDIMAGE.IMG
Example:  FATINFO.G4B --start=1929 (cd)</code></pre>    

### HISTORY
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
