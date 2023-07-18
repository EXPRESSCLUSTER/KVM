# Instal KVM on Ubuntu
- OS: Ubuntu Server 22.04.2 LTS

## Install KVM
1. Install the following packages.
   ```sh
   sudo apt -y install qemu-kvm libvirt-daemon-system libvirt-daemon virtinst bridge-utils libosinfo-bin
   ```

## Edit Permission
1. Edit /etc/libvirt/qemu.conf as below.
   ```
    :
   user = <your user name>
    :
   group = "libvirt"
    :
   dynamic_ownership = 1
    :
   ```
1. Edit /etc/libvirt/libvirtd.conf as below.
   ```
    :
   unix_sock_group = "libvirt"
    :
   unix_sock_ro_perms = "0777"
    :
   unix_sock_rw_perms = "0770"
    :
   auth_unix_ro = "none"
    :
   auth_unix_rw = "none"   
   ```
1. Run the following command to add your user to kvm and libvirt.
   ```sh
   sudo uermod -aG kvm <your user name>
   ```
   ```sh
   sudo uermod -aG libvirt <your user name>
   ```
1. Restart libvirtd.
   ```sh
   sudo systemctl restart libvirtd
   ```

## Install Virtual Machine
1. Save qcow2 file and iso file on your user directory.
1. Run the following command to intall VM.
   ```sh
   virt-install \
   --name alma9-20 \
   --ram 2048 \
   --vcpus 1 \
   --arch x86_64 \
   --disk /home/<your user name>/vm/alma9-20/alma9-20.qcow2 \
   --network bridge=virbr0 \
   --graphics none \
   --serial pty \
   --console pty \
   --location /home/<your user name>/work/Linux/CD/AlmaLinux-9.0-x86_64-dvd.iso \
   --extra-args "console=ttyS0"
   ```