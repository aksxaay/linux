### QEMU


##### To-do list
- [ ] Faster Networking
- [x] Shared Clipboard
- [ ] File Sharing
- [ ] Manage the Processors


Qemu is **a machine emulator that can run operating systems and programs for one machine on a different machine**.


### History - QEMU/KVM
[reference](https://www.packetcoders.io/what-is-the-difference-between-qemu-and-kvm/#:~:text=So%20to%20conclude%3A%20QEMU%20is,virtualization%20features%20of%20various%20processors.)
So to conclude: **QEMU** is a **type 2** hypervisor that runs within user space and performs virtual hardware emulation, whereas **KVM** is a **type** **1** hypervisor that runs in kernel space, that allows a user space program access to the hardware virtualization features of various processors.

QEMU can run independently, but due to the emulation being performed entirely in software it is extremely slow. To overcome this, QEMU allows you to use KVM as an accelerator so that the physical CPU virtualization extensions can be used. So to conclude: QEMU is a type 2 hypervisor that runs within user space and performs virtual hardware emulation, whereas KVM is a type 1 hypervisor that runs in kernel space, that allows a user space program access to the hardware virtualization features of various processors.[[3]](https://www.packetcoders.io/what-is-the-difference-between-qemu-and-kvm/#fn3)

1.  [KVM](https://www.linux-kvm.org/) – _a virtualization module in the Linux kernel that allows the kernel to function as a hypervisor. Type -1, easily outperforms all Type-2._ ([Wikipedia](https://en.wikipedia.org/wiki/Kernel-based_Virtual_Machine))
2.  [QEMU](https://www.qemu.org/) – _a machine emulator and virtualizer that can perform hardware virtualization. It can cooperate with KVM to run virtual machines at near-native speed_ ([Wikipedia](https://en.wikipedia.org/wiki/QEMU))
3.  [Virtual Machine Manager](https://virt-manager.org/) – a nice GUI to use above things as simply as possible.



[KVM linux mint guide](https://artofcode.wordpress.com/2021/02/04/how-to-install-kvm-qemu-on-linux-mint-20-1/)

- install certain apps
	- qemu-kvm
	- libvirt-daemon-system
	- libvirt-clients
	- bridge-utils
	- virt-manager
done

added user to the respective groups
virsh

`sudo virsh net-autostart default`

`sudo virsh net-list --all`


Mental Outlaw
- libguestfs - modifying VM disk images
- openbsd-netcat - A simple Unix utility which reads and writes data across network connections using TCP or UDP protocol.
- dnsmasq -  dnsmasq is a lightweight, easy to configure DNS forwarder, designed **to provide DNS (and optionally DHCP and TFTP) services to a small-scale network**.


I can't configure a bridge, and I've tried all the methods of what I can even remotely understand

[create-configure-bridge](https://computingforgeeks.com/how-to-create-and-configure-bridge-networking-for-kvm-in-linux/#:~:text=You%20need%20to%20have%20installed%20KVM%20on%20your%20system.&text=Configure%20a%20new%20network%20interface,window%2C%20provide%20virtual%20network%20information.)


`nmcli`
virbr - virtual bridge

setting it to `virtio` doesn't work
maybe hypervisor default does something?


apparently it should be on 2 threads like mental outlaw said so, but there's  a performance to memory ratio that having higher threads would benefit from, but not quite sure what that exactly would be.

I should really try giving it more CPU cores that would be fucking sick asf

[redhat cpu config](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/virtualization/ch33s08)

there is an inherent download and upload speed difference
![[Pasted image 20220312120108.png|400]]
inside vm
![[Pasted image 20220312120216.png|400]]
outside vm


video works sometimes, but can see the struggle
but-
the sound doesn't not come through

`aplay -l`
HDA intel PCH - Analog

[enable audio in qemu](https://techpiezo.com/linux/enable-audio-in-qemu-virtual-machine/)
`qemu-system-x86_64 -soundhw help`
shows all the devices that I can use

the processor is estimated to be half of its' stock performance

so the multiscore
![[Pasted image 20220312125136.png]]

### File Sharing
Now I have to figure out File Sharing between QEMU/KVM and the host system.


Clipboard
[enable clipboard + folder sharing // only on windows guest](https://dausruddin.com/how-to-enable-clipboard-and-folder-sharing-in-qemu-kvm-on-windows-guest/)
download `spice-webdavd` for file sharing


[folder sharing kvm // ostechnix](https://ostechnix.com/setup-a-shared-folder-between-kvm-host-and-guest/)


Tryna see if I can expand storage yk
rn the volume is over at gigga
`.qcow2` is the file format

[command line extend .qcow](https://computingforgeeks.com/how-to-extend-increase-kvm-virtual-machine-disk-size/)

[kvm performance tuning // youtube](https://www.youtube.com/watch?v=0Aft9kULiwc)

KVM snapshots are also kinda cool in-case I break sumn lol
`/var/lib/libvirt/images` this is where the snapshots are saved in


[dynamic / sparse Disk Allocation Problem](https://bytesandbones.wordpress.com/2019/11/06/kvm-qemu-qcow2-sparse-disk-allocation-problem-shrink-sparse-qcow2-disks-howto/)
virt-manager causes issues.
It's called Sparse Allocation?




dt knows how to share files- By creating shared mount points

[create dynamic disks .qcow2 // stackoverflow](https://askubuntu.com/questions/1298291/how-to-create-dynamic-disks-qcow2-as-it-happens-with-vdi)

```qemu-img create -f qcow2 -o preallocation=off <disk-name> <disk-size>```

found this somewhere in the corner

[Disk Images // QEMU Documentation](https://qemu.weilnetz.de/doc/6.0/system/images.html#cmdoption-image-formats-arg-vmdk)
- monolithicFlat
>`subformat`[](https://qemu.weilnetz.de/doc/6.0/system/images.html#cmdoption-image-formats-arg-subformat "Permalink to this definition")
Specifies which VMDK subformat to use. Valid options are `monolithicSparse` (default), `monolithicFlat`, `twoGbMaxExtentSparse`, `twoGbMaxExtentFlat` and `streamOptimized`.

>The VMDK format includes multiple differing subformats, **some of which store metadata in an external descriptor file, while others embed it with the main data in a single file**.


**A flat image allocates space ahead of time while a sparse images grows as the virtual machine writes to it.**


```qemu-img info win10.qcow2```
and this gives - 
> image: win10.qcow2
file format: qcow2
virtual size: 128 GiB (137438953472 bytes)
disk size: 73.7 GiB
cluster_size: 65536
Snapshot list:
ID        TAG                     VM SIZE                DATE       VM CLOCK
1         ps-ae-clipboard-setup    3.3 GiB 2022-03-14 13:26:16   00:08:22.463
2         ae-plugins-setup       3.82 GiB 2022-03-16 23:29:31   05:45:16.241
Format specific information:
    compat: 1.1
    lazy refcounts: true
    refcount bits: 16
    corrupt: false

```qemu-img info kali.qcow2```
>image: kali.qcow2
file format: qcow2
virtual size: 50 GiB (53687091200 bytes)
disk size: 196 KiB
cluster_size: 65536
Format specific information:
    compat: 1.1
    lazy refcounts: false
    refcount bits: 16
    corrupt: false

### Disk Image Utility
`qemu-img`
[QEMU Image Command](https://www.ibm.com/docs/en/linux-on-systems?topic=commands-qemu-image-command)

I just noticed Display Spice has a couple configs on it.

OpenGL: and we can choose a Card crazy.
Not many people apparently know about SPICE
[spice vs vnc // reddit](https://www.reddit.com/r/linux/comments/mlmbj/not_many_people_know_about_spice_and_what_is/)


About [Video QXL // archwiki](https://wiki.archlinux.org/title/QEMU/Guest_graphics_acceleration)

had to change it to UEFI - ahha

[attempt to read or write outside of disk hd0](https://superuser.com/questions/738231/error-attempt-to-read-or-write-outside-of-disk-hd0)


so nvm kali was installing properly, I think that security wasn't meant to be used that way ig?


anyways, I somehow setup the vm
on BIOS and straight lazy installation
also for some reason KDE Parrot didn't let me change resolution
so had to change Video Model to `Virtio`


### File Sharing
[sharing files host -> guest](https://www.linux-kvm.org/page/9p_virtio)

[ostechnix // shared folder between kvm-host and kvm-guest](https://ostechnix.com/setup-a-shared-folder-between-kvm-host-and-guest/)
Host steps:
`mkdir ~/KVM_Share`

full perms
`chmod 777 ~/KVM_Share`

correct SELinux type for shared folder
```
sudo semanage fcontext -a -t svirt_image_t "/home/sk/KVM_Share(/.*)?"
```

wait I can't follow these steps
- I don't have `libsemanage`
- don't have selinux either
wait fuck lets try out warpinator

Guest steps:


we can also try using scp = secure copy


https://docs.nvidia.com/grid/latest/grid-vgpu-release-notes-generic-linux-kvm/index.html

https://documentation.suse.com/sles/15-SP3/html/SLES-all/article-nvidia-vgpu.html