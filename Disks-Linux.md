
## Disks Linux

I'm also having mounting issues with my hard-drive.. It's called `my gigga` and that space is fucking killing me

  

I need to know all the commands in order to rename it to sumn, where the path isn't broken.

  

`cat /proc/mounts | grep gigga`

alt

`mount | grep gigga`

  

interesting to see that fstab doesn't have it listed

  

so I have to change it?

  

`sudo fdsik -l` lists disk and it's types

- format disk utility

  
  

`lsblk` doesn't list it right as well.. damn this is confusing.

```

# lsblk

NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT

sda 8:0 0 931.5G 0 disk

└─sda1 8:1 0 931.5G 0 part

nvme0n1 259:0 0 238.5G 0 disk

├─nvme0n1p1 259:1 0 243M 0 part /boot/efi

└─nvme0n1p2 259:2 0 238.2G 0 part /

```

what it takes to rename an external hard-drive
![[Pasted image 20220331000051.png]]