---
title: "Boot a windows partition from linux using KVM"
description: ""
date: 2021-12-12T08:03:42+01:00
featured: false
draft: false
comment: true
toc: true
reward: true
categories:
tags:
images:
  - /images/feature/linux-windows.jpg
---

This guide explains how to boot a physical Windows partition inside linux using qemu/kvm and virt-manager. It extends the three tutorials (in particular jianmin's tutorial) linked in the references section below and tries to provide more detaild step-by-step instructions.

<!--more-->

This guide proceeds in tiny steps so that we can ensure that the changes are working after each step. In the first part of the guide all the commands are run using `qemu` on the command line and only at the end everything is tied together and a VM is configured in Virt Manager. Each Step has its own references section with links to other guides usful to better understand the instructions in that Step. Some Steps that are not required are crossed out. They have been kept as a reference to things not to do. The commands have been tested on Manjaro linux.

## References

[https://jianmin.dev/2020/jul/19/boot-your-windows-partition-from-linux-using-kvm/](https://jianmin.dev/2020/jul/19/boot-your-windows-partition-from-linux-using-kvm/)

[https://k3a.me/boot-windows-partition-virtually-kvm-uefi/](https://k3a.me/boot-windows-partition-virtually-kvm-uefi/)

[https://lejenome.tik.tn/post/boot-physical-windows-inside-qemu-guest-machine](https://lejenome.tik.tn/post/boot-physical-windows-inside-qemu-guest-machine)

# Random concepts

## Virtio drivers

Paravirtualized devices, i.e. almost bare metal performance

Any virtio requires driver both on the host and guest

## Graphics

### GPU passthrough

Ideal case, the GPU is completely handled by the guest VM.

"Simple" to do with a dual GPU, one is passed the other is used by the host

Possible with a single GPU with unbinding, but cumbersome

[https://czak.pl/2020/04/09/three-levels-of-qemu-graphics.html](https://czak.pl/2020/04/09/three-levels-of-qemu-graphics.html)

### Looking glass

[https://looking-glass.io/](https://looking-glass.io/)

Solves some problems of the passthrough. Alpha code.

### Virtio (virgl)

[https://ryan.himmelwright.net/post/virtio-3d-vms/](https://ryan.himmelwright.net/post/virtio-3d-vms/)

Useful for Linux guest, because it is only useful for OpenGL (no DirectX)

# Install KVM and QEMU

1. Follow this [guide](https://wiki.archlinux.org/title/KVM#Checking_support_for_KVM) to check that:
    - virtualization is supported and enabled in the bios
    - kvm kernel modules are loaded
2. Install the required packages
    1. Install the basic packages
       
        ```bash
        sudo pacman -S qemu virt-manager dnsmasq iptables-nft
        ```
        
    2. Install the UEFI firmware
       
        ```bash
        sudo pacman -S edk2-ovmf
        ```
    
3. Add our user to the kvm and libvirt groups
   
    ```bash
     sudo gpasswd -a $(whoami) kvm
     sudo gpasswd -a $(whoami) libvirt
    ```
    
4. Run and enable boot up start libvirtd daemon 
   
    ```bash
    sudo systemctl enable --now libvirtd
    ```
    

## References

[https://gist.github.com/diffficult/cb8c385e646466b2a3ff129ddb886185](https://gist.github.com/diffficult/cb8c385e646466b2a3ff129ddb886185)

# Boot a physical Windows partition with UEFI

## General notes

- Adding/removing devices, cdrom, etc. may require changing the boot order (follow the troubleshoot in Step 4)
- Sometimes Windows forgets to load the VirtIO disk driver and give you an `INACCESSIBLE_BOOT_DEVICE` repeat the procedure in Step 6 to make it happy

## Step 1 - Create a virtual disk that includes the Windows partition

1. Create two partitions for the efi bootloader
   
    ```bash
    dd if=/dev/zero of=efi1 bs=1M count=100
    dd if=/dev/zero of=efi2 bs=1M count=1
    ```
    
2. Identify the name of the Windows partition, e.g., `/dev/nvme0n1p3`
3. Create a script `start_md0` to build the disk
   
    ```bash
    #!/usr/bin/env bash
    WIN=/dev/nvme0n1p3
    EFIDIR=$(cd $(dirname "${BASH_SOURCE[0]}") && pwd)
    
    set -e
    
    if [[ -e /dev/md0 ]]; then
      echo "/dev/md0 already exists"
      exit 1
    fi
    
    if mountpoint -q -- "${WIN}"; then
      echo "Unmounting ${WIN}..."
      umount ${WIN}
    fi
    modprobe loop
    modprobe linear
    LOOP1=$(losetup -f)
    losetup ${LOOP1} "${EFIDIR}/efi1"
    LOOP2=$(losetup -f)
    losetup ${LOOP2} "${EFIDIR}/efi2"
    mdadm --build --verbose /dev/md0 --chunk=512 --level=linear --raid-devices=3 ${LOOP1} ${WIN} ${LOOP2}
    chown $USER:disk /dev/md0
    echo "$LOOP1 $LOOP2" > ~/.win10-loop-devices
    ```
    
4. Create a script `stop_md0`
   
    ```bash
    #!/usr/bin/env bash
    mdadm --stop /dev/md0
    xargs losetup -d < /root/.win10-loop-devices
    ```
    
5. Test the scripts

   ```bash
   sudo ./start_md0
   ls /dev/md0
   sudo ./stop_md0
   ```

   

## Step 2 - Create a partition table on the virtual disk

```bash
sudo parted /dev/md0
(parted) unit s
(parted) mktable gpt
(parted) mkpart primary fat32 2048 204799    # depends on size of efi1 file
(parted) mkpart primary ntfs 204800 -2049    # depends on size of efi1 and efi2 files
(parted) set 1 boot on
(parted) set 1 esp on
(parted) set 2 msftdata on
(parted) name 1 EFI
(parted) name 2 Windows
(parted) quit
```

## Step 3 - Write a BCD entry on the UEFI boot menu

1. Download a [Windows DVD 10 ISO](https://www.microsoft.com/it-it/software-download/windows10ISO) (free)
2. Boot the DVD
   
    ```bash
    qemu-system-x86_64 \
        -bios /usr/share/ovmf/x64/OVMF_CODE.fd \
        -drive file=/dev/md0,media=disk,format=raw \
        -cpu host -enable-kvm -m 2G \
        -cdrom /media/dataHD/system/kvm-windows/win10iso/Win10_21H1_English_x64.iso
    ```
    
3. Click `Shift + F10` to open a shell
4. Assign a letter to the EFI partition
   
    ```powershell
    diskpart
    DISKPART> list disk
    DISKPART> select disk 0    # Select the disk
    DISKPART> list volume      # Find EFI volume (partition) number
    DISKPART> select volume 2  # Select EFI volume
    DISKPART> assign letter=B  # Assign B: to EFI volume
    DISKPART> exit
    ```
    
5. Write a Boot Configuration Data (BCD) entry
   
    ```powershell
    bcdboot C:\Windows /s B: /f ALL
    ```
    

## Step 4 - Boot the system the first time

```bash
qemu-system-x86_64 \
    -bios /usr/share/ovmf/x64/OVMF_CODE.fd \
    -drive file=/dev/md0,media=disk,format=raw \
    -cpu host -enable-kvm -m 2G
```

### Troubleshoot

`failed to load boot0001 uefi qemu dvd-rom qm00003 arch bvdboot`

1. Boot the VM
2. Click `Esc` when you see the Tiano Core logo
3. From `Boot maintenance > Boot options` change the boot order and put the disk on top

### References

- [https://pve.proxmox.com/wiki/OVMF/UEFI_Boot_Entries](https://pve.proxmox.com/wiki/OVMF/UEFI_Boot_Entries)

## Step 5 - Install spice guest tools (with virtio drivers)

1. Install [spice guest tools](https://www.spice-space.org/download/windows/spice-guest-tools/spice-guest-tools-latest.exe) inside the Windows VM
2. Reboot

## ~~Step 5 - Install VirtIO guest additions~~

[Virtio drivers are included in the spice guest tools]

1. Install `virtio-win` from AUR
2. Boot the VM with the VirtioIO DVD

```bash
qemu-system-x86_64 \
    -bios /usr/share/ovmf/x64/OVMF_CODE.fd \
    -drive file=/dev/md0,media=disk,format=raw \
    -cpu host -enable-kvm -m 2G \
    -cdrom /var/lib/libvirt/images/virtio-win.iso
```

1. Open the dvd drive and install `virtio-win-guest-tools`

## ~~Step 5b - Load the virtio tablet driver~~

[No need to add the tablet device if we use spice]

Add a device to make the mouse work properly with the new graphics driver (it also prevents the mouse to be grabbed)

```bash
qemu-system-x86_64 \
    -bios /usr/share/ovmf/x64/OVMF_CODE.fd \
    -drive file=/dev/md0,media=disk,format=raw,if=virtio \
    -cpu host -enable-kvm -m 2G \
    -vga virtio -display sdl,gl=on \
    -device virtio-tablet,wheel-axis=true
```

## Step 6 - Boot Windows with disk virtio driver

Perform all the steps in this precise order.

Careful, you may need to touch the boot order after some of the steps because the virtio disk is seen as a different disk.

1. Boot Windows with the virtio disk driver, Windows should look for the driver in the cdrom and figure out it should load it at boot time. If it fails proceed with steps 2-8
   
    ```bash
    qemu-system-x86_64 \
        -bios /usr/share/ovmf/x64/OVMF_CODE.fd \
        -drive file=/dev/md0,media=disk,format=raw,if=virtio \
        -cpu host -enable-kvm -m 2G \
        -cdrom /var/lib/libvirt/images/virtio-win.iso
    ```
    
2. Create a dummy hard drive
   
    ```bash
    qemu-img create -f qcow2 dummy.qcow2 1G
    ```
    
3. Boot the system with the new hard drive using virtio drivers
   
    ```bash
    qemu-system-x86_64 \
        -bios /usr/share/ovmf/x64/OVMF_CODE.fd \
        -drive file=/dev/md0,media=disk,format=raw \
        -cpu host -enable-kvm -m 2G \
        -cdrom /var/lib/libvirt/images/virtio-win.iso \
        -drive file=dummy.qcow2,if=virtio
    ```
    
4. In Windows run `diskmgmt.msc` 
    1. check that the 1G disk is detected
    2. Right click on the disk > Properties and check that the driver is virtio


5. Set Windows to boot in safe mode so that it loads all the driver at boot time
   
    ```bash
    bcdedit /set {current} safeboot minimal
    ```
    
6. Restart the VM with the md0 disk set to use the virtio drivers (keep the dummy disk and the cdrom)
   
    ```bash
    qemu-system-x86_64 \
        -bios /usr/share/ovmf/x64/OVMF_CODE.fd \
        -drive file=/dev/md0,media=disk,format=raw,if=virtio \
        -cpu host -enable-kvm -m 2G \
        -cdrom /var/lib/libvirt/images/virtio-win.iso \
        -drive file=dummy.qcow2,if=virtio
    ```
    
7. Set Windows to boot in normal mode
   
    ```powershell
    bcdedit /deletevalue {current} safeboot
    ```
    
8. Start Windows without the dummy disk but with the cdrom
   
    ```bash
    qemu-system-x86_64 \
        -bios /usr/share/ovmf/x64/OVMF_CODE.fd \
        -drive file=/dev/md0,media=disk,format=raw,if=virtio \
        -cpu host -enable-kvm -m 2G \
        -cdrom /var/lib/libvirt/images/virtio-win.iso
    ```
    
9. Start Windows without the cdrom 
   
    ```bash
    qemu-system-x86_64 \
        -bios /usr/share/ovmf/x64/OVMF_CODE.fd \
        -drive file=/dev/md0,media=disk,format=raw,if=virtio \
        -cpu host -enable-kvm -m 2G
    ```
    

### References

[https://wiki.archlinux.org/title/QEMU#Change_existing_Windows_VM_to_use_virtio](https://wiki.archlinux.org/title/QEMU#Change_existing_Windows_VM_to_use_virtio)

[https://superuser.com/a/1253728](https://superuser.com/a/1253728)

## Step 7 - Load the qxl graphics driver

1. Load the qxl vga driver

```bash
qemu-system-x86_64 \
    -bios /usr/share/ovmf/x64/OVMF_CODE.fd \
    -drive file=/dev/md0,media=disk,format=raw,if=virtio \
    -cpu host -enable-kvm -m 2G \
    -vga qxl -display sdl
```

## ~~Step 7 - Load the virtio graphics driver~~

[The virtio graphics driver aka virgl is only good for OpenGL not for DirectX, not so useful in Windows. It makes the mouse lag with spice]

1. Load the virtio vga driver and add a device to make the mouse work properly with the new graphics driver (it also prevents the mouse to be grabbed)

   ```bash
   qemu-system-x86_64 \
       -bios /usr/share/ovmf/x64/OVMF_CODE.fd \
       -drive file=/dev/md0,media=disk,format=raw,if=virtio \
       -cpu host -enable-kvm -m 2G \
       -device virtio-tablet,wheel-axis=true \
       -vga virtio -display sdl
   
   ```

2. In Windows open Device Manager and check that the `Display Adapter` is a `Virtio GPU`

3. Enable hardware acceleration

   ```bash
   qemu-system-x86_64 \
       -bios /usr/share/ovmf/x64/OVMF_CODE.fd \
       -drive file=/dev/md0,media=disk,format=raw,if=virtio \
       -cpu host -enable-kvm -m 2G \
       -device virtio-tablet,wheel-axis=true \
       -vga virtio -display sdl,gl=on
   ```

4. Check that the hardware acceleration is working by running `dxdiag` and looking at the second page

### References

[https://wiki.archlinux.org/title/QEMU/Guest_graphics_acceleration](https://wiki.archlinux.org/title/QEMU/Guest_graphics_acceleration)

## Step 8 - Load the networking virtio driver

```bash
qemu-system-x86_64 \
    -bios /usr/share/ovmf/x64/OVMF_CODE.fd \
    -drive file=/dev/md0,media=disk,format=raw,if=virtio \
    -cpu host -enable-kvm -m 2G \
    -vga virtio -display sdl,gl=on \
    -device virtio-tablet,wheel-axis=true \
    -nic user,model=virtio-net-pci
```

## Step 9 - Optimize the performance

1. Tweaks:
    1. add cpu tweaks for Windows hypervisor
    2. define the cpu topology, smp 1 socket, 2 cores, 2 threads each (host system has a quad core cpu)
    3. extend the memory to 6 GB (host system has 16 GB of RAM)
    4. add `virtio-balloon` to reclaim memory from the guest

    ```bash
    qemu-system-x86_64 \
        -enable-kvm \
        -bios /usr/share/ovmf/x64/OVMF_CODE.fd \
        -drive file=/dev/md0,media=disk,format=raw,if=virtio,aio=native,cache=none \
        -cpu host,hv_relaxed,hv_spinlocks=0x1fff,hv_vapic,hv_time \
        -smp 4,sockets=1,cores=2,threads=2 \
        -m 6G \
        -vga qxl -display sdl,gl=on \
        -nic user,model=virtio-net-pci \
        -device virtio-balloon
    ```

2. Enable huge pages (follow this [guide](https://wiki.archlinux.org/title/KVM#Enabling_huge_pages) to compute the number of pages and allow them to be used by the virtual machine)
   
    ```bash
    qemu-system-x86_64 \
        -enable-kvm \
        -bios /usr/share/ovmf/x64/OVMF_CODE.fd \
        -drive file=/dev/md0,media=disk,format=raw,if=virtio,aio=native,cache=none \
        -cpu host,hv_relaxed,hv_spinlocks=0x1fff,hv_vapic,hv_time \
        -smp 4,sockets=1,cores=2,threads=2 \
        -m 6G \
        -mem-path /dev/hugepages \
        -vga virtio -display sdl,gl=on \
        -device virtio-tablet,wheel-axis=true \
        -nic user,model=virtio-net-pci \
        -device virtio-balloon
    ```
    
    1. Set the number of huge pages to use before the vm starts
       
        ```bash
        su -c "echo 3080 > /proc/sys/vm/nr_hugepages"
        ```
        
    2. Start the virtual machines and check that the `HugePages_Free` is less than `HugePages_Total` to ensure that hugepages are in use by the VM
       
        ```bash
        grep HugePages /proc/meminfo
        ```
        
    3. Release the huge pages when the vm terminates
       
        ```bash
        su -c "echo 0 > /proc/sys/vm/nr_hugepages"
        ```
        

3. Enable tap networking
   - TODO (really needed? my network performance are already very good)

### References

[https://wiki.archlinux.org/title/QEMU#Improve_virtual_machine_performance](https://wiki.archlinux.org/title/QEMU#Improve_virtual_machine_performance)

[http://kvmonz.blogspot.com/p/knowledge-disk-performance-hints-tips.html](http://kvmonz.blogspot.com/p/knowledge-disk-performance-hints-tips.html)

[https://wiki.archlinux.org/title/KVM#Enabling_huge_pages](https://wiki.archlinux.org/title/KVM#Enabling_huge_pages)

## Step 10 - Configure a virt-manager VM

1. [Wizard step 1] Select `Import existing disk image`
2. [Wizard step 2]
    1. Existing storage path: `/dev/md0`
    2. Select the operating systems: `win10`
3. [Wizard step 3] Choose the amount of memory and cpus to allocate
4. [Wizard step 4] Check `Customize configuration before install`
5. Overview
    1. Firmware: OVMF_CODE.fd
6. CPUs
    1. Topology 1,2,2 (host system has a quad core cpu)
7. Disk
    1. Remove the Sata Disk
    2. Add a Storage with `Device Type` equal to `virtio`
8. NIC
    - Device model: virtio
9. Tablet, remove this
10. Video QXL
    - Do not use virtio
11. Ensure all qemu arguments for tweaking performance are present in libvirt
    1. [auto] Disk parameters `aio=native,cache=none`
       
        ```xml
        <driver name="qemu" type="raw" cache="none" io="native"/>
        ```
        
    2. [auto] Hypervisor enlightment `hv_relaxed,hv_spinlocks=0x1fff,hv_vapic,hv_time` maps to:
       
        ```xml
        <features>
         <hyperv>
           <relaxed state='on'/>
           <vapic state='on'/>
           <spinlocks state='on' retries='8191'/>
         </hyperv>
        <features/>
        
        <clock ...>
         <timer name='hypervclock' present='yes'/>
        </clock>
        ```
        
    3. [auto] smp, we defined the topology in a previous step
    4. Huge pages. Execute `sudo virsh edit win10` and add the following block before the closing `</domain>` tag
       
        ```xml
        <memoryBacking>
          <hugepages/>
        </memoryBacking>
        ```
        
        Start the virtual machines and check that the free huge pages are less than the total hugepages
        
        ```powershell
        grep HugePages /proc/meminfo
        ```
        

### References

[https://wiki.libvirt.org/page/FAQ#What_is_the_difference_between_qemu:.2F.2F.2Fsystem_and_qemu:.2F.2F.2Fsession.3F_Which_one_should_I_use.3F](https://wiki.libvirt.org/page/FAQ#What_is_the_difference_between_qemu:.2F.2F.2Fsystem_and_qemu:.2F.2F.2Fsession.3F_Which_one_should_I_use.3F)

[https://www.linuxquestions.org/questions/linux-newbie-8/how-to-add-flags-in-libvirt-4175617668/#post5781729](https://www.linuxquestions.org/questions/linux-newbie-8/how-to-add-flags-in-libvirt-4175617668/#post5781729)

## Step 11 - Make libvirt automatically start/stop the md0 disk

1. Create the file `/etc/libvirt/hooks/qemu` (assuming the name of the VM is `win10`)
   
    ```bash
    #!/usr/bin/env bash
    EFIDIR=/media/dataHD/linux/kvm/
    if [[ $1 == "win10" ]]; then
      if [[ $2 == "prepare" ]]; then
        echo 3080 > /proc/sys/vm/nr_hugepages
        "${EFIDIR}/start_md0"
      elif [[ $2 == "release" ]]; then
        "${EFIDIR}/stop_md0"
        echo 0 > /proc/sys/vm/nr_hugepages
      fi
    fi
    ```
    
2. Make it executable
   
    ```bash
    sudo chmod u+x /etc/libvirt/hooks/qemu
    ```
    

If everything worked as expected it should now be possible to start the virtual machine from Virt Manager, and it should build the disk and allocate the hugepages memory.