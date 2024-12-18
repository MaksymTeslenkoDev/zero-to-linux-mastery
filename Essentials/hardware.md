# Hardware

This chapter introduces and explains the basics of what makes up a computer and expands to a few optional items that are fairly commonplace.

## Motherbord

The motherboard, or system board, is the main hardware board in the computer through which the central processing unit (CPU), random-access memory (RAM) and other components are all connected. Depending on the computer type (laptop, desktop, server) some devices, such as processors and RAM, are attached directly to the motherboard, while other devices, such as add-on cards, are connected via a bus.

## Processors

A central processing unit (CPU or processor) is one of the most critical hardware components of a computer. The processor is the brain of the computer, where the execution of code takes place and where most calculations are done. 

Although support is available for more types of processors in Linux than any other operating system, there are primarily just two types of processors used on desktop and server computers: x86 and x86_64. On an x86 system, the system processes data 32 bits at a time; on an x86_64 the system processes data 64 bits at a time. An x86_64 system is also capable of processing data 32 bits at a time in a backward compatible mode. One of the main advantages of a 64-bit system is the ability to work with more memory, while other advantages include increased efficiency of processing and increased security.

To see which family the CPU of the current system belongs to, use the `arch` command:

```bash
sysadmin@localhost:~$ arch
x86_64
```

For more information concerning the CPU, use the `lscpu` command:
```bash
sysadmin@localhost:~$ lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                4
On-line CPU(s) list:   0-3                                                    
Thread(s) per core:    1                                                      
Core(s) per socket:    4                                                      
Socket(s):             1
NUMA node(s):          1                                                      
Vendor ID:             GenuineIntel   
```
The first line of this output shows that the CPU is being used in a 64-bit mode, as the architecture reported is x86_64. The second line of output shows that the CPU is capable of operating in either a 32- or 64-bit mode, therefore, it is actually a 64-bit CPU.

## Random Access Memory

The motherboard typically has slots where random-access memory (RAM) can be connected to the system. The 32-bit architecture systems can use up to 4 gigabytes (GB) of RAM, while 64-bit architectures are capable of addressing and using far more RAM.

When the system is running low on RAM, virtualized memory called swap space is used to temporarily store data to disk. Data stored in RAM that has not been used recently is moved over to the section of the hard drive designated as swap, freeing up more RAM for use by currently active programs. If needed, this swapped data can be moved back into RAM at a later time. The system does all of this automatically as long as swap is configured.

To view the amount of RAM in your system, including the swap space, execute the `free` command. The `free` command has a `-m` option to force the output to be rounded to the nearest megabyte (MB) and a `-g` option to force the output to be rounded to the nearest gigabyte (GB): 
```bash
sysadmin@localhost:~$ free -m                                                
             total       used       free     shared    buffers     cached     
Mem:          1894        356       1537          0          25       177     
-/+ buffers/cache:        153       1741                                      
Swap:         4063          0       4063
```
The output shows that this system has a total of 1,894 MB and is currently using 356 MB.

The amount of swap appears to be approximately 4 GB, although none of it appears to be in use. This makes sense because so much of the physical RAM is free, so there is no need at this time for virtual RAM (swap space) to be used.

## Hard Drives

Hard drives are associated with file names (called device files) that are stored in the `/dev` directory. Each device file name is made up of multiple parts.

- File type
    The file name is prefixed based on the different types of hard drives. IDE (Intelligent Drive Electronics) hard drives begin with `hd`, while USB, SATA (Serial Advanced Technology Attachment) and SCSI (Small Computer System Interface) hard drives begin with `sd`.
- Device Order
    Each hard drive is then assigned a letter which follows the prefix. For example, the first IDE hard drive would be named /dev/hda and the second would be /dev/hdb, and so on.
- Partition
    Finally, each partition on a disk is given a unique numeric indicator. For example, if a USB hard drive has two partitions, they could be associated with the /dev/sda1 and /dev/sda2 device files

The following example shows a system that has three sd devices: /dev/sda, /dev/sdb and /dev/sdc. Also, there are two partitions on the first device, as evidenced by the /dev/sda1 and /dev/sda2 files, and one partition on the second device, as evidenced by the /dev/sdb1 file:
```bash
root@localhost:~$ ls /dev/sd*                                               
/dev/sda /dev/sda1  /dev/sda2  /dev/sdb  /dev/sdb1  /dev/sdc
```

The `fdisk` command can be used to display further information about the partitions:

```bash
root@localhost:~$ fdisk -l /dev/sda                                             
Disk /dev/sda: 21.5 GB, 21474836480 bytes                                       
255 heads, 63 sectors/track, 2610 cylinders, total 41943040 sectors             
Units = sectors of 1 * 512 = 512 bytes                                          
Sector size (logical/physical): 512 bytes / 512 bytes                           
I/O size (minimum/optimal): 512 bytes / 512 bytes                               
Disk identifier: 0x000571a2                                                     
                                                                                
⁠⁠​ ⁠
   Device Boot      Start         End      Blocks   Id  System                  
/dev/sda1   *        2048    39845887    19921920   83  Linux                   
/dev/sda2        39847934    41940991     1046529    5  Extended                
/dev/sda5        39847936    41940991     1046528   82  Linux swap / Solaris
```

## Solid State Disks

While the phrase hard disk is typically considered to encompass traditional spinning disk devices, it can also refer to the newer and very different solid state drives or disks.

Solid state disks are essentially larger capacity USB thumb drives in construction and function. As there are no moving parts, and no spinning disks, just memory locations to be read by the controller, a solid state disk is measurably and visibly faster in accessing the information stored in its memory chips.

Advantages of a solid state disk include lower power usage, time savings in system booting, faster program loads, and less heat and vibration from no moving parts. Disadvantages include higher costs in comparison to spinning hard disks, lower capacity due to the higher cost, and if soldered directly on the motherboard/mainboard, no ability to upgrade by swapping out the drive.

