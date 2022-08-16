### USB not live installer OS


I'm basically trying to install garuda on a separate usb, and this article covers everything important.

[itsfoss // install any os on USB](https://itsfoss.com/intsall-ubuntu-on-usb/)

idea is 
- any drive on the system should not have an efi partition flag, otherwise you can't use it as intended and may receive grub error.

- Solution: just temporarily remove the ESP flag from the partition and you should be good to go.

mine already didn't have `esp` flag bruh

when no flag is selected it defaults to `msftdata`


### fixing Cruzer Blade 16 gb usb


I/O error file system drive that gives enough verbose outputs
[repair corrupt fat32 file system](https://askubuntu.com/questions/147228/how-to-repair-a-corrupted-fat32-file-system)

```
sudo dosfsck -w -r -l -a -v -t /dev/sdc1
```
ran this verbose command.

```
fsck.fat 4.1 (2017-01-24)
Checking we can access the last sector of the filesystem
0x41: Dirty bit is set. Fs was not properly unmounted and some data may be corrupt.
 Automatically removing dirty bit.
Boot sector contents:
System ID "MSDOS5.0"
Media byte 0xf8 (hard disk)
       512 bytes per logical sector
      8192 bytes per cluster
      3050 reserved sectors
First FAT starts at byte 1561600 (sector 3050)
         2 FATs, 32 bit entries
   7607808 bytes per FAT (= 14859 sectors)
Root directory start at cluster 2 (arbitrary size)
Data area starts at byte 16777216 (sector 32768)
   1901950 data clusters (15580774400 bytes)
63 sectors/track, 255 heads
        32 hidden sectors
  30463968 sectors total
Got 64512 bytes instead of 7607808 at 1561600
```


trying `f3`

[checking physical health of USB drive](https://www.cyberciti.biz/faq/linux-check-the-physical-health-of-a-usb-stick-flash-drive/)
using f3 utility it does sector checks OK!

we can also use dmesg, that tells us everything important that goes on in linux
> print and control kernel ring buffer