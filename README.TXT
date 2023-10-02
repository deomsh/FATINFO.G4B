FATINFO.G4B v0.3 (20231002), by deomsh (grub4dos version 20170607 or higher)
Function: Get FAT-info (no switch), or switch: basic tests or return variable(s)
Use 1:    FATINFO.G4B [--mdbase=sector] [--hex] DEVICE [switch]
Use 2:    FATINFO.G4B [--mdbase=sector] [--start=sector] [--hex] FILE [switch]
Use 3:    FATINFO.G4B [--mdbase=sector] [--partition=0-62] [--hex] BLOCKLIST [switch]
Switch:   /T|/TQ|/F|/FT|/A|/A32|/L|/L32|/V
Help:     FATINFO.G4B [/?]
Remarks:  DEVICE: existing FAT-devices (always with parentheses, no path allowed!)
          FILE: existing FAT-imagefile (always at with path!)
          --mdbase=sector to change startsecor of memory in use (default 0x3000)
                   max 512 sectors needed
          --start=sector to find bootsector in imagefile (default=0)
          --partition=0-62 to get bootsector in blocklist (default: no partition)
          --hex to view in hex bootsectors and first sector of FAT(s)
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
          /V get quiet fat info on device. variables: FILESYS BYTEPSEC SECPCLUS
           RESERVED NUMFATS SECPFAT SECTRACK NUMHEADSROOTENTR DEVSECT MEDIABYT
           UUID + on fat32: CLUSFREE NEXTFREE (FSInfo)
        Blocklist is mapped to (rd), size must fit in available memory
         Above 2GB loaded in top memory if available
         Afterwards blocklist is available in (rd), partitions (rd,0) and up
         To get FAT info, size must be a least Hidden + Reserved + FAT sectors
Example:  FATINFO.G4B (fd0)
Example:  FATINFO.G4B (hd0,0)
Example:  FATINFO.G4B (rd)
Example:  FATINFO.G4B (fd0) /T
Example:  FATINFO.G4B (hd0,0) /T
Example:  FATINFO.G4B (rd) /TQ ;; set result
Example:  FATINFO.G4B (rd) /F ;; set result
Example:  FATINFO.G4B (hd2,0) /FT ;; set result
Example:  FATINFO.G4B (hd1,0) /V ;; set
Example:  FATINFO.G4B (hd1,0) /L ;; set result
Example:  FATINFO.G4B (hd0,0) /A32 ;; set result
Example:  FATINFO.G4B (hd0,0)/FDDIMAGE.IMG
Example:  FATINFO.G4B --start=63 /HDDIMAGE.IMG
Example:  FATINFO.G4B (0xe0)0x2C+720
Example:  FATINFO.G4B --partition=0 (hd0,0)44992028+2g

History:
Version 0.3
Bugfixes
Tests related too CHS/LBA
Test of FSLastFree on fat32
Fatinfo on a blocklist
Switch for hex view of bootsector(s) and first sector of FAT1/FAT2

Version 0.2
First published version
