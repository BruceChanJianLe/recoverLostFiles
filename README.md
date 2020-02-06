# Recover Lost Files  
How to recover lost files from rm -rf in Linux.  

## Tools
- `sudo apt-get install foremost`
- `sudo apt-get install extundelete`
- `sudo apt-get install ext4magic`

This rkkkkeference recommend using `ext4magic`

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
Block size is 4096 for this example.  

**Step 3**  
Find the 


## Reference  
[video](https://www.youtube.com/watch?v=HN2T4CXLY6A)  
[link](https://askubuntu.com/questions/604311/how-to-restore-data-after-running-rm?noredirect=1&lq=1)  
