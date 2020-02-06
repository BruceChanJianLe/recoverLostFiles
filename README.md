# Recover Lost Files  
How to recover lost files from rm -rf in Linux.  
Please do not attempt to create the same folder or file at where you lost it. Try no to do anything on the folder where you lost your file it may become hindrance to you when you try to recover the lost files.  

## Tools
- `sudo apt-get install foremost`
- `sudo apt-get install extundelete`
- `sudo apt-get install ext4magic`

This reference recommend using `ext4magic`

## Start  
**Step 1**  
Install the tool.  
`sudo apt-get install ext4magic`  

**Step 2**    
- 1. Find the name of your drive. Note that based on the size of the drive you may tell which one it is in.  
`sudo fdisk -l`  
example output:
```
...
Device              Start        End    Sectors   Size Type
/dev/nvme0n1p1       2048    1050623    1048576   512M EFI System
/dev/nvme0n1p2    1050624 1998407679 1997357056 952.4G Linux filesystem
/dev/nvme0n1p3 1998407680 2000408575    2000896   977M Linux swap
```
- 2. Find the block size.
`sudo dumpe2fs -h /dev/nvme0n1p2`  
example output:
```
Free inodes:              61877835
First block:              0
Block size:               4096
Fragment size:            4096
```
Block size is `4096` for this example.  

**Step 3**  
Find the Inode number and the transaction number.  
`sudo ext4magic /dev/nvme0n1p2 -s 4096 -J -f /home/jl/Documents/File-directory`  
-s takes the block size, which is 4096  
-J prints out the information of Inode and transaction  
-f takes the path to your lost file  
example output:
```
Filesystem in use: /dev/nvme0n1p2

Using  internal Journal at Inode 8
Inode found "/home/jl/Documents/File-directory"   8522621 

Journal Super Block:
 Signature: 0xc03b3998 
 Blocktype : V2 superblock 
 Journal block size: 4096 
 Number of journal blocks: 32768 
 Journal block where the journal actually starts: 1
 Sequence number of first transaction: 525559
 Journal block of first transaction: 1
 Error number: 0
 Compatible Features: 0
 Incompatible features: 1
 Read only compatible features: 0
 Journal UUID: d7d55592-8309-4b2d-8f88-ae9b6e50df53 
 Number of file systems using journal: 1
 Location of superblock copy: 0
 Max journal blocks per transaction: 0
 Max file system blocks per transaction: 0
 IDs of all file systems using the journal:
01. : 00000000-0000-0000-0000-000000000000
 


Dump Inode 8522621 from journal transaction 522768
Inode: 8522621   Type: directory    Mode:  0775   Flags: 0x80000 
Generation: 1693757458    Version: 0x00000000:00000028
User:  1000   Group:  1000   Size: 4096
File ACL: 0    Directory ACL: 0
Links: 9   Blockcount: 8
Fragment:  Address: 0    Number: 0    Size: 0
 ctime: 1580941836:3003794864 -- Thu Feb  6 06:30:36 2020
 atime: 1580912836:0834033352 -- Wed Feb  5 22:27:16 2020
 mtime: 1580941836:3003794864 -- Thu Feb  6 06:30:36 2020
crtime: 1579320237:3384736908 -- Sat Jan 18 12:03:57 2020
Size of extra inode fields: 32
  8522621  d  775 (2)   1000   1000           4096  6-Feb-2020 06:30 .
  3933275  d  775 (2)   1000   1000           4096 19-Jan-2020 15:36 ..
  8522622  _  664 (1)   1000   1000         177945 18-Jan-2020 12:05 Lostfile.png
  8522625  _  664 (1)   1000   1000            756 18-Jan-2020 15:03 Lostfile.cpp
  8521484  _  664 (1)   1000   1000           3565  5-Feb-2020 21:31 Lostfile.md
  8522627  d  775 (2)   1000   1000           4096  5-Feb-2020 22:34 Lostfile
  8521358  d  754 (2)      0      0           4096  6-Feb-2020 05:57 output
```
Node the Inode number is `8522621` and the transaction number is `522768`.  

**Step 4**  
Final step require that you have another hard-drive (extrenal hard disk drive), so that you can direct the files there. Note that it will not work if they are on the same system. If you use `extundelete` you can do it on the same drive.  
`sudo ext4magic /dev/nvme0n1p2 -s 4096 -R - I 8522621 -t 522768 -d /media/jl/New\ Volume/Restore_Files`  
-R means recovery  
-I takes the Inode number  
-t takes the transaction number (this is optional)
-d is the output folder path, everything that is recovered will be directed to that folder  
No example output for this one. Finally, check for your recovered files.

## Reference  
[video](https://www.youtube.com/watch?v=HN2T4CXLY6A)  
[link](https://askubuntu.com/questions/604311/how-to-restore-data-after-running-rm?noredirect=1&lq=1)  
