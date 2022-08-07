## Logical Volume Manager (LVM)

**LVM** - introduces an extra layer between the physical disks and the file system 
allowing file systems to be:
- **resized** and moved easily, w/o outage
- discontinuous space on disk (data protection)
- meaningful names to volumes, not cryptic shit
- can span **multiple physical disks**, can merge multiple incompatible hard drives
[reference](https://www.youtube.com/watch?v=MeltFN-bXrQ)

**why is it useful?**
- can dynamically shift disk sizes without having to restart / downtime
- recommended for servers absolutely


So far I only found it by default on Kali / Ubuntu - **Ubiquity**
Apparently Calamares is just not as intuitive rip.
- Guided - Entire disk and set up LVM

video creator's only gripe, they don't let them choose how much of the hard drive to dedicate to the LVM 

if you need LVM snapshots, you need some un-claimed space.
OHH, basically like another swap file like thing.


-> Create New Partition Table
![[configure-logical-volume-manager.png]]


need to set LVM manually
![[Pasted image 20220320234252.png]]

if a 100% of that hard drive had been used? you need to reserve unclaimed couple gigabytes, need emergency space in a pinch, a lil but of a buffer.

- leave some unclaimed spaces

### Proper LVM Layout
[Beginner's Guide to LVM // the geek diary](https://www.thegeekdiary.com/redhat-centos-a-beginners-guide-to-lvm-logical-volume-manager/)


![diagram](https://cdn.thegeekdiary.com/wp-content/uploads/2014/10/LVM-basic-structure.png)

Phys -> Volume Group -> Logical Volume -> FS
LVM Physical Volume - Initialize
VG - arbitrary container to contain other volumes


the only way to solve no Phys space for correction, is add another drive make it physical volume, merge and continue process

![[Pasted image 20220320235104.png]]

so after this we made 
- 1 Physical Volume
- 1 Volume Group
![[Pasted image 20220320235238.png]]

`vg_ubuntu` is the VG

Step - 3
**LV**
- lv_root - *8G*
- lv_home - 

each has naming purpose.

Final Config
![[lvm-config.png]]

okay then that was weird, this image just refused to form. okay.

![[Pasted image 20220320235723.png]]
0 - 1 - 1 - 1

![[Pasted image 20220320235753.png]]

choose #1 
Use as: ext4, btrfs, 

- Ext4
Mount Point: / 

![[Pasted image 20220320235954.png]]


Final Config
![[Pasted image 20220321000039.png]]


an actual file system will look like this if you ran `df -h`
![[Pasted image 20220321000150.png]]
/dev/mapper/vg_ubuntu-lv_root

interesting naming
just adds '-' to `LG-LV`

`lsblk`


Part - 4 : Expanding a File System
- Expand `lv_root` to a larger size


`pvcreate` /dev/sdb
- don't use it against an important harddrive, will clean
- converted to Physical Volume

`pvdisplay`
![[Pasted image 20220321002131.png]]

not claimed by LVM

`vgextend` vg_ubuntu /dev/sdb

extending `vg_ubuntu` by adding `/dev/sdb`

`df -h` still don't show nothin

we're not that far yet

`vgdisplay`
we have 32 gigs free?
additional room to play
- create another logical volume lmao


`lvextend` -L +10G 


resize the fs? can have the available space that the LVM has

need to find difference between filesystem and what its mounted on

`/dev/sdb1` mounted on `/media/axsae/SONANCE`
`df -h`



### kali LVM setup

Kali Linux Setup
![[kali-lvm-setup.png]]

![[Pasted image 20220321153611.png]]
this is the LVM setup
I can't resize these sizes - espeically that primary partition for /boot

full Kali install takes a good minute, 
a whole ass hour at best.