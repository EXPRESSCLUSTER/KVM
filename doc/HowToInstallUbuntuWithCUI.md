# How to Install Ubuntu with CUI

## How to Install Ubuntu
1. Install cloud-utils.
1. Download a cloud image file.
   - https://cloud-images.ubuntu.com
1. Convert the file.
   ```sh
   qemu-img convert -f qcow2 -O qcow2 jammy-server-cloudimg-amd64-disk-kvm.img ubuntu22-disk.qcow2
   ```
1. Resize disk if you want.
   ```sh
   qemu-img resize ubuntu22-disk.qcow2 60G
   ```
1. Create a cloud-init.cfg file as below.
   ```sh
   #cloud-config
   system_info:
     default_user:
       name: fukunaga
       home: /home/fukunaga
   
   password: password
   chpasswd: { expire: False }
   hostname: ubuntu22-61
   
   # configure sshd to allow users logging in using password
   # rather than just keys
   ssh_pwauth: True
   ```
1. Create the ISO file from the cloud-init.cfg file.
   ```sh
   cloud-localds cloud-init.iso cloud-init.cfg
   ```
1. Install Ubuntu.
   ```sh
   virt-install \
   --name ubuntu22-61 \
   --memory 1024 \
   --disk /vm2/ubuntu22-61/ubuntu22-disk.qcow2,device=disk,bus=virtio \
   --disk /vm2/ubuntu22-61/cloud-init.iso,device=cdrom \
   --os-type linux \
   --os-variant ubuntu20.04 \
   --virt-type kvm \
   --graphics none \
   --network bridge=virbr0 \
   --import
   ```
   - In my environment (CentOS 7), osinfo-query returns up to Ubuntu 20.04 so that I set ubuntu20.04 for --os-variant.
     ```sh
     osinfo-query os|grep ubuntu
      ubuntu10.04          | Ubuntu 10.04 LTS                                   | 10.04    | http://ubuntu.com/ubuntu/10.04
      ubuntu10.10          | Ubuntu 10.10                                       | 10.10    | http://ubuntu.com/ubuntu/10.10
     (snip)
      ubuntu20.04          | Ubuntu 20.04                                       | 20.04    | http://ubuntu.com/ubuntu/20.04
     ```

## Link
- https://blog.programster.org/create-ubuntu-22-kvm-guest-from-cloud
