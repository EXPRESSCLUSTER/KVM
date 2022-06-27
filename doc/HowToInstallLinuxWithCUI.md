# How to Install Linux with CUI
1. Create image file as below.
   ```sh
   qemu-img create -f qcow2 rhel8.qcow2 60G
   ```
2. Run the following command to install Linux. 
   ```sh
   virt-install --name rhel8 --hvm --virt-type kvm --ram 1024 --vcpus 1 --arch x86_64 --os-type linux --disk /vm/rhel8/rhel8.qcow2 --network bridge=virbr0 --graphics none --serial pty --console pty --location rhel-8.2-x86_64-dvd.iso --extra-args "console=ttyS0"
   ```
   - If you can find your OS version, you can set it with ```--os-variant``` option.
     ```sh
     osinfo-query os
     ```
     ```sh
      Short ID             | Name                                               | Version  | ID
     ----------------------+----------------------------------------------------+----------+-----------------------------------------
      alpinelinux3.5       | Alpine Linux 3.5                                   | 3.5      | http://alpinelinux.org/alpinelinux/3.5
      :
      rhel7.8              | Red Hat Enterprise Linux 7.8                       | 7.8      | http://redhat.com/rhel/7.8
      :
     ```
3. You can get installation CUI.

# Appendix
## Attach/Detach Disk
- Attach
  ```sh
  virsh attach-disk <guest OS name> <qcow2 file> hdb --type disk --config --subdriver qcow2 --driver qemu
  ```
- Detach
  ```sh
  virsh detach-disk --domain <guest OS name> --target hdb --config
  ```
## Enable Shared Disk
1. Shutdown a guest OS.
1. Attach a disk.
1. Run the following command and add cache and shareable.
   ```sh
   virsh edit <guest OS name>
   ```
   ```xml
       <disk type='file' device='disk'>
         <driver name='qemu' type='raw' cache='none'/>
         <source file='/vm2/ml8-101/sd1.img'/>
         <target dev='sdd' bus='scsi'/>
         <shareable/>
         <address type='drive' controller='0' bus='0' target='0' unit='3'/>
       </disk>
   ```
## Attach/Detach DVD-ROM
- Attach
  ```sh
  virsh attach-disk <guest OS name> <ISO file name> hdb --type cdrom --mode readonly --targetbus ide
  ```
- Detach
  ```sh
  virsh detach-disk --domain <guest OS name> --target hdb --config
  ```
## How to Delete VM
1. Undefine the VM.
   ```sh
   virsh undefine <domain name>
   ```
1. Check pool name and device name.
   ```sh
   virsh pool-list --all
   virsh vol-list <pool name>
   ```
1. Delete the device.
   ```sh
   virsh vol-delete --pool <pool name> <device name>
   ```
1. Delete the pool.
   ```sh
   virsh pool-destroy <pool name>
   virsh pool-delete <pool name>
   ```
1. Undefine the pool.
   ```sh
   virsh pool-undefine <pool name>
   ```  