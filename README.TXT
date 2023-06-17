FATINFO.G4B v0.2 (20230617), by deomsh (grub4dos version 20170607 or higher)
Function: Get FAT-info (no switch), or switch: basic tests or return variable(s)
Use 1:   FATINFO.G4B [--mdbase=sector] DEVICE [switch]
Use 2:    FATINFO.G4B [--mdbase=sector] [--start=sector] FILE [switch]
Switch:   /T|/TQ|/F|/FT|/A|/A32|/L|/L32|/V
Remarks:  DEVICE: existing FAT-devices (always with parentheses!)
          FILE: existing FAT-imagefile (always at least with path!)
          --mdbase=sector to change startsecor of memory in use (default 0x3000)
                   max 512 sectors needed
          --start=sector to find bootsector in imagefile (default=0)
Switch:   Only ONE switch allowed:
          /T tests: Fit on partition, Last sector valid, Media descriptor
           Signatures, Sectors per FAT, Media descr. in FAT1, Check Dirty bit,
           FAT1/ FAT2 equal, FSInfo equal to free cluster-count (fat32 only)
          /TQ quit tests: if all tests passed variable result=1
           otherwise 0 (if no/ not all tests done variable result not exist)
          /F quiet test if DEVICE is FAT (variable result=1 0)
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

History:
Version 0.7 
First published verison
